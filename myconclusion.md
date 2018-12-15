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

