## 工厂模式（Factory Method）

工厂方法模式分为三种：普通工厂模式、多个工厂方法模式、静态工厂模式。

**普通工厂模式，** 就是建立一个工厂类，对实现了同一接口的一些类进行实例的创建。

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
**3. 创建工厂类**

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class SenderFactory  {

	public ISender sender(String type) {  
        	if ("mail".equals(type)) {  
            		return new MailSender();  
       		} else if ("sms".equals(type)) {  
            		return new SmsSender();  
        	} else {  
            		System.out.println("请输入正确的类型!");  
            		return null;  
        	}  
    	}
	
	public static void main(String[] args) {
		SenderFactory factory = new SenderFactory();
		ISender sender = factory.sender("sms");
		sender.send();
	}
	
}
```
**多个工厂方法模式，** 是对普通工厂方法模式的改进，在普通工厂方法模式中，如果传递的字符串出错，则不能正确创建对象，而多个工厂方法模式是提供多个工厂方法，分别创建对象。

eg.发送邮件和短信

**1. 创建“产品类”**

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class MailSender {

	public void send() {
		System.out.println("this is mailsender!"); 
	}

}

class SmsSender{

	public void send() {
		System.out.println("this is sms sender!");
	}
	
}
```

**2. 创建工厂类**

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class SenderFactory  {

	public MailSender getMailSender() {  
        	return new MailSender();  
    	}
	
	public SmsSender getSmsSender(){
		return new SmsSender(); 
	}
	
	public static void main(String[] args) {
		SenderFactory factory = new SenderFactory();
		factory.getMailSender().send();
	}
	
}
```

**静态工厂模式，** 将上面的多个工厂方法模式里的方法置为静态的，不需要创建工厂类实例，直接调用即可。

**修改工厂类**
```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class SenderFactory  {

	public static MailSender getMailSender() {  
        	return new MailSender();  
    	}
	
	public static SmsSender getSmsSender(){
		return new SmsSender(); 
	}
	
	public static void main(String[] args) {
		SenderFactory.getMailSender().send();
	}
	
}
```
**总结**  
工厂模式适合：凡是出现了大量的产品需要创建，并且具有共同的接口时，可以通过工厂方法模式进行创建。在以上的三种模式中，第一种如果传入的字符串有误，不能正确创建对象，第三种相对于第二种，不需要实例化工厂类，所以，大多数情况下，我们会选用第三种——静态工厂模式。

