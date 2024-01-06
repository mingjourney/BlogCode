---
title: java菜鸟记
date: 2021-01-03 12:00:01
tags:
  - java
  - 菜鸟
  - 大一
categories:
  - 笔记
---

## java

三元运算符可以嵌套

### 位运算符

操作的是整形的数据

<<:在一定范围内,每向左移一位,相当于 \*2

> 最高效计算2\*8. --> 2 << 3. // 8 << 1;

### 数组

​ equal

fill

sort

toString

binarySearch

### 构造器(constructor)

如果没有显示的定义累的构造器的话 系统默认提供一个空参构造器/

### 方法调用时的内存分配

#### 内存何时分配

方法只有被调用时，在JVM中会给该方法分配“运行所属“内存空间。

#### JVM内存划分的三块主要空间

- **方法区内存**（代码片段）
- 堆内存
- 栈内存(分配方法运行的所属内存空间)

#### 数据结构——“栈”

- 栈针永远指向栈顶元素

- 异常 需要注意的问题 需 要按照类型的要求输入,很具相应的方法,吐过输入的数据类型月要皮球的数据类型不匹配时没汇报一导致程序

- 栈顶元素处于活跃，其他元素静止

- 术语

  - 入栈/压栈/push
  - 弹栈/出栈/pop

- **先进后出 后进先出**

  main进->m1进->m2进->m2出->m1出->main出

#### 方法代码片段的存储

1. 方法代码片段属于.class字节码文件的一部分，字节码文件在（classLoader加载器寻找HelloWorld.class）类加载的时候，将其放到了方法去中。所以JVM中的三块主要的内存空间中**方法区内存**最先有数据，存放了代码片段。
2. 代码片段虽然在方法区内存当中只有一份，但是可以被重复调用。每次调用这个方法的时候需要给该方法分配**独立的活动场所**，在栈内存中分配。
3. 方法调用，给方法分配内存，压栈；方法结束之后，释放分配内存，弹栈

### 类与对象

类描述对象的共同特征

**类描述的是 状态和动作**（属性+方法）

#### 类的定义

类{

状态：属性

动作：方法；

}

```java
//学生类
public class Student{
	//属性【描述对象的状态信息】
	//属性一般是通过变量的形式完成定义的（int String boolean）
	//在类体中，方法体之外定义的变量被称为“成员变量”

	//学号
	int no;
	//姓名
	String name;
	//年龄
  int age;

	//方法
  //方法买哦书的是对象的动作信息

  //唱歌

  //跳舞

}
```

##### java两种数据类型

1. 基本数据类型

   short

   long

   int

   boolean

   String

   char

2. 引用数据类型

   String.class SUN公司定义

   System.class SUN公司定义

   Student.class 我定义

   MingShen.class 我定义

   ....

   **java中所有的类都为引用数据类型**

#### 对象的创建和使用

```java
public class OOtest01{
  public static void main(String[] args){
    //int 是基本数据类型
    //i 是变量名
    //10 是一个int类型的字面值
    int i = 10;

    //通过一个类可以实例化N个对象
		//实例化对象语法 new 类名（）；
		//new运算符作用是创建对象，在JVM中
    //new运算符的作用是创建对象，在JVM堆内存中开辟新的内存空间
    //方法区内存：再类加载的时候，clss字节码代码片段被加载到内存空间当中
    //栈内存（局部变量）：方法代码片段执行的时候，会给该方法分配内存空间，在栈内存中压栈
    //堆内存：new studeng（）；

    //Student 是引用数据类型
    //s 是变量名
    //new Student是一个学生对象
    Student s = new Student();

  }
}
```

**当程序执行到new时的内存图如下**

![image-20210527190012042](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210527190012042.png)

Non, age, sex, name, addr都是**实例变量**

student这个new出来的对象的**内存地址**给了s，s时**局部变量**，s为**引用**

