## 原型模式（Prototype）  

将一个对象作为原型，对其进行复制、克隆，产生一个和原对象类似的新对象。原型模式虽然是创建型的模式，但是与工厂模式没有关系。  

如何实现？只需要实现Cloneable接口，覆写clone方法。
```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Prototype implements Cloneable {

	/** 
     * 实现Cloneable接口，覆写clone方法
     * Cloneable接口是个空接口，可以任意定义实现类的方法名
     * @return 从自身克隆出来的对象 
	 * @throws CloneNotSupportedException 
     */
	public Prototype clone() throws CloneNotSupportedException {
		Prototype prototype = (Prototype) super.clone();
		return prototype;
	}  
	
}
```
**对象深、浅复制**  

浅复制：将一个对象复制后，基本数据类型的变量都会重新创建，而引用类型，指向的还是原对象所指向的。  

深复制：将一个对象复制后，不论是基本数据类型还有引用类型，都是重新创建的。简单来说，就是深复制进行了完全彻底的复制，而浅复制不彻底。注意：深复制需要对象及对象里的引用类型序列化，  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Prototype implements Cloneable, Serializable {  
  
    private static final long serialVersionUID = 1L;  
    private int num;  
    private SerializableObject obj;
    
    public Prototype(int num, SerializableObject obj) {
		super();
		this.num = num;
		this.obj = obj;
	}

	public int getNum() {
		return num;
	}

	public SerializableObject getObj() {
		return obj;
	}

    /* 浅复制 */  
    public Object shallowClone() throws CloneNotSupportedException {  
        Prototype proto = (Prototype) super.clone();  
        return proto;  
    }  
  
    /* 深复制 */  
    public Object deepClone() throws IOException, ClassNotFoundException {  
  
        /* 写入当前对象的二进制流 */  
        ByteArrayOutputStream baos = new ByteArrayOutputStream();  
        ObjectOutputStream oos = new ObjectOutputStream(baos);  
        oos.writeObject(this);  
  
        /* 读出二进制流产生的新对象 */  
        ByteArrayInputStream bais = new ByteArrayInputStream(baos.toByteArray());  
        ObjectInputStream ois = new ObjectInputStream(bais);  
        return ois.readObject();  
    }  

    public static void main(String[] args) {
    	Prototype p = new Prototype(6, new SerializableObject());
    	try {
    		/* 浅复制 */
    		Prototype p1 = (Prototype) p.shallowClone();
    		
    		System.out.println(p.hashCode());		//366712642
    		System.out.println(p1.hashCode());		//1829164700
    		/* 引用类型，指向的还是原对象所指向的，hashCode一致 */
    		System.out.println(p.getObj().hashCode());	//2018699554
    		System.out.println(p1.getObj().hashCode());	//2018699554
    		
    		/* 深复制 */
    		Prototype p2 = (Prototype) p.deepClone();
    		
    		System.out.println(p.hashCode());		//366712642
    		System.out.println(p2.hashCode());		//1283928880
    		/* 引用类型，指向的还是原对象所指向的，hashCode不一致 */
    		System.out.println(p.getObj().hashCode());	//2018699554
    		System.out.println(p2.getObj().hashCode());	//295530567
    		
		} catch (CloneNotSupportedException e) {
			System.out.println("浅复制失败！");
		} catch (ClassNotFoundException e) {
			System.out.println("深复制失败！");
		} catch (IOException e) {
			System.out.println("深复制失败！");
		}
	}
  
}  

class SerializableObject implements Serializable {  
    private static final long serialVersionUID = 1L;  
}
```  

**扩展**  

[《JAVA与模式》26天系列—第6天—原型模式](https://blog.csdn.net/m13666368773/article/details/7690559)
