## 解释器模式（Interpreter）

解释器模式是类的行为模式。给定一个语言之后，解释器模式可以定义出其文法的一种表示，并同时提供一个解释器。客户端可以使用这个解释器来解释这个语言中的句子。  



```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Context {

    private int num1;
	
    private int num2;
      
    public Context(int num1, int num2) {  
        this.num1 = num1;  
        this.num2 = num2;  
    }  
      
    public int getNum1() {  
        return num1;  
    }  
    
    public void setNum1(int num1) {  
        this.num1 = num1;  
    }  
    
    public int getNum2() {  
        return num2;  
    }  
    
    public void setNum2(int num2) {  
        this.num2 = num2;  
    } 
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Expression {

    int interpret(Context context);

}

class Plus implements Expression {  
	  
    @Override  
    public int interpret(Context context) {  
        return context.getNum1()+context.getNum2();  
    }

}

class Minus implements Expression {  

    @Override  
    public int interpret(Context context) {  
        return context.getNum1()-context.getNum2();  
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
	// 计算200+58-8的值  
        int result = new Minus().interpret((new Context(new Plus()  
                .interpret(new Context(200, 58)), 8)));  
        System.out.println(result);

    }

}
```