什么是对象？new运算符在堆内存中开辟的内存空间称为对象

什么是引用？引用是一个变量，只不过这个变量中保存了另一个java对象的内存地址

java语言当中程序员只能通过“引用”去访问对内存当中对象内部的实例变量

访问实例变量的内存数据

读取数据：引用.变量名

修改数据：引用.变量名 = 值

```java
	int StuNo = s.no;
	int StuAge = s.Age；//读取
  s.no = 10;
  s.name = "MingShen";
  s.age = 10;//会把堆内存的内存值改了
  Student stu = new Student();//再次new对象 stu是引用
```

![image-20210527193017688](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210527193017688.png)

局部变量在栈内存中储存

成员变量中的实例变量在堆内存的java对象内部储存

实例变量是一个对象一份，100个对象有100份

```java
System.out.printIn(stu.age);//10
System.out.printIn(student.no);//报错
```

报错因为no这个实例变量不能直接采用“类名”的方式访问

因为no是实例变量，对象级别的变量，变量储存在Java对象内部，必须先有对象通过对象才能访问no这个实例变量，不能直接通过“类名”

成员变量没有手动赋值，系统赋默认值

| 数据类型                  |               |
| ------------------------- | ------------- |
| byte， short， int， long | 0             |
| float，double             | 0.0           |
| char                      | \u000         |
| 引用数据类型              | null （空值） |
|                           |               |
|                           |               |

```java
public class Address{
  String city;
  String street;
  String zipcode;
}

```

```java
public class User{
  //属性【以下都是成员变量之实例变量】
  int no;//int基本数据类型，no是实例变量
  String name;//String引用数据类型，name是引用满是实例变量
  Address addr;//Address引用数据类型，addr是引用
}
```

```java
public class OOTest02
{
  public static void main(String[] args){
    //创建User对象
    //u是局部变量
    //u是一个引用
    //u保存内存地址指向堆内存的User对象
    User u = new User()；//（从右往前）
    u.no = 110;
    u.name = "jack";//字符串比较特殊不用new，jack是一个Java对象，属于String对象
    u.addr = new Address();//等号右边先执行，Address对象三个none、none、none

    //main方法只有一个引用u
    //一切只能通过u来访问
    System.out.println(u.name + "居住在" + u.addr.city);
  }
}
```

![image-20210527194926715](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210527194926715.png)

![image-20210527195317973](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210527195317973.png)

**找出所有引用**

![image-20210527195451792](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210527195451792.png)

空引用访问“实例”相关数据一定会造成空指针异常 NullPointerException

实例相关数据表示访问时必须有对象参与

### 类怎么定义

[修饰符列表]class 类名{

属性；

方法；

}

现实世界当中超市中的商品，发现这所有的商品都要有一些共同的特征，例如，每一个商品都有编号，每一个商品都有单价，所以定义以下的类，来代表所有商品。

```java
public class Product{//为什么要写class？因为要做超市的管理系统，超市有商品，商品有共同的特征，想到了编号，单价，所以利用基本数据类型（int double），定义了两个变量。
	//编号【数字，整数】
  int productNo;
  //productNo是基本数据类型
  //单价【数字，小数】
  double price;
  //price 是基本数据类型

  Student s= null;
}
Product pro = new Product{};//实例变量需要new出来才能引用，引用方式如下
pro.price = 0x1123123;//访问 可以（读一读）（改一改）
```

**product是实例变量，下面对他进行访问**

```java
public class ProductTest{
  public static void main(string[] args){
    //创建对象，商品对象
    //iphoneProMax局部变量
    //iphoneProMax引用
    //iphoneProMax变量中保存内存地址指向堆内存当中的商品对象
    Product iphoneProMax = new Product();
    //访问实例变量的语法： 引用.变量名
    System.out.printIn("商品编号："+iphoneProMax.productNo);
    iphoneProMax.price = 8000.0；
    System.out.println("商品单价："+iphoneProMax.price);
  }

```

