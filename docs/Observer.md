## 观察者模式（Observer）

观察者模式是对象的行为模式，又叫发布-订阅(Publish/Subscribe)模式、模型-视图(Model/View)模式、源-监听器(Source/Listener)模式或从属者(Dependents)模式。  

观察者模式定义了一种一对多的依赖关系，让多个观察者对象同时监听某一个主题对象。这个主题对象在状态上发生变化时，会通知所有观察者对象，使它们能够自动更新自己。  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Observer {
	
    void update();
	
}

class Observer1 implements Observer {

    @Override
    public void update() {
	System.out.println("Observer1 has received!");
    }
	
}

class Observer2 implements Observer {

    @Override
    public void update() {
	System.out.println("Observer2 has received!");
    }
	
}
```


```java
import java.util.Enumeration;
import java.util.Vector;

/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Subject {

    /*增加观察者*/  
    void add(Observer observer);  
      
    /*删除观察者*/  
    void del(Observer observer);  
      
    /*通知所有的观察者*/  
    void notifyObservers();  
      
    /*自身的操作*/  
    void operation();
	
}

abstract class AbstractSubject implements Subject {
	
    private Vector<Observer> vector = new Vector<Observer>();
	
    @Override  
    public void add(Observer observer) {  
        vector.add(observer);  
    }  
  
    @Override  
    public void del(Observer observer) {  
        vector.remove(observer);  
    }  
  
    @Override  
    public void notifyObservers() {  
        Enumeration<Observer> enumo = vector.elements();  
        while(enumo.hasMoreElements()){  
            enumo.nextElement().update();  
        }  
    }
	
}

class MySubject extends AbstractSubject {  
	  
    @Override  
    public void operation() {  
        System.out.println("update self!");  
        notifyObservers();  
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
	Subject sub = new MySubject();  
        sub.add(new Observer1());  
        sub.add(new Observer2());  
        sub.operation(); 
    }

}
```

Console
```java
update self!
Observer1 has received!
Observer2 has received!
```






