# java基础（1）

### JVM、JRE和JDK的关系

### JVM

jvm是java的虚拟机，java程序需运行在虚拟机上，不同的平台有自己的虚拟机，所以java语言可以实现跨平台

### JRE

包含java虚拟机和java程序所需要的核心内库。核心内库主要是java.lang包：**包含了运行java程序必不可少的系统类，比如说基本数据类型、基本数学函数。字符串处理、线程、异常处理类等，系统缺省加载这个包**，如果只需要运行开发好的java程序，只需要安装jre即可

### JDK

提供给开发人员使用，其中**包含了java开发工具，也有JRE**，其中编译工具（javac.exe），打包工具（jar.exe）

**三者间的关系为，JDK包含了jre及开发工具等，jre包含了运行环境及jvm还有核心内库**

## 基础语法

### 数据类型

**java有哪些基础数据类型**

+ 基本数据类型
  + 数值型
    + 整数类型（**byte、short、int、long**）
    + 浮点型（**double、float**）
  + 字符型（**char**）
  + 布尔型（**boolean**）

+ 引用类型
  + 类（**class**）
  + 接口（**interface**）
  + 数组（**[]**）

### switch的key值范围

**java5以前，只支持byte、short、char、int，java5以后引入枚举，java7后引入String**，目前的版本中，暂不支持long类型

### Math.round()方法取值问题

方法使用**四舍五入**进行计算，可以理解为当前值+0.5，然后向下取整，主要注意负数的计算

### 数据类型转换

#### 1.float i = 3.4

此做法是错误的，3.4是双精度类型，也就是double类型，转化为float是向下转型，会造成精度丢失，可以使用强制转换，或者在数据后面加上F进行赋值

#### 2.short s1  = 1

对于short s1= 1 s1 = s1 + 1，这种写法是错误的，因为1是int类型，在做操作时会自动向上转型为int类型

short s1 = 1 s1 += 1,这种写法是正确的，因为在计算时会自动进行数据类型转换，相当于强制转换

### java编码类型

java语言采用Unicode标准编码，保证了每个字符的唯一性

### 访问修饰符

访问修饰符包含有**private、default、protect、public**，区别主要在访问权限、使用对象上

private:**同一类可见，可以用于变量、方法。不能用来修饰外部类**

default:**同一包内可见，可用于类、接口、变量、方法**

protect：**对同一包内、子类可见。可用于变量、方法，不能用来修饰外部类**

public：**对所有类可见。可用于类、接口、变量、方法**

### 运算符

&的运算符有两种：**按位与和逻辑与**

都是需要两边条件式为true时执行，但是&&是短路运算符，如果左边为false，则右边不进行运行

### 关键字

#### goto

**java中保留了goto关键字，但是并未使用**

#### final

用于修饰类、属性和方法

+ 被final修饰的类不可以被继承
+ 被final修饰的方法不能被重写
+ 被final修饰的变量不能被更改，不能更改，但是引用不可以被更改

#### finally

一般作用在try-catch语句块中，在处理异常时，我们将一定要执行的代码逻辑放在finally代码块中，表示不论是否发生异常，该代码块一定会执行

#### finalize

是Object类中提供的方法，主要用于垃圾回收，在我们调用System.gc()方法时，垃圾回收器调用的就是这个方法，这也是一个对象是否可被回收的判断

#### this

this是自身的一个对象，代表对象本身，可以理解为指向对象本身的一个指针

用法：

1. 普通的引用，this想当于指向当前对象本身
2. 形参与成员名字重名（构造方法）
3. 引用本类的构造方法（构造方法内引用构造方法，此时的this代表的就是本类的构造方法），切只能引用一个

#### super

super是一个指向父类的指针，也有三种用法

1. 普通的直接引用，可以直接用supper.xxx来引用父类成员

2. 子类的成员变量或方法和父类的重名时，用super进行区分

   ```java
   class Person{
   	protect String name;
   	
   	public Perrson(String name){
   		this.name = name;
   	}
       
       class Student extends Person{
           private String name;
           
           public Student(String name, String naem1){
               super(name);
               this.name = name1;
           }
           
           public void getInfo(){
               System.out.println(this.name); //子类变量
               System.out.println(super.name); //父类变量
           }
       }
   }
   ```

   

3. 引用父类构造参数，如上代码；

#### static

主要意义在于创建独立于具体对象的域变量或者方法。**即使没有创建对象，也能使用属性和调用方法**