```java
/*
	要求定义一个汽车类
		*品牌：字符串
		*颜色：字符串
		*号牌：字符串
		*购买总价：浮点型数据
*/
public class Car{
  //属性【成员变量之：实例变量】
  //总价
  基本数据类型
  double price;
  //品牌
  引用数据类型
  String brand;
  //颜色
  //引用数据类型
  String color;
  //号牌
  //引用数据类型
  String No;
}
```

```java
/*
	房屋类(不太行)
*/
public class house{
  double proportion;
  String owner;//想知道户主的身份信息，但是不能在下一行定义实例变量ID，会误解为房子的ID

}

```

```java
/*
	房屋类（行）
*/
public class house{
  double proportion;
  Person owner ;//new是个地址（其中就是联系电话、就是地址、就是各种信息）
}
public class Person{
  //名字
  String name;
  //身份证号
  String id;
  //性别
  boolean sex;
  //年龄
  int age;
  //妻子
  Wife wife；
}
```

### 封装

1. 对外提供简单的操作入口，照相机原理很复杂，但是操作很简单
2. 提高安全性
3. 以后程序可以重服使用
4. 封装后才形成真正的对象，“独立体”

```java
public class User
  public static void main()
```

#### 封装的步骤

1. 属性私有化，是用private进行修饰，private表示私有，修饰后的所有数据只能在本类中访问。

2. 对外提供简单的入口

   对外提供两个方法（get，set）

#### 背会

sette and getter方法没有static关键字

有static关键字修饰的方法怎么调用：类 名.方法名（实参）；

没有static关键字修饰的方法：引用.方法名（实参）

```java
public class User{
  private int age;
  public void setAge(int a){
  if(a < 0 || a >150 ){
    System.out.println("对不起，您给的年龄不合法")
    return;
  }
  age = a;
	}
 	public int getAge(){
  return age;
}
}

```

封装之后

```java
public class UserTest{
  public static void main(String[] srgs){
    User user = new User();
    user.setAge(10);
    System.out.println(user.getAge());
  }
}
```

#### 构造方法

1. 构造方法又称构造函数，返回值类型不需要指定，加上void int 就变成普通方法了

2. 构造方法的作用：通过方法的调用创建对象。

3. 构造方法的方法名必须和类名保持一致

4. 每个构造方法实际上执行结束之后都有返回值，但是这个return不需要写，返回值就是类本身

5. [修饰符列表]构造方法方法名（形参数）

   {方法体}；

   }

```java
public class ConstructorTest01{
    public static void main(String[] args){
        //创建User对象
        //调用User类的构造方法来完成对象的创建
        //以下程序创建了4个对象，只要构造函数调用就会查u你感觉爱你对象，并且在“堆内存”开辟内存空间

        User u2 = new User(10);
        User u3 = new User("gujm");
        ConstructorTest01.dosome();
        dosome();
        ConstructorTest01 t = new ConstructorTest01();
        t.doOther();
    }
    //调用带有static的方法：类名
    public static void dosome(){
        System.out.println("do some!");
    }
    //调用没有static的方法：引用
    //doOther方法在ConstructorTest01类中，所以需要创建ConstructorTest01对象
    //创建ConstructorTest01对象之后，调用无参数构造方法
    public void  doOther(){
        System.out.println("Do other!");
    }
}
```

```java
public class User{

    public User(int i){
        System.out.println("带有int类型构造器");
    }
    public User(String name){
        System.out.println("带有String类型构造器");
    }
}

```

构造方法作用：创造对象，给实例对象赋值

```java
public class ConstructorTest01{
    public static void main(String[] args) {
        Account act1 = new Account();
        act1.setActno("111");
        System.out.println(act1.getActno());
        Account act2 = new Account("10");
        System.out.println(act2.getActno());
        System.out.println(act1.getBalance());
    }
}
```

