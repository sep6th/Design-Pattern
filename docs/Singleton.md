## 单例模式（Singleton）

单例类只能对外提供一个由自身创建的唯一实例。

**优点：**  
1. 对于频繁使用的对象，可以省略new操作花费的时间，这对于那些重量级对象而言，节省了非常可观的一笔系统开销；  
2. 由于new操作的次数减少，因而对系统内存的使用频率也会降低，这将减轻GC压力，缩短GC停顿时间。  

**饿汉式**  

饿汉式是典型的**空间换时间**  ，当类装载的时候就会创建类的实例。相比懒汉式，每次调用的时候，就不需要再判断，节省了运行时间。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class EagerSingleton {
	
	private static EagerSingleton instance = new EagerSingleton();
	
	private EagerSingleton() {}
	
	public static EagerSingleton getInstance() {
		return EagerSingleton.instance;
	}
	
}
```
**懒汉式**  

懒汉式是典型的**时间换空间**  ,就是每次获取实例都会进行判断，看是否需要创建实例，浪费判断的时间。当然，如果一直没有人使用的话，那就不会创建实例，则节约内存空间。


```java
class LazySingleton {
	
	private static LazySingleton instance = null;
	
	private  LazySingleton() {}
	
	public static LazySingleton getInstance() {
		if(instance == null){
			instance = new LazySingleton();
		}
		return instance;
	}
	
}
```
但是，像这样毫无线程安全保护的类，如果我们把它放入多线程的环境下，肯定就会出现问题，如何解决？我们首先会想到对getInstance方法加synchronized关键字。  

```java
	public static synchronized LazySingleton getInstance() {
		if(instance == null){
			instance = new LazySingleton();
		}
		return instance;
	}

```
synchronized关键字锁住的是这个对象，这样的用法，在性能上会有所下降，因为每次调用getInstance()，都要对对象上锁，事实上，只有在第一次创建对象的时候需要加锁，之后就不需要了，所以，这个地方需要改进。  


```java
	public static LazySingleton getInstance() {
		if(instance == null){
			synchronized (instance) {
				if(instance == null){
					instance = new LazySingleton();
				}
			}
		}
		return instance;
	}
```

在Java指令中创建对象和赋值操作是分开进行的，也就是说instance = new Singleton();语句是分两步执行的，但是JVM并不保证这两个操作的先后顺序。  

eg.A、B两个线程：  

a> A、B线程同时进入了第一个if判断

b> A首先进入synchronized块，由于instance为null，所以它执行instance = new Singleton();  

c> 由于JVM内部的优化机制，JVM先画出了一些分配给Singleton实例的空白内存，并赋值给instance成员（注意此时JVM没有开始初始化这个实例），然后A离开了synchronized块。  

d> B进入synchronized块，由于instance此时不是null，因此它马上离开了synchronized块并将结果返回给调用该方法的程序。  

e> 此时B线程打算使用Singleton实例，却发现它没有被初始化，于是错误发生了。  

接下来，使用Java的类级内部类和多线程缺省同步锁的知识，巧妙地同时实现了延迟加载和线程安全。

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Singleton {  
    
    private Singleton(){}  
    /** 
     * 类级的内部类，也就是静态的成员式内部类，该内部类的实例与外部类的实例 
     * 没有绑定关系，而且只有被调用到时才会装载，从而实现了延迟加载。 
     */  
    private static class SingletonHolder{  
        /** 
         * 静态初始化器，由JVM来保证线程安全 
         */  
        private static Singleton instance = new Singleton();  
    }  
      
    public static Singleton getInstance(){  
        return SingletonHolder.instance;  
    }  
}
```
当getInstance方法第一次被调用的时候，它第一次读取SingletonHolder.instance，导致SingletonHolder类得到初始化；而这个类在装载并被初始化的时候，会初始化它的静态域，从而创建Singleton的实例，由于是静态的域，因此只会在虚拟机装载类的时候初始化一次，并由虚拟机来保证它的线程安全性。这种方式的优势在于，getInstance方法并没有被同步，并且只是执行一个域的访问，因此延迟初始化并没有增加任何访问成本。

**拓展——类级内部类**  
 
有static修饰的成员式内部类被称为类级内部类；没有static修饰的成员式内部类被称为对象级内部类。  
类级内部类相当于其外部类的static成分，它的对象与外部类对象间不存在依赖关系，因此可直接创建。而对象级内部类的实例，是绑定在外部对象实例中的。  
类级内部类中，可以定义静态的方法。在静态方法中只能够引用外部类中的静态成员方法或者成员变量。  
类级内部类相当于其外部类的成员，只有在第一次被使用的时候才被会装载。  

**拓展——多线程缺省同步锁**  

在多线程开发中，为了解决并发问题，主要是通过使用synchronized来加互斥锁进行同步控制。但是在某些情况中，JVM已经隐含地为您执行了同步，这些情况下就不用自己再来进行同步控制了。  
这些情况包括：  
1. 由静态初始化器（在静态字段上或static{}块中的初始化器）初始化数据时。  
2. 访问final字段时。  
3. 在创建线程之前创建对象时。  
4. 线程可以看见它将要处理的对象时。  

**单例和枚举**  

按照《高效Java 第二版》中的说法：单元素的枚举类型已经成为实现Singleton的最佳方法。用枚举来实现单例非常简单，只需要编写一个包含单个元素的枚举类型即可。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public enum EnumSingleton {  
    /** 
     * 定义一个枚举的元素，它就代表了Singleton的一个实例。 
     */  
    uniqueInstance;  
      
    /** 
     * 单例可以有自己的操作 
     */  
    public void singletonOperation(){  
        //功能处理  
    	System.out.println("others operation");
    } 
    
    public static void main(String[] args) {
    	EnumSingleton es = EnumSingleton.uniqueInstance;
    	es.singletonOperation();
	}
} 
```
使用枚举来实现单实例控制会更加简洁，而且无偿地提供了序列化机制，并由JVM从根本上提供保障，绝对防止多次实例化，是更简洁、高效、安全的实现单例的方式。
