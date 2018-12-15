# 面向对象基本概念
## 1. 成员变量与局部变量
* 成员变量存在于**堆内存**，生命周期与**对象**同步；

* 局部变量存在于**栈内存**，调用方法时存在，结束调用时销毁。

## 2. 成员变量与静态变量
* 静态变量在内存中存在于**方法区的静态区**，随着**类**的加载，随着类消失而消失。

* **调用方法不同：**静态变量可以通过**类名**调用，也可以通过**对象**调用；而成员变量只能通过**对象**调用。


## 3. 关于继承
### 1. 继承的作用
### 2. Java中继承的特点
* Java只支持单继承，不支持多继承
```java
class SubDemo extends Demo{} //ok
class SUbDemo extends Demo1,Demo2,...{}// error
```
* Java支持多层继承(继承体系)
```java
class A{}
class B extends A{}
class C extends B{}
```
* 子类只能继承父类的所有非私有变量(成员方法和成员变量)

 tips：破坏了封装性

* 子类不能继承父类的构造方法，但是可以通过super关键字去访问父类构造方法。 

* super关键字与this关键字很像，this代表本类对应的引用，super代表父类的存储空间引用。

* 子类中所有的构造方法默认都会访问父类中**空参数的构造方法**。
    因为子类会继承父类中的数据，可能还会使用父类的数据。所以，子类初始化之前一定会先完成父类的初始化。
    每一个构造方法的第一句默认都是：super()。super()一定要出现再第一句上，否则父类数据会被多次初始化。

* **面试题：** 方法重写和方法重载的区别?方法重载能改变返回值类型吗?

**重写方法**必须满足下列条件

(1) 子类的方法的名称及参数必须和所覆盖的方法相同

(2) 子类的方法返回类型必须和所覆盖的方法相同

(3) 子类方法不能缩小所覆盖方法的访问权限

(4) 子类方法不能抛出比所覆盖方法更多的异常

**重载方法**必须满足下列条件

(1) 方法名必须相同

(2) 方法的参数签名必须相同

(3) 方法的返回类型和方法的修饰符可以不相同

```java
public class B extends A{
	//下面的是方法的重写（overRide）
	public void riding(){
		System.out.println("this is  overRiding ");
	}
	//下面两个函数是方法的重载(overLoad)，但是返回值类型不同，可以运行
	public String loading(int x){
		System.out.println("this is overLoading return String");
		return null;
	}
	public int loading(int x,int y){
		System.out.println("this is overLoading return int");
		return 0;
	}
	public static void main(String[] args) {
		 A a = new A();
		 a.riding();
		 a.loading(2);
		 a.loading(2,3);
	}
    /*有以上程序可以知道，方法的重载是可以改变返回值类型的，但是尚不能说明方法的重写是否可以改变返回值类型，现在修改A类中 riding()方法的返回值类型为int.*/
    public int riding(){  
        System.out.println("this is  overRiding ");  
    } // error
    /*由此可知，方法的覆盖是不允许修改返回值类型的。*/
}
class A{
	public void riding(){
		System.out.println("this is B");
	}
}
```
* 对于父类private修饰的成员

子类可以**继承**父类的所有成员跟方法，继承下来不代表可以**访问**，要访问得看访问控制规则。私有属性也可以继承，不过根据访问控制规则，私有属性虽继承下来却不可以访问的，只有通过public的方法访问继承下来的私有属性。

```java
public class A {
    private int a;
    
    public int getA(){
        return a;
    }
    public void setA(int a){
        this.a=a;
    }

}
public class B extends A{
     private int b;

    public int getB() {
        return b;
    }

    public void setB(int b) {
        this.b = b;
    }
     
}
public class C extends B {
    private int c;
    public int getC() {
        return c;
    }

    public void setC(int c) {
        this.c = c;
    }
}
```
```java
C c1=new C();//c1可以通过setA()与getA()方法访问和控制A的私有变量a。
```
* **final关键字面试题**

    * final修饰局部变量
        * 在方法内部该变量不可以被改变。
        * 在方法声明上：
            * 基本类型：是值不能被改变；
            * 引用类型： 是地址值不能被改变。
    * final修饰变量的初始化时机
        * 在对象构造完毕前即可。、

## 关于多态
### 1. 多态概述
* 多态概述
* **多态的前提和体现**
    * 有继承关系
    * 有方法重写
    * 有父类引用指向子类