**用来形成静态代码块以优化程序性能**：只会在类加载时执行一次，因此，很多时候会将一些只需要进行一次初始化操做都放在static代码块中进行

**静态只能访问静态**

**非静态既可以访问静态，也可以访问非静态**

#### break

跳出当前循环体

#### continue

跳出当次循环，进入下次循环

#### return

程序返回，不再执行下面的代码

#### 如何跳出多重嵌套循环

设置标记点，然后使用break即可

```java
public static void main(String[] args){
        ok:
        for(int i = 0; i < 10; i++){
            for(int j = 0; j<20; j++){
                System.out.println(i);
                if(j == 2){
                    break ok;
                }
            }
        }
    }
```

### 面向对象

三大特性：封装、继承、多态

#### 封装

隐藏对象的属性和细节实现，仅对外部提供公共访问方式，将变化隔离，便于使用，提高复用性和安全性

#### 继承

继承是多态的前提，在已有的类做为基础，建立新的类

1. 子类拥有父类的非private方法
2. 子类可以拥有自己的属性和方法
3. 子类可以用自己的方式来实现父类的方法（重写）

#### 多态

父类接口定义的引用变量可以指向子类或具体实现类的实例对象

+ 方法重写（子类继承父类，并重写父类已有的或抽象的方法）
+ 对象造型（用父类型引用子类型对象，这样的调用就会根据子类对象的不同而表现出不同的行为）

#### 面向对象的五大基本原则

+ 单一职责原则（类的功能要纯粹，不能包罗万象）
+ 开放封闭原则（面对扩展是开放的，面对修改是封闭的）
+ 里氏替换原则（子类可以替换父类出现在任何父类要出现的地方）
+ 依赖倒置原则（高层次的模块不应该依赖低层次的模块，他们都应该依赖于抽象）
+ 接口分离原则（每个功能有自己的对外接口）

### 类和接口

#### 抽象类和接口的对比

相同点：

+ 都不能被实例化
+ 都位于继承的顶端，用于被其它类继承或者实现
+ 都包含抽象方法，子类必须覆写这些方法

不同点

| 参数       | 抽象类                                                       | 接口                                                         |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| 声明       | 使用abstract声明                                             | 使用interface声明                                            |
| 实现       | 子类使用extend继承抽象类，如果子类不是抽象类的话，需要提供所有声明的方法实现 | 子类使用implents关键字来实现接口。它需要提供接口中所有声明方法的实现 |
| 构造器     | 可以有构造器                                                 | 没有构造器                                                   |
| 访问修饰符 | 任意访问修饰符                                               | public                                                       |
| 多继承     | 一个类最多只能继承一个抽象类                                 | 一个类可以实现多个接口                                       |
| 字段声明   | 可以任意声明字段                                             | 默认static和final                                            |

#### 抽象类和普通类的区别

+ 普通类不能包含抽象方法，抽象了可以有抽象方法
+ 抽象类不能直接实例化，普通类可以直接实例化
+ 抽象类不能用final修饰

###  变量和方法

#### 成员变量和局部变量的区别有哪些

变量：程序执行过程中，在某个范围能改变的值

成员变量：位于方法外，类内部定义的变量

局部变量：方法内定义的变量

#### 作用域

成员变量：整个类有效

局部变量：方法内有效，不能定义与成员变量的引用一致

#### 存储位置

成员变量：对象存在则存在，对象消失则消失，存储在堆内存中

局部变量：在方法中杯调用，存储在栈内存中，方法调用完成，自动释放

#### 初始值

成员变量：有初始值

局部变量：没有默认值，需要进行赋值

#### 构造方法

作用：完成类的初始化工作

调用子类前会先调用父类的构造方法的原因是帮助子类进行初始化工作

特性：

+ 名字与类名相同
+ 没有返回值，但是不能用void进行声明
+ 生成类时自动执行，无需调用

#### 无参构造的作用

java在执行子类的构造方法之前，如果没有super()来调用父类特点的构造方法，则会调用父类无参构造方法。java类中默认提供无参构造，但是在编写了有参构造后需要提供一个无参构造方法。

#### 静态变量和普通变量的差别

1. 静态变量被所有的对象共享，在内存中只有一个副本，只会在类初始化时加载一次
2. 普通变量不被共享，在创建对象时创建，存在多个副本，各个对象拥有的副本各不影响

#### 静态方法和实例方法的区别

