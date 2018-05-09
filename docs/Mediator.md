## 中介者模式（Mediator）

中介者模式是对象的行为模式。中介者模式包装了一系列对象相互作用的方式，使得这些对象不必相互明显引用。从而使它们可以较松散地耦合。当这些对象中的某些对象之间的相互作用发生改变时，不会立即影响到其他的一些对象之间的相互作用。从而保证这些相互作用可以彼此独立地变化。  

**优点**  

1. 松散耦合  
中介者模式通过把多个同事对象之间的交互封装到中介者对象里面，从而使得同事对象之间松散耦合，基本上可以做到互补依赖。这样一来，同事对象就可以独立地变化和复用，而不再像以前那样“牵一处而动全身”了。  
2. 集中控制交互  
多个同事对象的交互，被封装在中介者对象里面集中管理，使得这些交互行为发生变化的时候，只需要修改中介者对象就可以了，当然如果是已经做好的系统，那么就扩展中介者对象，而各个同事类不需要做修改。  
3. 多对多变成一对多  
没有使用中介者模式的时候，同事对象之间的关系通常是多对多的，引入中介者对象以后，中介者对象和同事对象的关系通常变成双向的一对多，这会让对象的关系更容易理解和实现。  

**缺点**  

过度集中化。如果同事对象的交互非常多，而且比较复杂，当这些复杂性全部集中到中介者的时候，会导致中介者对象变得十分复杂，而且难于管理和维护。  

eg.出租与租赁  

抽出出租动作，由房主和中介实现。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */
 
interface Landlord {
    String hire();
}

class LandlordImpl implements Landlord {
	
    private String house;
	
    public LandlordImpl(){
	house = "别墅";
    }
	
    @Override
    public String hire(){
	System.out.println("出租 "+house);
	return house;
    }
	
}

public class Mediator implements Landlord {

    Landlord landlord;
	
    public Mediator(Landlord landlord){
	this.landlord = landlord;
    }

    @Override
    public String hire() {
	return landlord.hire();
    }
	
}
```

租户，有租赁动作。  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Lessee {
	
    public void renting(Landlord landlord){
	System.out.println("租赁到"+landlord.hire());
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
		
	Landlord landlord = new LandlordImpl();
	Mediator mediator = new Mediator(landlord);
	Lessee l = new Lessee();
	l.renting(mediator);
	
    }

}
```

**拓展**  

[《JAVA与模式》26天系列—第26天—调停者模式](https://blog.csdn.net/m13666368773/article/details/7712213)

