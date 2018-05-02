## 代理模式（Proxy）  

代理模式是对象的结构模式。代理模式给某一个对象提供一个代理对象，并由代理对象控制对原对象的引用。  

按照代理类的创建时期，可以分为：静态代理和动态代理。  

静态代理：　指程序员创建好代理类，编译时直接生成代理类的字节码文件。  
动态代理：　在程序运行时，通过反射机制动态生成代理类。  

静态代理特点： 代理类和委托类实现了相同的接口，代理类通过委托类实现了相同的方法。这样就出现了大量的代码重复，而且代理类只能为特定的接口(Service)服务。  

动态代理特点： 代理类需要实现InvocationHandler接口。  

使用场合举例： 如果需要委托类处理某一业务，那么我们就可以先在代理类中，对客户的权限、各类信息先做判断，如果不满足某一特定条件，则将其拦截下来，不让其代理。  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Service {

    void sayHello();
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class ServiceImpl implements Service {

    @Override
    public void sayHello() {
	System.out.println("hello sep6th!");
    }
	
    public boolean auth(){
	return true;
    }

}
```

```java
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.lang.reflect.Proxy;

/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class ServiceProxy implements InvocationHandler {

    private Object target;
    
    /*执行顺序：1*/
    public ServiceProxy(Object target) {
	this.target = target;
    }
        
    /*执行顺序：3*/
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
	Object result = null;
	if (!(target instanceof ServiceImpl)) {  
            System.out.println("不能代理该对象");  
            return result;  
        } else if(!((ServiceImpl) target).auth()) {
            System.out.println("权限不够");  
            return result;
        }
	    result = method.invoke(target, args);  
	    return result;
	}
	
    /**
     * 执行顺序：2
     * 返回委托类接口的实例对象
     */
    public Object getProxyInstance() {  
        return Proxy.newProxyInstance(target.getClass().getClassLoader(), target.getClass().getInterfaces(), this);  
    } 
	
    public static void main(String[] args) {
	// 创建委托类实例，即被代理的类对象  
        ServiceImpl target = new ServiceImpl();  
        // 创建动态代理类  
        ServiceProxy proxy = new ServiceProxy(target);
        Service service = (Service) proxy.getProxyInstance();
        service.sayHello();
    }

}
```

**拓展——Cglib代理**

```java
/**
 * 目标对象
 */
public class PersonDao {

    public void delete(){
        System.out.println("---删除操作---");
    }
    
}
```

```java
import java.lang.reflect.Method;

import net.sf.cglib.proxy.Enhancer;
import net.sf.cglib.proxy.MethodInterceptor;
import net.sf.cglib.proxy.MethodProxy;

/**
 * Cglib子类代理工厂
 * (对PersonDao 在内存中动态构建一个子类对象)
 */
public class CglibProxyFactory implements MethodInterceptor {

    /* 维护目标对象*/
    private Object target;

    public CglibProxyFactory(Object target) {
        this.target = target;
    }
    
    /* 给目标对象创建代理对象*/
    public Object getProxyInstance(){
    
        /*1. 工具类*/
        Enhancer en = new Enhancer();
        /*2. 设置父类（以子类方式在内存中动态创建代理对象，需要知道子类的父类，
         *    此处为target，即是PersonDao的实例对象）
         */
        en.setSuperclass(target.getClass());
        /*3. 设置回调函数（执行target类里的方法时，会触发拦截器中的方法）*/
        en.setCallback(this);
        /*4. 创建子类(代理对象)*/
        return en.create();
	
    }

    /*
     * CGLib采用非常底层的字节码技术，可以为一个类创建一个子类，
     * 并在子类中采用方法拦截的技术拦截所有父类方法的调用，并顺势植入横切逻辑。
     */
    @Override
    public Object intercept(Object obj, Method method, Object[] args, MethodProxy proxy) throws Throwable {
        
	/*添加其它代码*/
        System.out.println("--开启事务--");

        /*执行目标对象的方法*/
        Object returnValue = method.invoke(target, args);

        /*添加其它代码*/
        System.out.println("--提交事务--");

        return returnValue;
	
    }

}
```

```java
import org.junit.Test;

import com.sep6th.dao.PersonDao;
import com.sep6th.proxy.CglibProxyFactory;

public class Mytest {

    @Test
    public void test() {
    
        /*目标对象*/
        PersonDao target = new PersonDao();
        System.out.println(target.getClass());

        /*代理对象*/
        PersonDao proxy = (PersonDao)new CglibProxyFactory(target).getProxyInstance();
        System.out.println(proxy.getClass());

        /*执行代理对象的方法*/
        proxy.delete();
	
    }
}
```




