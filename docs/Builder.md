## 建造者模式（Builder）

将一个复杂的对象的构建与它的表象分离，使得同样的构建过程可以创建不同的表象。  

工厂模式提供的是创建单个类的模式，而建造者模式则是将各种产品集中起来进行管理，用来创建复合对象，所谓复合对象就是指某个类具有不同的属性，其实建造者模式就是前面抽象工厂模式和最后的Test结合起来得到的。与工厂模式的区别是：建造者模式更加关注与零件装配的顺序。  

通常包括以下几个角色：  
1. **Product** ：要创建的复杂对象。
2. **Builder** ：给出一个抽象接口，以规范产品对象的各个组成成分的建造。这个接口规定要实现复杂对象的哪些部分的创建，并不涉及具体的对象部件的创建。  
3. **ConcreteBuilder** ：实现Builder接口，针对不同的商业逻辑，具体化复杂对象的各部分的创建。 在建造过程完成后，提供产品的实例。  
4. **Director** ：调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建。  

**应用实例**  
1. 去肯德基，汉堡、可乐、薯条、炸鸡翅等是不变的，而其组合是经常变化的，生成出所谓的"套餐"。 
2. JAVA 中的 StringBuilder。

**使用场景** 
1. 需要生成的对象具有复杂的内部结构。  
2. 需要生成的对象内部属性本身相互依赖。  

eg. 造人

Product（要创建的复杂对象）  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Product {  
	// 产品机器人结构
    private String head;  
    private String body;  
    private String foot;  
     
    public void setHead(String head) {  
        this.head = head;  
    }  
    
    public void setBody(String body) {  
        this.body = body;  
    }  
   
    public void setFoot(String foot) {  
        this.foot = foot;  
    }

	@Override
	public String toString() {  
		return "Product [head=" + head + ", body=" + body + ", foot=" + foot + "]";  
	}  
    
}
```

Builder（给出一个抽象接口，以规范产品对象的各个组成成分的建造。这个接口规定要实现复杂对象的哪些部分的创建，并不涉及具体的对象部件的创建）  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Builder {
	
	Builder buildHead();
	
	Builder buildBody();
	
	Builder buildFoot();
	
	Product build();
	
}  
```
ConcreteBuilder（实现Builder接口，针对不同的商业逻辑，具体化复杂对象的各部分的创建。 在建造过程完成后，提供产品的实例）  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class ConcreteBuilder implements Builder {
	
	//创建一个Product实例，用于调用set方法
	Product product ;
	
	public ConcreteBuilder(){
		product = new Product();
	}
	
	public Builder buildHead() {
		product.setHead("建造头部部分");
		return this;
	}
	
	public Builder buildBody() {
		product.setBody("建造身体部分");
		return this;
	}
	
	public Builder buildFoot() {
		product.setFoot("建造四肢部分");
		return this;
	}
	
	//在建造过程完成后，提供产品的实例
	public Product build() {
		return product;
	}
	
}  
```
Director（调用具体建造者来创建复杂对象的各个部分，在指导者中不涉及具体产品的信息，只负责保证对象各部分完整创建或按某种顺序创建）  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Director {

	public static Product constructProduct(Builder builder) {  
		//按照 头部--->身体--->四肢 的顺序创建人物
		return builder.buildHead().buildBody().buildFoot().build();
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
		Product product = Director.constructProduct(new ConcreteBuilder()); 
		System.out.println(product.toString());
	}  
    
} 
```



**拓展**  

eg.统一API响应结果封装  
 
```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */
 
import com.alibaba.fastjson.JSON;

public class Result {

    private Integer code;
    private String msg;
    private Object data;
    
    public Result setCode(ResultCode resultCode) {
        this.code = resultCode.code();
        return this;
    }
	
    public int getCode() {
        return code;
    }

    public String getMsg() {
        return msg;
    }

    public Result setMsg(String msg) {
        this.msg = msg;
        return this;
    }

    public Object getData() {
        return data;
    }

    public Result setData(Object data) {
        this.data = data;
        return this;
    }
    
    @Override
    public String toString() {
        return JSON.toJSONString(this);
    }
    
}
```


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public enum ResultCode {

    SUCCESS(200),				//成功  
    FAIL(400),					//失败  
    UNAUTHORIZED(401),			//未认证（签名错误）  
    NOT_FOUND(404),				//接口不存在  
    INTERNAL_SERVER_ERROR(500); //服务器内部错误  
	
	private final Integer code;  

	ResultCode(Integer code) {
        this.code = code;
    }

	public Integer code() {
        return code;
    }
	
}
```


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class ResultGenerator {
    private static final String DEFAULT_SUCCESS_MSG = "SUCCESS";

    public static Result genSuccessResult() {
        return new Result()
                .setCode(ResultCode.SUCCESS)
                .setMsg(DEFAULT_SUCCESS_MSG);
    }

    public static Result genSuccessResult(Object data) {
        return new Result()
                .setCode(ResultCode.SUCCESS)
                .setMsg(DEFAULT_SUCCESS_MSG)
                .setData(data);
    }

    public static Result genFailResult(String msg) {
        return new Result()
                .setCode(ResultCode.FAIL)
                .setMsg(msg);
    }
}
```
