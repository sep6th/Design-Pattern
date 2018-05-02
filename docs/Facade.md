## 外观模式（Facade）  

外观模式，也称门面模式。是对象的结构模式，外部与一个子系统的通信必须通过一个统一的门面对象进行。门面模式提供一个高层次的接口，使得子系统更易于使用。为了好理解下文称门面模式。  

门面模式的优点： 
 
1. 松散耦合  
门面模式松散了客户端与子系统的耦合关系，让子系统内部的模块能更容易扩展和维护。  

2. 简单易用  
门面模式让子系统更加易用，客户端不再需要了解子系统内部的实现，也不需要跟众多子系统内部的模块进行交互，只需要跟门面类交互就可以了。  

3. 更好的划分访问层次  
通过合理使用Facade，可以帮助我们更好地划分访问的层次。有些方法是对系统外的，有些方法是系统内部使用的。把需要暴露给外部的功能集中到门面中，这样既方便客户端使用，也很好地隐藏了内部的细节。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class ModulA {
	
    public void start(){
	System.out.println("ModulA");
    }
	
    /* ...与其他模块交叉调用的方法 */
	
}

class ModulB {
	
    public void start(){
	System.out.println("ModulB");
    }
	
    /* ...与其他模块交叉调用的方法 */
	
}

public class Facade {
	
    ModulA a = new ModulA();
    ModulB b = new ModulB();
	
    public void methodA(){
	a.start();
    }
	
    public void methodB(){
	b.start();
    }
	
    public static void main(String[] args) {
	Facade f = new Facade();
	f.methodA();
	f.methodB();
    }
	
}
```

