## 迭代子模式（Iterator）

迭代子模式又叫游标(Cursor)模式，是对象的行为模式。迭代子模式可以顺序地访问一个聚集中的元素而不必暴露聚集的内部表象（internal representation）。  

**聚集和JAVA聚集**  
多个对象聚在一起形成的总体称之为聚集(Aggregate)，聚集对象是能够包容一组对象的容器对象。聚集依赖于聚集结构的抽象化，具有复杂化和多样性。数组就是最基本的聚集，也是其他的JAVA聚集对象的设计基础。  

JAVA聚集对象是实现了共同的java.util.Collection接口的对象，是JAVA语言对聚集概念的直接支持。从1.2版开始，JAVA语言提供了很多种聚集，包括Vector、ArrayList、HashSet、HashMap、Hashtable等，这些都是JAVA聚集的例子。  


```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Collection {

    Iterator iterator();
	
    /*取得集合元素*/  
    Object get(int i);  
      
    /*取得集合大小*/  
    int size();
	
}

class CollectionImpl implements Collection {

    public String str[] = {"A","B","C","D","E"};
	
    @Override
    public Iterator iterator() {
	return new IteratorImpl(this);
    }

    @Override
    public Object get(int i) {
	return str[i];
    }

    @Override
    public int size() {
	return str.length;
    }  
	
}
```

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public interface Iterator {

    //前移  
    Object previous();  
      
    //后移  
    Object next();  
    
    //是否有下一个
    boolean hasNext();  
      
    //取得第一个元素  
    Object first();
	
}

class IteratorImpl implements Iterator {  
	  
    private Collection collection;  
    private int pos = -1;  
      
    public IteratorImpl(Collection collection){  
        this.collection = collection;  
    }  
      
    @Override  
    public Object previous() {  
        if(pos > 0){  
            pos--;  
        }  
        return collection.get(pos);  
    }  
  
    @Override  
    public Object next() {  
        if(pos < collection.size() - 1){  
            pos++;  
        }  
        return collection.get(pos);  
    }  
  
    @Override  
    public boolean hasNext() {  
        if(pos < collection.size() - 1){  
            return true;  
        }else{  
            return false;  
        }  
    }  
  
    @Override  
    public Object first() {  
        pos = 0;  
        return collection.get(pos);  
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
	Collection collection = new CollectionImpl();  
        Iterator it = collection.iterator();  
          
        while(it.hasNext()){  
            System.out.println(it.next());  
        }
    }

}
```
