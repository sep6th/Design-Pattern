## 抽象工厂模式（Abstract Factory Method）
工厂方法模式存在的问题是，类的创建依赖工厂类，也就是说，如果想要拓展程序，必须对工厂类进行修改，这违背了闭包原则。如何解决？就用到抽象工厂模式，创建多个工厂类，这样一旦需要增加新的功能，直接增加新的工厂类就可以了，不需要修改之前的代码。  

eg.发送邮件和短信  

**1. 创建“产品”接口**  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface ISender {

	void send();
	
}
```
**2. 多个“产品”类实现“产品”接口**  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class MailSender implements ISender {

	@Override
	public void send() {
		System.out.println("this is mailsender!"); 
	}

}

class SmsSender implements ISender{

	@Override
	public void send() {
		System.out.println("this is sms sender!");
	}
	
}
```
**3. 创建工厂接口**  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface IFactoryManager {

	ISender getSender();
	
}
```

**4. 多个工厂类实现工厂接口**  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class MailSenderFactory implements IFactoryManager  {

	@Override
	public ISender getSender() {
		return new MailSender(); 
	}
	
}

class SmsSenderFactory implements IFactoryManager  {

	@Override
	public ISender getSender() {
		return new SmsSender(); 
	}
	
}

/* ... 添加其他工厂实现 IFactoryManager */

public class SenderFactory {
	
	public static void main(String[] args) {
		IFactoryManager factoryManager = new MailSenderFactory();
		factoryManager.getSender().send();
	}
	
}
```
**总结**  
该模式的好处就是，如果你现在想增加一个功能：发及时信息，则只需做一个实现类，实现ISender接口，同时做一个工厂类，实现IFactoryManager接口，就OK了，无需去改动现成的代码。这样做，拓展性较好！