```java
public class Account {
    private String actno;//实例变量，创建对象之后才会有实例变量，实例方法在构造方法过程中完成赋值
    private double balance;
    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public double getBalance() {
        return balance;
    }

    public void setBalance(double balance) {
        this.balance = balance;
    }
    public Account(){
        System.out.println("空参数方法");
    };
    public Account(String s){
        actno = s;
    }

}
```

### 参数传递

传的永远是变量保存的值

```java
public class ConstructorTest01{
    public static void main(String[] args) {
        User u = new User(20);
      //User User = 0x1234
      //传递u给add方法的时候，实际上传递的是u变量中保存的值，只不过这个值是一个java对象的内存地址
        add(u);
        System.out.println(u.age);
    }
    public static void add(User u){
        u.age++;
        System.out.println("add-->" + u.age);
    }
    static class User{
        int age;
        public User(int i){
            age = i;
        }
    }
}
```

![image-20210620092032503](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210620092032503.png)

第二个例子

```java
User u =0x1234;
User u1= u;//u传递给了u1，实际上是吧0x1234这个值赋值给了u1，u和u1实际上是两个不同的局部变量
//但是他们的两个变量指向堆内存中同一个java对象
```

### this

this 是一个 关键字，翻译为：这个，用在方法中

this是一个引用，this是一个变量，this变量中保存内存地址指向了自身，this储存子啊JVM堆内存java对象内部

创建100个java对象，每个对象都有this，也就是说有100个不同的this

**this代表当前正在做（执行实例方法）这个动作的对象**

不能用在static方法中

在static方法中不能“直接”访问实例变量 实例方法

因为实例都需要对象存在

而static中没有this，也就是说，当前对象是不存在的

自然无法访问当前对象的实例方法

**用来区分实例变量和局部变量时 this不能省**

```java

```

this（）这种语法只能放在构造方法第一行

### static

应该采用类名的方式访问，即使new了对象访问static方法，其本质也用的是类名，与引用指向的对象无关，哪怕（t = null）也不会出现空指针异常

```java
public class Chinese{
 private String id;
 private String name;
 //国籍每个对象由于都是Chinese类的实例化，所以每个中国人国籍都是Chinese
 //实例对象在堆内存
 //String country;//所以没必要定义为实例变量,没必要让每一个对象都占内存，改为下一行
 static String country = "中国";//类级别特征，变量前加static，静态变量不需要创造对象，内存就开辟了，在方法区内存
}
```

![image-20210624165417791](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210624165417791.png)

#### 什么时候成员变量申明为静态变量？

所有的对象都有这个属性，并且所有对象的这个属性值一样

#### 什么时候成员变量申明为静态变量？

所有对象都有这个属性，不同对象值不同或属性值随对象值改变

#### static使用方式

静态变量在类加载的时候u初始化，内存在方法区开辟，访问时不需创造对象，直接**类名.变量名**

#### 使用static定义“静态代码块”

1.语法格式：

​ static {

java语句;

​ }

2.静态代码在类加载时执行，并且只执行一次

3.静态代码块在一个类中可以编写多个，从上往下

4.静态代码块的作用？什么时候用

​ 静态代码时java为程序员准备的一个特殊时刻，在类加载时刻，若需要此刻还在一段特殊程序，可以直接放到静态代码中

#### 静态方法

描述一个动作，所有对象执行这个动作最终产生的影响一样，不再属于某一个对象动作

大多数“工具类”的方法都是静态方法，方便使用

### 继承

继承的基本作用：代码复用，但是继承最重要的作用时：有了继承才有了以后方法的覆盖和多态机制

[修饰符列表]class 类名 extends 父类名{

​ 类体 = 属性 + 方法

}

java语言的继承制支持单继承，一个类不能同时继承很多类，只能继承一个类。

B类继承A类，A称为父类，基类，超类，superclass

