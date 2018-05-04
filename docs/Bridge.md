## 桥接模式（Bridge）  

将抽象化与实现化解耦，使得二者可以独立变化。  

涉及的角色有：  
1. **Abstraction** 抽象化角色，抽象化给出的定义，并保存一个对实现化对象的引用。  

2. **RefinedAbstraction** 修正抽象化角色，扩展抽象化角色，改变和修正父类对抽象化的定义。  

3. **Implementor** 实现化角色，这个角色给出实现化角色的接口，但不给出具体的实现。必须指出的是，这个接口不一定和抽象化角色的接口定义相同，实际上，这两个接口可以非常不一样。实现化角色应当只给出底层操作，而抽象化角色应当只给出基于底层操作的更高一层的操作。  

4. **ConcreteImplementor** 具体实现化角色，这个角色给出实现化角色接口的具体实现。  

优点：  
1. 分离抽象和实现部分。分离抽象部分和实现部分，从而极大地提供了系统的灵活性。让抽象部分和实现部分独立出来，分别定义接口，这有助于对系统进行分层，从而产生更好的结构化的系统。  
2. 更好的扩展性。使得抽象部分和实现部分可以分别独立地扩展，而不会相互影响，从而大大提高了系统的可扩展性。  

抽象化角色、修正抽象化角色  
```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public abstract class Abstraction {

    protected Implementor impl;
	
    public Abstraction(Implementor impl){  
        this.impl = impl;  
    }
	
    public void operation(){  
        impl.operationImpl();  
    }
	
}


class RefinedAbstraction extends Abstraction {

    public RefinedAbstraction(Implementor impl) {
	super(impl);
    }
	
    public void otherOperation(){}
	
}
```

实现化角色、具体实现化角色  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public abstract class Implementor {

    public abstract void operationImpl();
	
}

class ConcreteImplementor extends Implementor {

    @Override
    public void operationImpl() {
	System.out.println("具体操作");
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
	Abstraction ab = new RefinedAbstraction(new ConcreteImplementor());
	ab.operation();
    }

}
```

**拓展**  

[《JAVA与模式》26天系列—第14天—桥梁模式](https://blog.csdn.net/m13666368773/article/details/7694322)