+ 在外部调用静态方法时，可以使用“类名.方法名”进行直接调用
+ 静态方法在访问本类的成员时，只能访问静态成员，实例方法没有这个限制

### 内部类

在类的内部定义一个类，就是内部类，内部类可以分为四种：**成员内部类、局部内部类、匿名内部类和静态内部类**

#### 静态内部类

定义在类内部的静态类，创建方式为**new 外部类.静态内部类()**

```java
public class OuterClass {

    private static int i;

    private int j;

    static class StaticInner{
        public StaticInner() {
            System.out.println(i);
        }
    }

    public static void main(String[] args){
        new OuterClass.StaticInner();

    }
}
```

#### 成员内部类

定义在类内部，成员位置上的非静态类，创建方式为**外部类实例.new 内部类()**

```java
public class OuterClass {

    private static int i;

    private int j;

    private class MemberClass{
        public MemberClass() {
            System.out.println("创建成员内部类");
            System.out.println(i);
        }
    }

    public static void main(String[] args){
        OuterClass outerClass = new OuterClass();
        MemberClass memberClass = outerClass.new MemberClass();

    }
}
```

#### 局部内部类

定义在方法内部的类就是局部内部类

```java
public class OuterClass {

    private static int i;

    private int j;


    public static void main(String[] args){
        
        testFunction();
    }

    private static void testFunction(){
        int d;
        class Inner {
            public Inner() {
                System.out.println("创建局部内部类");
                // System.out.println(j); 编译错误，定义在静态方法中的局部类不可以访问外部类的实例变量
                System.out.println(i);
            }
        }
        Inner inner = new Inner();
    }
}
```

#### 匿名内部类

没有名字的内部类，直接使用**new 接口/类名进行创建**

```java
public class Outer {

    private void test(final int i) {
        new Service() {
            public void method() {
                for (int j = 0; j < i; j++) {
                    System.out.println("匿名内部类" );
                }
            }
        }.method();
    }
 }
 //匿名内部类必须继承或实现一个已有的接口 
 interface Service{
    void method();
}
```

+ 匿名内部类必须继承一个抽象类或者实现一个接口
+ 不能定义任何静态成员和静态方法
+ 当所在的方法的形参需要被匿名内部类使用时，必须声明为final
+ 匿名内部类不是抽象的，它必须要实现继承的类或者实现的接口的所有抽象方法

### == 和equals的区别

#### ==

判断两个对象的地址是否一致，也就是两个对象是否是同一个对象

#### equals

作用也是判断两个对象是否相等：

+ 类没有覆盖equals方法，等价于使用 “==”
+ 类覆盖了equals方法，则通过equals方法逻辑判断对象是否一致
+ String的equals方法是重写过的，对比的是值
+ 当创建String对象时，虚拟机会在常量值中查找，如果有当前的值就复制给当前的引用，如果没有就创建

```java
public class test1 {
    public static void main(String[] args) {
        String a = new String("ab"); // a 为一个引用
        String b = new String("ab"); // b为另一个引用,对象的内容一样
        String aa = "ab"; // 放在常量池中
        String bb = "ab"; // 从常量池中查找
        if (aa == bb) // true
            System.out.println("aa==bb");
        if (a == b) // false，非同一对象
            System.out.println("a==b");
        if (a.equals(b)) // true
            System.out.println("aEQb");
        if (42 == 42.0) { // true
            System.out.println("true");
        }
    }
}
```

### hashCode与equals

#### hashcode()介绍

hashCode()的作用是获取哈希码，也称之为散列码，它实际上是返回一个int整数。这个哈希码的作用是**确定该对象在哈希表中的索引位置**，方法定义在Object类中，这也就意味着java中任何一个类都有hashCode()方法

#### hashCode()和equals的相关规定

+ 如果两个对象相等，则hashCode一定是相同的
+ 两个对象相等，是用equals比较也一定是返回true
+ 两个对象有相同的hashCode值，他们也不一定是想等的

### java包

#### JDK中常用的包

+ java.lang:系统基础类
+ java.io:输入输出有关的类，比如文件操作
+ java.nio:完善io包中的功能，提高io性能的一个新的包
+ java.net：网络相关的包
+ java.util：系统辅助类，特别是集合类
+ java.sql：数据库操作类

### 反射

#### 什么是反射机制

在运行状态中，对于任意一个类，都能知道他的属性和方法；对于任意一个对象，都能够调用他的任意一个方法和属性