​ B称为子类，派生类，subclass

私有不支持继承，构造方法不支持继承

### 方法覆盖

又称方法重写override/Overwrite

父类中的方法满足不了子类中的业务需求，有必要重写方法

就是重新写一遍**完全一样复制**方法，方法区中代码自己改，方法名不变，返回类型不变，参数列表不变，抛出异常不能更低，不能更少，访问权限不能更低可以更高

animal.move(); cat extend animal;

私有方法不能继承不能覆盖

构造方法不能继承不能覆盖

静态方法不存在覆盖（多态后）

### 多态

animal/ bird /cat

cat和bird没有任何继承关系

面向对象三大特征：封装 继承 多态

多态中涉及到的几个概念

- 向上转型（upcasting）

  子类型 --> 父类型

  自动类型转换

- 向下转型（downcasting）

  父类型 --> 子类型

  强制类型转换

![image-20210625084016091](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210625084016091.png)

```java
public class Animal {
    public void move(){
        System.out.println("动物在行走");
    }
}
```

```java
 public class Cat extends Animal{
    public void move(){
        System.out.println("猫在走猫步");
    }
    public void catchMouse(){
        System.out.println("猫抓老鼠");
    }
}
```

```java
public class Bird extends Animal{
    public void move(){
        System.out.println("鸟在飞翔");
    }
}
```

```java
public class ConstructorTest01{
    public static void main(String[] args) {
        Animal a = new Animal();
        a.move();

        Cat b = new Cat();
        b.move();
        b.catchMouse();

        Bird c = new Bird();
        c.move();

        //使用多态

        /*
        Cat is an Animal
        父类型引用指向子类型对象
        new Cat()创建的对象类型为Cat，a2这个引用的数据类型是Animal，可见他们进行了类型转换
        子类型转换成了父类型，称为向上转型，自动类型转换
        */
        Animal a2 = new Cat();
   /*    java两个阶段 编译阶段· 运行阶段

编译无法通过程序无法运行

编译阶段编译器检查a2这个引用的数据类型为Animal.class,字节码中有move方法，所以编译通过了，这个过程我们称为静态绑定，编译阶段绑定

在程序运行阶段，JVM堆内存当中真实创建的对象是Cat对象啊，那么下面程序在运行阶段一定调用Cat对象的move()方法，此时发生程序动态绑定
        */
        a2.move();
      /*
      	a2.catchMouse();
      	编译阶段就出错了
      */
      Cat c2 = (Cat)a2;
      c2.catchMouse();

      //以下程序编译没问题
      //但是运行会出现异常，因为JVM堆内存中真正存在的对象是Bird类型，Bird对象无法转换为Cat对象，因为两种类型之间不存在任何继承关系，此时出现了著名的异常
      //java.lang.ClassCastException
      //类型转换异常，向下转型时会发生
      Animal a3 = new Bird();
      cat c3 = (Cat)a3;
      /*
      以上异常只有在强制类型转换时才会发生，这么说向下转型存在隐患
      向上转型只要编译通过一定能运行，
      向下转型，运行可能错误java.lang.ClassCastException
      */
    }
}
```

#### 什么时候需要向下转型

当调用的属性和方法是在子类型中特有的，父类型中不存在

#### 避免向下转型ClassCastException异常

（引用instanceof 数据类型名）

（a instanceof Animal）

true表示：

​ a引用指向的对象是Animal

false。。。

```java
if(a3 instanceof Bird){
  Bird b2 = (Bird)a3;
  b2.fly;
}
else if(a3 instanceof Cat){
  Cat c3 = (Cat)a3;
  C2.catchMouse();
}
```

耦合度低 扩展力强

### final

1最终的、不可变得

2final修饰的类无法**被继承**

3final修饰的方法不能被**覆盖**

4变量赋值后不能**再赋值**

5实例变量加final需要手动赋值 final int age = 0；

