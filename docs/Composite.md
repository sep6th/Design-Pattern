## 组合模式（Composite）  

组合模式属于对象的结构模式，有时又叫做“部分——整体”模式。组合模式将对象组织到树结构中，可以用来描述整体与部分的关系。组合模式可以使客户端将单纯元素与复合元素同等看待。  

涉及到三个角色：  

1. **Component** 抽象构件角色，这是一个抽象角色，它给参加组合的对象定义出公共的接口及其默认行为，可以用来管理所有的子对象。合成对象通常把它所包含的子对象当做类型为Component的对象。在安全式的合成模式里，构件角色并不定义出管理子对象的方法，这一定义由树枝构件对象给出。  

2. **Composite** 树枝构件角色，代表参加组合的有下级子对象的对象。树枝构件类给出所有的管理子对象的方法，如add()、remove()以及getChild()。  

3. **Leaf** 树叶构件角色，树叶对象是没有下级子对象的对象，定义出参加组合的原始对象的行为。  

eg.服装  

Component 抽象构件角色  

```java

import java.util.List;

/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public abstract class Component {

    /** 
     * 输出组建自身的名称 
     */  
    public abstract void printStruct(String preStr);  
  
    /** 
     * 聚集管理方法，增加一个子构件对象 
     * @param child 子构件对象 
     */  
    public void addChild(Component child) {  
        /** 
         * 缺省实现，抛出异常，因为叶子对象没有此功能 或者子组件没有实现这个功能 
         */  
        throw new UnsupportedOperationException("对象不支持此功能");  
    }  
  
    /** 
     * 聚集管理方法，删除一个子构件对象 
     *  
     * @param index 子构件对象的下标 
     */  
    public void removeChild(int index) {  
        /** 
         * 缺省实现，抛出异常，因为叶子对象没有此功能 或者子组件没有实现这个功能 
         */  
        throw new UnsupportedOperationException("对象不支持此功能");  
    }  
  
    /** 
     * 聚集管理方法，返回所有子构件对象 
     */  
    public List<Component> getChild() {  
        /** 
         * 缺省实现，抛出异常，因为叶子对象没有此功能 或者子组件没有实现这个功能 
         */  
        throw new UnsupportedOperationException("对象不支持此功能");  
    }
	
}
```

Composite 树枝构件角色  

```java

import java.util.ArrayList;

/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

class Composite extends Component {  
    /** 
     * 用来存储组合对象中包含的子组件对象 
     */  
    private List<Component> childComponents = new ArrayList<Component>();  
    /** 
     * 组合对象的名字 
     */  
    private String name;  
  
    /** 
     * 构造方法，传入组合对象的名字 
     * @param name 组合对象的名字 
     */  
    public Composite(String name) {  
        this.name = name;  
    }  
  
    /** 
     * 聚集管理方法，增加一个子构件对象 
     * @param child 子构件对象 
     */  
    public void addChild(Component child) {  
        childComponents.add(child);  
    }  
  
    /** 
     * 聚集管理方法，删除一个子构件对象 
     * @param index 子构件对象的下标 
     */  
    public void removeChild(int index) {  
        childComponents.remove(index);  
    }  
  
    /** 
     * 聚集管理方法，返回所有子构件对象 
     */  
    public List<Component> getChild() {  
        return childComponents;  
    }  
  
    /** 
     * 输出对象的自身结构 
     * @param preStr 前缀，主要是按照层级拼接空格，实现向后缩进 
     */  
    @Override  
    public void printStruct(String preStr) {  
        // 先把自己输出  
        System.out.println(preStr + "+" + this.name);  
        // 如果还包含有子组件，那么就输出这些子组件对象  
        if (this.childComponents != null) {  
            // 添加两个空格，表示向后缩进两个空格  
            preStr += "  ";  
            // 输出当前对象的子对象  
            for (Component c : childComponents) {  
                // 递归输出每个子对象  
                c.printStruct(preStr);  
            }  
        }  
  
    }  
  
} 
 
```

Leaf 树叶构件角色  

```java
/** 
 * The Apache License 2.0
 * Copyright (c) 2018 sep6th
 */

public class Leaf extends Component {

    /** 
     * 叶子对象的名字 
     */  
    private String name;  
  
    /** 
     * 构造方法，传入叶子对象的名称 
     * @param name 叶子对象的名字 
     */  
    public Leaf(String name) {  
        this.name = name;  
    }
	
    /** 
     * 输出叶子对象的结构，叶子对象没有子对象，也就是输出叶子对象的名字 
     * @param preStr 前缀，主要是按照层级拼接的空格，实现向后缩进 
     */  
    @Override
    public void printStruct(String preStr) {
    	System.out.println(preStr + "-" + name);
    }

	
    public static void main(String[] args) {
        Component root = new Composite("服装");  
        Component c1 = new Composite("男装");  
        Component c2 = new Composite("女装");  
  
        Component leaf1 = new Leaf("衬衫");  
        Component leaf2 = new Leaf("夹克");  
        Component leaf3 = new Leaf("裙子");  
        Component leaf4 = new Leaf("套装");  
  
        root.addChild(c1);  
        root.addChild(c2);  
        c1.addChild(leaf1);  
        c1.addChild(leaf2);  
        c2.addChild(leaf3);  
        c2.addChild(leaf4);  
  
        root.printStruct("");
    }
	
}
```

Console 输出  
```
+服装
  +男装
    -衬衫
    -夹克
  +女装
    -裙子
    -套装
```




