## 命令模式（Command）

命令模式属于对象的行为模式。命令模式又称为行动(Action)模式或交易(Transaction)模式。  

命令模式把一个请求或者操作封装到一个对象中。命令模式允许系统使用不同的请求把客户端参数化，对请求排队或者记录请求日志，可以提供命令的撤销和恢复功能。  


**优点**  

1. 更松散的耦合  
命令模式使得发起命令的对象——客户端，和具体实现命令的对象——接收者对象完全解耦，也就是说发起命令的对象完全不知道具体实现对象是谁，也不知道如何实现。  

2. 更动态的控制  
命令模式把请求封装起来，可以动态地对它进行参数化、队列化和日志化等操作，从而使得系统更灵活。  

3. 很自然的复合命令  
命令模式中的命令对象能够很容易地组合成复合命令，也就是宏命令，从而使系统操作更简单，功能更强大。  

4. 更好的扩展性
由于发起命令的对象和具体的实现完全解耦，因此扩展新的命令就很容易，只需要实现新的命令对象，然后在装配的时候，把具体的实现对象设置到命令对象中，然后就可以使用这个命令对象，已有的实现完全不用变化。  

eg.司令员下令让士兵去干件事情，从整个事情的角度来考虑，司令员的作用是，发出口令，口令经过传递，传到了士兵耳朵里，士兵去执行。这个过程好在，三者相互解耦，任何一方都不用去依赖其他人，只需要做好自己的事儿就行，司令员要的是结果，不会去关注到底士兵是怎么实现的。  

Invoker是调用者（司令员），Receiver是被调用者（士兵），MyCommand是命令，实现了Command接口，持有接收对象  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Receiver {

    public void action(){  
        System.out.println("command received!");  
    }
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Command {
	
    void exe();
	
}

class CommandImpl implements Command {

    private Receiver receiver;
	
    public CommandImpl(Receiver receiver) {
	this.receiver = receiver;
    }

    @Override
    public void exe() {
	receiver.action();
    }
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Invoker {
	
    private Command command;  
    
    public Invoker(Command command) {  
        this.command = command;  
    }  
  
    public void action(){  
        command.exe();  
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
		
	Receiver receiver = new Receiver();  
        Command cmd = new CommandImpl(receiver);  
        Invoker invoker = new Invoker(cmd);  
        invoker.action();
      
    }

}
```

命令模式的目的就是达到命令的发出者和执行者之间解耦，实现请求和执行分开。  


