6final 修饰的对象无法改变指向别的对象

#### 常量定义语法格式

```java
public static final 类型 常量名 = 值；
//常量名必须大写 单词之间下划线连接
public static final String GUO_JI = "中国";
public static final double PI = 3.14159;
```

### package && import

在java源程序第一行编写package语句

package 包名;

包名命名规范：

公司域名倒序 +项目名oa +模块名 +功能名;

com.bjpowernode.oa.user.service;

包名必须全部小写，也是标识符

一个包对应一个目录

import com.bjpowernode.oa.user.service；

import语句用来倒入其他类，同一个包下的类不需要导入

import 类名;

import 包名.\*;

Java.lang.\* 不需要手动引入，系统自动

import java.util.\*

4

### 访问控制权限

> ​ public 表示公开的
>
> ​ protected 同包下可以访问，子类可以访问
>
> ​ 缺省 同包
>
> ​ private 表示私有，只能在本类访问

缺省和protected出了包之后就

### super

super关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。x

#### 构造器

子类是不继承父类的构造器（构造方法或者构造函数）的，它只是调用（隐式或显式）。如果父类的构造器带有参数，则必须在子类的构造器中显式地通过 **super** 关键字调用父类的构造器并配以适当的参数列表。

如果父类构造器没有参数，则在子类的构造器中不需要使用 **super** 关键字调用父类构造器，系统会自动调用父类的无参构造器

```java
class SuperClass {
  private int n;
  SuperClass(){
    System.out.println("SuperClass()");
  }
  SuperClass(int n) {
    System.out.println("SuperClass(int n)");
    this.n = n;
  }
}
// SubClass 类继承
class SubClass extends SuperClass{
  private int n;

  SubClass(){ // 自动调用父类的无参数构造器
    System.out.println("SubClass");
  }

  public SubClass(int n){
    super(300);  // 调用父类中带有参数的构造器
    System.out.println("SubClass(int n):"+n);
    this.n = n;
  }
}
// SubClass2 类继承
class SubClass2 extends SuperClass{
  private int n;

  SubClass2(){
    super(300);  // 调用父类中带有参数的构造器
    System.out.println("SubClass2");
  }

  public SubClass2(int n){ // 自动调用父类的无参数构造器
    System.out.println("SubClass2(int n):"+n);
    this.n = n;
  }
}
public class TestSuperSub{
  public static void main (String args[]){
    System.out.println("------SubClass 类继承------");
    SubClass sc1 = new SubClass();
    SubClass sc2 = new SubClass(100);
    System.out.println("------SubClass2 类继承------");
    SubClass2 sc3 = new SubClass2();
    SubClass2 sc4 = new SubClass2(200);
  }
}
输出结果为：

------SubClass 类继承------
SuperClass()
SubClass
SuperClass(int n)
SubClass(int n):100
------SubClass2 类继承------
SuperClass(int n)
SubClass2
SuperClass()
SubClass2(int n):200
```

```java
do {
            str = br.readLine();
            System.out.println(str);
        } while (!str.equals("end"));
    }
```

除了static内部类,内部类中不允许static静态变量

### 值传递

![image-20210727142922916](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210727142922916.png)

![image-20210727144149656](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210727144149656.png)

