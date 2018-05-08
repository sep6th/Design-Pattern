## 模板方法模式（Template Method）

模板方法模式是对象的行为模式。准备一个抽象类，将部分逻辑以具体方法以及具体构造函数的形式实现，然后声明一些抽象方法来迫使子类实现剩余的逻辑。不同的子类可以以不同的方式实现这些抽象方法，从而对剩余的逻辑有不同的实现。这就是模板方法模式的用意。  

eg.计算器  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public abstract class AbstractCalculator {
	
    /*主方法，实现对本类其它方法的调用*/  
    public final int calculate(String exp,String opt){
    	String array[] = exp.split(opt);
    	int arrayInt[] = new int[2];  
        arrayInt[0] = Integer.parseInt(array[0]);  
        arrayInt[1] = Integer.parseInt(array[1]);
        return doCalculate(arrayInt[0], arrayInt[1]);  
    }  
    
    /*被子类重写的方法*/
    abstract int doCalculate(int param1,int param2);
    
}

```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */
class Plus extends AbstractCalculator {

    @Override
    public int doCalculate(int param1, int param2) {
	return param1 + param2;
    }

}

class Minus extends AbstractCalculator {

    @Override
    public int doCalculate(int param1, int param2) {
	return param1 - param2;
    }

}

class Multiply extends AbstractCalculator {

    @Override
    public int doCalculate(int param1, int param2) {
	return param1 * param2;
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

	AbstractCalculator calculator = new Plus();
	int result = calculator.calculate("200+50", "\\+");
	System.out.println(result);
	result = calculator.doCalculate(200, 50);
	System.out.println(result);
		
    }

}
```

**拓展**  

HttpServlet担任抽象模板角色
模板方法：由service()方法担任。
基本方法：由doPost()、doGet()等方法担任。

