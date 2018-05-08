## 责任链模式（Chain of Responsibility）

责任链模式是一种对象的行为模式。在责任链模式里，很多对象由每一个对象对其下家的引用而连接起来形成一条链。请求在这个链上传递，直到链上的某一个对象决定处理此请求。发出这个请求的客户端并不知道链上的哪一个对象最终处理这个请求，这使得系统可以在不影响客户端的情况下动态地重新组织和分配责任。  



```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

interface IHandler {
	
    void operator();
	
}

abstract class AbstractHandler {  
    
    private IHandler handler;  
  
    public IHandler getHandler() {  
        return handler;  
    }  
  
    public void setHandler(IHandler handler) {  
        this.handler = handler;  
    }  
      
} 

class HandlerImpl extends AbstractHandler implements IHandler {

    private String name;
	
    public HandlerImpl(String name) {
	this.name = name;
    }



    @Override
    public void operator() {
	System.out.println(name+" deal!");  
        if(getHandler()!=null){  
            getHandler().operator();  
        }  
    }  
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Test {

    public static void main(String[] args) {
		
	HandlerImpl h1 = new HandlerImpl("h1");
	HandlerImpl h2 = new HandlerImpl("h2");
	HandlerImpl h3 = new HandlerImpl("h3");
		
	h1.setHandler(h2);
	h2.setHandler(h3);
	h1.operator();

    }

}
```

**拓展**

[《JAVA与模式》26天系列—第20天—责任链模式](https://blog.csdn.net/m13666368773/article/details/7702368)
