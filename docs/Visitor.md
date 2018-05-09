## 访问者模式（Visitor）

访问者模式是对象的行为模式。访问者模式的目的是封装一些施加于某种数据结构元素之上的操作。一旦这些操作需要修改的话，接受这个操作的数据结构则可以保持不变。  
简单来说，访问者模式就是一种分离对象数据结构与行为的方法，通过这种分离，可达到为一个被访问者动态添加新的操作而无需做其它的修改的效果。  

**优点**  

1. 好的扩展性  
　　能够在不修改对象结构中的元素的情况下，为对象结构中的元素添加新的功能。  

2. 好的复用性  
　　可以通过访问者来定义整个对象结构通用的功能，从而提高复用程度。  

3. 分离无关行为  
　　可以通过访问者来分离无关的行为，把相关的行为封装在一起，构成一个访问者，这样每一个访问者的功能都比较单一。  

**缺点**  

1. 对象结构变化很困难  
　　不适用于对象结构中的类经常变化的情况，因为对象结构发生了改变，访问者的接口和访问者的实现都要发生相应的改变，代价太高。  

2. 破坏封装
　　访问者模式通常需要对象结构开放内部数据给访问者和ObjectStructrue，这破坏了对象的封装性。  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Visitor {
	
    void visit(Subject sub);
	
}

class VisitorImpl implements Visitor {

    @Override
    public void visit(Subject sub) {
	System.out.println("visit the subject："+sub.getSubject()); 
    }

}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Subject {

    void accept(Visitor visitor);  
	
    String getSubject();
	
}

class SubjectImpl implements Subject {

    @Override
    public void accept(Visitor visitor) {
	visitor.visit(this);
    }

    @Override
    public String getSubject() {
	return "SubjectImpl";
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
		
	Visitor visitor = new VisitorImpl();  
        Subject sub = new SubjectImpl();  
        sub.accept(visitor);
        
    }

}
```