* 多态的好处
    * 提高了程序的维护性(继承保证)
    * 提高了程序的扩展性(由多态保证)
* 多态的弊端
    * 不能访问子类的特有功能
    * 那么我们怎么才能访问子类的特有功能呢？
        * **多态中的转型**
一个示例：
```java
public class polymorphic_test {
    public static void main(String[] args){
        /*经典多态对比*/
        /* Cat cat=new Cat();
        cat.cry();
        Dog dog=new Dog();
        dog.cry();
         */
        Animal an=new Cat();
        an.cry();
        an=new Dog();
        an.cry();
        /*Cat an=new Cat();
        an.cry();
        an=new Dog();*///error
        Master mm=new Master();
        mm.feed(new Dog(),new Bone());
        mm.feed(new Cat(),new Fish());
    }
}

class Animal{
    int age;
    String name;

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void cry(){
        System.out.println("我不知道叫什么");
    }
    public void eat(){
        System.out.println("我不知道吃什么");
    }
}

class Cat extends Animal{
    //override
    public void cry(){
        System.out.println("喵喵喵");
    }

    public void eat(){
        System.out.println("我是猫，我爱吃鱼！");
    }
}

class Dog extends Animal{
    //override
    public void cry(){
        System.out.println("汪汪汪");
    }

    public void eat(){
        System.out.println("我是狗，我爱吃骨头！");
    }
}

class Food{
    String name;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void showName(){

    }
}

class Fish extends Food{
    //override

    @Override
    public void showName() {
        System.out.println("Food:fish");
    }
}

class Bone extends Food{
    //override

    @Override
    public void showName() {
        System.out.println("Food:Bone");
    }
}
//主人类
class Master{
    // 给动物喂食物，如果没有多态，他要写给猫喂食和给狗喂食两个方法
    // 有了多态，以后即使再来好多动物，用这一个函数就可以了
    public void feed(Animal an,Food f){
        an.eat();
        an.cry();
        f.showName();
    }
}
```
* 多态中的**转型问题**，从右往左看
```java
    //多态中的转型
    Animal p0=new Animal();
    Dog p1=new Dog();
    Cat p2=new Cat();
    Animal p3=new Dog();
    Animal p4=new Cat();

    p0=p1;
    p1=p2;//error
    p1=(Dog)p3;
    p2=(Cat)p4;
    p1=p3;//error
    p2=p4;//error;
```

## 抽象类

### 1.抽象类的概述
### 2.抽象类的特点：**不能被实例化**
### 3.抽象类的成员特点
* 成员变量：可以是常量也可以是变量
* 构造方法：有构造方法，但不能被实例化
    * 那么构造方法的作用是什么？用于子类访问父类数据的初始化
* 成员方法
    * 也可以有**抽象方法**，限定子类必须完成某些动作
    * 也可以有非抽象方法，提高代码的复用性

## 接口
### 1.接口概述
### 2.接口特点
* 接口用关键字interface表示
* 类实现接口用implements表示
* **接口不能被实例化**
    * 那么接口如何实现实例化：
        * 按照多态的方式，有具体的子类实例化。其实这也是多态的一种，接口多态。
* **接口的子类**
    * 要么是抽象类
    * 要么重写接口中的所有抽象方法
### 3.接口成员的特点
* 成员变量
    * 只能是**常量**
    * 默认修饰符 public static final
* 构造方法
    * 没有，因为接口主要是扩展功能的，没有具体的存在
* 成员方法
    * 只能是抽象方法
    * 默认修饰符 public abstract

## 类与类、类与接口、接口与接口的关系
* 类与类
    * 继承：只能单继承但可以多层继承
* 类与接口
    * 实现：可以单实现也可以多实现。还可以在继承一个类的同时实现多个接口。
* 接口与接口
    * 继承：可以单继承也可以多继承。

## 抽象类与接口的区别
* 成员区别
    * 抽象类：变量、常量；有抽象方法和非抽象方法。
    * 接口： 常量；抽象方法。
* 关系区别：同上
    * 类与类: 单继承
    * 类与接口：单实现，多实现
    * 接口与接口：单继承，多继承
* 设计理念的区别
    * 抽象类：被继承体现的是“is a”的关系。共性功能
    * 接口：被实现体现的是-“like a”的关系。扩展功能。
