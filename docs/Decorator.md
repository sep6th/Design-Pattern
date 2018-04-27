## 装饰模式（Decorator）  

装饰模式又名包装(Wrapper)模式。装饰模式以对客户端透明的方式扩展对象的功能，是继承关系的一个替代方案。简而言之，给一个对象增加一些新的功能，而且是动态的，要求装饰对象和被装饰对象实现同一个接口，装饰对象持有被装饰对象的实例。  

**应用场景**  

1. 需要扩展一个类的功能。  
2. 动态的为一个对象增加功能，而且还能动态撤销。（继承不能做到这一点，继承的功能是静态的，不能动态增删。）  

涉及的角色有：  

1. **Component** ：抽象构件角色，给出一个抽象接口，以规范准备接收附加责任的对象。  
2. **ConcreteComponent** ：具体构件角色，定义一个将要接收附加责任的类。  
3. **Decorator** ：装饰角色，持有一个构件(Component)对象的实例，并定义一个与抽象构件接口一致的接口。  
4. **ConcreteDecorator** ：具体装饰角色，负责给构件对象“贴上”附加的责任。  

eg.给小女孩戴花

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class ConcreteComponent implements Component {
	
    public void girlMethod() {
	System.out.println("girl");
    }
}

interface Component {

    void girlMethod();
	
}

class Decorator implements Component {

    Component concreteComponent;
	
    public Decorator(Component concreteComponent){
	this.concreteComponent = concreteComponent;
    }
	
    @Override
    public void girlMethod() {
	concreteComponent.girlMethod();
    }
	
}

class ConcreteDecorator extends Decorator {

    public ConcreteDecorator(Component concreteComponent) {
	super(concreteComponent);
    }
	
    @Override
    public void girlMethod() {
	super.girlMethod();
	newMethod();
    }

    private void newMethod() {
	System.out.println("Put a flower on her head.");
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
	Component concreteComponent = new ConcreteDecorator(new ConcreteComponent());
	concreteComponent.girlMethod();
    }
	
}
```
**简化**  

大多数情况下，装饰模式的实现都要比上面给出的示意性例子要简单。

如果只有一个ConcreteComponent类，那么可以考虑去掉抽象的Component类（接口），把Decorator作为一个ConcreteComponent子类。  

如果只有一个ConcreteDecorator类，那么就没有必要建立一个单独的Decorator类，而可以把Decorator和ConcreteDecorator的责任合并成一个类。甚至在只有两个ConcreteDecorator类的情况下，都可以这样做。  

注意：装饰模式对客户端的透明性要求程序不要声明一个ConcreteComponent类型的变量，而应当声明一个Component类型的变量。  


**装饰模式的优点**  

1. 装饰模式与继承关系的目的都是要扩展对象的功能，但是装饰模式可以提供比继承更多的灵活性。装饰模式允许系统动态决定“贴上”一个需要的“装饰”，或者除掉一个不需要的“装饰”。继承关系则不同，继承关系是静态的，它在系统运行前就决定了。  
2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。  

**装饰模式的缺点**  

由于使用装饰模式，可以比使用继承关系需要较少数目的类。使用较少的类，当然使设计比较易于进行。但是，在另一方面，使用装饰模式会产生比使用继承关系更多的对象。更多的对象会使得查错变得困难，特别是这些对象看上去都很相像。  


