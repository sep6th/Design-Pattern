## 状态模式（State）

状态模式，又称状态对象模式（Pattern of Objects for States），状态模式是对象的行为模式。  

状态模式允许一个对象在其内部状态改变的时候改变其行为。这个对象看上去就像是改变了它的类一样。  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 * 状态类的核心类
 */

public class State {

    private String value;  
    
    public String getValue() {  
        return value;  
    }  
  
    public void setValue(String value) {  
        this.value = value;  
    }  
  
    public void online(){  
        System.out.println("online!");  
    }  
      
    public void offline(){  
        System.out.println("offline!");  
    }  
	
}
```


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 * 状态切换类
 */

public class Context {  
	  
    private State state;  
  
    public Context(State state) {  
        this.state = state;  
    }  
  
    public State getState() {  
        return state;  
    }  
  
    public void setState(State state) {  
        this.state = state;  
    }  
  
    public void method() {  
        if (state.getValue().equals("online")) {  
            state.online();  
        } else if (state.getValue().equals("offline")) {  
            state.offline();  
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
		
		State state = new State();  
        Context context = new Context(state);  
          
        //设置online状态  
        state.setValue("online");  
        context.method();  
          
        //设置offline状态  
        state.setValue("offline");  
        context.method();

	}

}
```