```java
/**
 * @author Gujm
 * @date 2021/7/27 8:14 下午
 */
public class BankTest{
    public static void main(String[] args) {
        Bank bank = new Bank();
        bank.addCustomer("G","jm");
        bank.getCustomer(0).setAccount(new Account(2000));
        bank.getCustomer(0).getAccount().withdraw(500);
        double balance = bank.getCustomer(0).getAccount().getBalance();
        System.out.println(bank.getCustomer(0).getFirstName()+"账户余额为"+balance);
    }
}
 class Bank {
    private Customer[] customers;
    private int numberOfCustomers;//客户个数
    public Bank(){
        customers = new Customer[10];
    }
    //添加客户
    public void addCustomer(String f, String l){
        Customer cust = new Customer(f,l);
        customers[numberOfCustomers++] = cust;
    }
    //获取客户个数
    public int getNumberOfCustomers(){
        return numberOfCustomers;
    }
    //获取制定位置的客户
    public Customer getCustomer(int index){
        if(index >= 0 && index < numberOfCustomers){
        return customers[index];
        }else{
            return null;
        }
    }
}
 class Account{
    private double balance;
    public Account(double init_balance){
        this.balance = init_balance;
    }
    public double getBalance() {
        return balance;
    }
    public void deposit(double amt){
        if(amt > 0){
            balance += amt;
            System.out.println("存钱成功");
        }
    }
    public void withdraw(double amt){
        if (balance >= amt){
            balance -= amt;
            System.out.println("取钱成功");
        }
        else {
            System.out.println("余额不足");
        }
    }
}
 class Customer{
    private String firstName;
    private String lastName;
    private Account account;
    public Customer(String f, String l){
        this.firstName = f;
        this.lastName = l;
    }
     public String getFirstName() {
         return firstName;
     }

     public String getLastName() {
         return lastName;
     }
     public void setAccount(Account account) {
         this.account = account;
     }
     public Account getAccount() {
         return account;
     }
}
```

### equals()

String、Date 、 File、Io包装都重写了equals()方法, 只比较实体内容是否相等.

### 基本数据类型包装类

```java
String str1 = "123";

int num2 = Integer.parseInt(str1);

System.out.println(num2+1);

String str2 = "true1";

Boolean b1 = Boolean.parseBoolean(str2);

sout(b1);
```

### 多线程

```java
package java3;

/**
 * @author Gujm
 * @date 2021/8/21 2:15 下午
 */
class Clerk {
    private int productCount = 0;

    //生产
    public synchronized void produceProduct() {
        if (productCount < 20) {
            productCount++;
            System.out.println(Thread.currentThread().getName() + "开始生产第" + productCount);
        }else{
            //等待
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    //消费
    public synchronized void consumeProduct() {
        if (productCount > 0){
            System.out.println(Thread.currentThread().getName() + "开始消费第" + productCount);
            productCount--;
        }else {
            //等待
            try {
                wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class Producer extends Thread {
    private Clerk clerk;

    public Producer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(getName() + "开始生产。。");
        while (true) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.produceProduct();
        }
    }
}

class Customer extends Thread {
    private Clerk clerk;

    public Customer(Clerk clerk) {
        this.clerk = clerk;
    }

    @Override
    public void run() {
        System.out.println(getName() + "开始消费。。");
        while (true) {
            try {
                Thread.sleep(20);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            clerk.consumeProduct();
        }
    }
}

public class ProductTest {
    public static void main(String[] args) {
        Clerk clerk = new Clerk();
        Producer p1 = new Producer(clerk);
        p1.setName("生产者1");
        Customer c1 = new Customer(clerk);
        c1.setName("消费者1");
        Customer c2 = new Customer(clerk);
        c2.setName("消费者2");
        p1.start();
        c1.start();
        c2.start();
    }
}
```

### 常用类

#### String

### ArrayList常用方法

```java
增:add(Object obj)

删:remove(int index) remove(Object obj)

改:set(int index, Object ele)

查:get(int index)

插:add(int index, Object ele)

遍历:1

Iterator迭代器方式

Iterator iterator = list.iterator();

while(iterator.hasNext()){

​		System.out.println("****");

}

遍历:2

增强for循环

for(Object obj : list){

​		System.out.println(obj);

}

System.out.println("***");



遍历:3

普通for循环

for(int i = 0; i < list.size(); i++){

​		System.out.println(list.get(i));

}



没听
```

![image-20210905000354527](https://36038098-1323630637.cos.ap-nanjing.myqcloud.com/images/image-20210905000354527.png)
