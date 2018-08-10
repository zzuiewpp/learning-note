# 对象转型

## 向上转型

​	**父类引用指向子类对象的时候，该引用能访问到的只是作为父类的那部分所拥有的属性和方法，至于作为子类的那部分该引用看不到。如果需要访问子类的所有方法和属性，则需要进行*对象转型*，即强制类型转换。**

## 向下转型

​	float—>int；double—>float等等





# 动态绑定— 态

​	**动态绑定使得程序的可扩展性达到了最好**

## 满足条件

​	继承 + 重写 + 父类引用指向子类对象

## 动态绑定

​	动态绑定是在程序运行时确定的，子类继承父类了父类的成员，同时子类又有自己的私有属性和方法，然后父类又作为另一个类中的成员变量，逻辑如下：

```java
// 基类
Class Animal{
    String name;
    public void action(){
        ...
    }
}
// 子类
Class Cat extends Animal{
    String name;
    String addr;
    public void action(){
        ...
    }
}
class Person{
    String name;
    Animal animal;
    ...
}
public class Action{
    public static void main(String[] args){
        Animal a = new Cat();
		Person p = new Person(name, a);
    	a.action();	        
    }
}
```

如上，初始化Action对象并new对象时，如何判断执行执行哪个action()方法？如果是根据引用类型，则执行基类的action方法；而在动态绑定中（多态）中，采用的是根据实际类型，即在程序中new的谁则调用的就是谁的action方法。**深入理解该机制，当实际中找执行哪个action的过程中，在基类内部有个指针指向自己的action方法，当new某个集成于该基类的子类对象时，这个指针则改变指向，即指向刚new的这个子类的action方法，这就是动态绑定机制。**<u>只有当程序运行起期间，new出了对象后才会判断要调用哪个的方法。</u>

![动态绑定](/Users/walker/Library/Mobile Documents/com~apple~CloudDocs/note/pic/动态绑定.png)

**子类的test方法会出现在基类的相同位置**





# 反射

