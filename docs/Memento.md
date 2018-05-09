## 备忘录模式（Memento）

备忘录模式又叫做快照模式(Snapshot Pattern)或Token模式，是对象的行为模式。  

备忘录对象是一个用来存储另外一个对象内部状态的快照的对象。备忘录模式的用意是在不破坏封装的条件下，将一个对象的状态捕捉(Capture)住，并外部化，存储起来，从而可以在将来合适的时候把这个对象还原到存储起来的状态。备忘录模式常常与命令模式和迭代子模式一同使用。  

涉及的角色:  

1. **备忘录(Memento)角色**  
a.将发起人（Originator）对象的内战状态存储起来。备忘录可以根据发起人对象的判断来决定存储多少发起人（Originator）对象的内部状态。    
b.备忘录可以保护其内容不被发起人（Originator）对象之外的任何对象所读取。  

备忘录有两个等效的接口：  
窄接口：负责人（Caretaker）对象（和其他除发起人对象之外的任何对象）看到的是备忘录的窄接口(narrow interface)，这个窄接口只允许它把备忘录对象传给其他的对象。  
宽接口：与负责人对象看到的窄接口相反的是，发起人对象可以看到一个宽接口(wide interface)，这个宽接口允许它读取所有的数据，以便根据这些数据恢复这个发起人对象的内部状态。  

2. **发起人（Originator）角色**  
a.创建一个含有当前的内部状态的备忘录对象。  
b.使用备忘录对象存储其内部状态。  

3. **负责人（Caretaker）角色**  
a.负责保存备忘录对象。  
b.不检查备忘录对象的内容。  


**“黑箱”备忘录模式的实现 **   
备忘录角色对发起人（Originator）角色对象提供一个宽接口，而为其他对象提供一个窄接口。这样的实现叫做“黑箱实现”。  
在JAVA语言中，实现双重接口的办法就是将备忘录角色类设计成发起人角色类的内部成员类。  


发起人角色类Originator中定义了一个内部的Memento类。由于此Memento类的全部接口都是私有的，因此只有它自己和发起人类可以调用。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Originator {
	
    private String state;
	
    public void setState(String state) {  
        this.state = state;  
        System.out.println("赋值状态：" + state);  
    }
	
    /** 
     * 工厂方法，返还一个新的备忘录对象 
     */  
    public MementoIF createMemento() {  
        return new Memento(state);  
    }
	
    /** 
     * 发起人恢复到备忘录对象记录的状态 
     */
    public void restoreMemento(MementoIF memento) {  
        this.setState(((Memento) memento).getState());  
    }  
  
    private class Memento implements MementoIF {  
  
        private String state;  
  
        /** 
         * 构造方法 
         */  
        private Memento(String state) {  
            this.state = state;  
        }  
  
        private String getState() {  
            return state;  
        }  
  
        private void setState(String state) {  
            this.state = state;  
        }  
    }
	
	
}
```
窄接口MementoIF，这是一个标识接口，因此它没有定义出任何的方法。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface MementoIF {}
```

负责人角色类Caretaker能够得到的备忘录对象是以MementoIF为接口的，由于这个接口仅仅是一个标识接口，因此负责人角色不可能改变这个备忘录对象的内容。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Caretaker {  
	  
    private MementoIF memento;  
  
    /** 
     * 备忘录取值方法 
     */  
    public MementoIF retrieveMemento() {  
        return memento;  
    }  
  
    /** 
     * 备忘录赋值方法 
     */  
    public void saveMemento(MementoIF memento) {  
        this.memento = memento;  
    }  
}
```

测试  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Test {

    public static void main(String[] args) {
		
	Originator o = new Originator();  
        Caretaker c = new Caretaker();  
        // 改变负责人对象的状态  
        o.setState("On");  
        // 创建备忘录对象，并将发起人对象的状态存储起来  
        c.saveMemento(o.createMemento());  
        // 修改发起人对象的状态  
        o.setState("Off");  
        // 恢复发起人对象的状态  
        o.restoreMemento(c.retrieveMemento());
		
    }

}

```
**“自述历史”模式**  

所谓“自述历史”模式(History-On-Self Pattern)实际上就是备忘录模式的一个变种。在备忘录模式中，发起人(Originator)角色、负责人(Caretaker)角色和备忘录(Memento)角色都是独立的角色。虽然在实现上备忘录类可以成为发起人类的内部成员类，但是备忘录类仍然保持作为一个角色的独立意义。在“自述历史”模式里面，发起人角色自己兼任负责人角色。  

1. 备忘录角色有如下责任：  

a.将发起人（Originator）对象的内部状态存储起来。  
b.备忘录可以保护其内容不被发起人（Originator）对象之外的任何对象所读取。  

2. 发起人角色有如下责任：  

a.创建一个含有它当前的内部状态的备忘录对象。  
b.使用备忘录对象存储其内部状态。  

3. 客户端角色有负责保存备忘录对象的责任。  

窄接口MementoIF，这是一个标识接口，因此它没有定义出任何的方法。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface MementoIF {}
```  

发起人角色同时还兼任负责人角色，也就是说它自己负责保持自己的备忘录对象。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Originator {

    public String state;

    /**
     * 改变状态
     */
    public void changeState(String state) {
	this.state = state;
	System.out.println("状态改变为：" + state);
    }

    /**
     * 工厂方法，返还一个新的备忘录对象
     */
    public Memento createMemento() {
	return new Memento(this);
    }

    /**
     * 将发起人恢复到备忘录对象所记录的状态上
     */
    public void restoreMemento(MementoIF memento) {
        Memento m = (Memento) memento;
	changeState(m.state);
    }

    private class Memento implements MementoIF {

	private String state;

	/**
         * 构造方法
         */
	private Memento(Originator o) {
		this.state = o.state;
	}

	private String getState() {
		return state;
	}

    }
    
}

```

客户端角色类  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Client {  
  
    public static void main(String[] args) {  
        Originator o = new Originator();  
        // 修改状态  
        o.changeState("state 0");  
        // 创建备忘录  
        MementoIF memento = o.createMemento();  
        // 修改状态  
        o.changeState("state 1");  
        // 按照备忘录恢复对象的状态  
        o.restoreMemento(memento);  
    }  
  
} 
```

**拓展**  

[《JAVA与模式》26天系列—第22天—备忘录模式](https://blog.csdn.net/m13666368773/article/details/7709156)

