

@[toc]

[**Github**地址](https://github.com/Yexiaomo/interview)

[**CSDN**地址](https://blog.csdn.net/qq_32603745)

## 一、基础知识（必会）
#### 1.1 JDK，JRE和JVM的区别与联系有哪些？
**答: 首先要熟悉其基本概念**

|分类|概念|
|-|-|
|JDK(Java Development Kit) |是一个开发工具包, 是Java开发环境的核心组件, 并提供编译, 运行和调试一个Java程序所需要的工具, 可执行文件和二进制文件, 是一个平台特定的软件. |
|JRE(Java Runtime Environment)| 是指Java运行时环境, 是JVM的实现, 提供了运行Java程序的平台. JRE包含了JVM, 但不包含Java编译器/调试器之类的开发工具.|
|JVM (Java Virtual Machine) |是指Java虚拟机, 当我们运行一个程序时, JVM负责将字节码转换为特定机器码, JVM提供了内存管理/垃圾回收和安全机制等.|

**区别于联系**

- JDK是开发工具包, 用来开发JAva程序, 而JRE是Java的运行时环境
- JDK和JRE中都包含了JVM
- JVM是Java编程的核心, 独立于硬件和操作系统, 具有平台无关性, 而这也是*Java程序可以一次编写, 多处执行的原因*

>延伸① Java语言的平台无关性是如何实现的？
>答:
>
> - JVM屏蔽了操作系统和底层硬件的差异
> - Java面向JVM编程, 先编译生成字节码文件, 然后交给JVM解释成机器码执行
> - 通过规定基本数据类型的取值范围和行为

>延伸② Java语言是编译型还是解释型语言?
>答:
>
>Java的执行经历了编译和解释的过程，是一种先编译，后解释执行的语言，不可以单纯归到编译性或者解释性语言的类别中。

#### 1.2 接口和抽象类有什么区别?
**答:** 首先需要知道, 定义接口需要用到`interface`, 定义抽象类需要用到关键字`abstract`关键字, 区别还有以下几点

- 抽象类中可以没有抽象方法, 也可以抽象方法和非抽象方法共存.
- 接口中的方法在JDK8之前只能是抽象的(默认就是抽象的), 从JDK8版本可以通过`default`实现在接口中创建默认方法.
- 抽象类和类一样是单继承, 但接口可以实现多个父接口
- 抽象类中可以存在普通的成员变量; 接口中的变量必须是`static`, `final`类型;必须被初始化, *接口中只有常量, 没有变量*;

>延伸① 抽象类和接口应该如何选择？分别在什么情况下使用呢？
>答:
>根据抽象类和接口不同之处, 当我们仅仅需要定义一些抽象方法而不需要额外的具体方法或变量时, 就使用接口, 反之使用抽象类.

>延伸② 在JDK>= 1.8中接口通过default关键字实现默认方法
> 
>
```java
public interface DemoInterface {
    // 在方法前面添加default
    default void say(String msg){
        System.out.println("Hello " + msg);
    }
    // 普通的抽象方法
    void test();
}
```
当一个类实现该接口时，可以继承到该接口中的默认方法
```java
public class Demo implements DemoInterface {
    public static void main(String[] args){
        Demo demo = new Demo();
        demo.say("world");
    }
    @Override
    public void test(){}
}

>>Hello world
```
**注意**如果继承了多个接口中都出现了*同样* 的`default`方法, 如果不加以修改,会报错.  需要
- 重写多个接口中的相同的默认方法
- 或者在实现类中指定要使用哪个接口中的默认方法

#### 1.3 Java中的8种基本数据类型及其取值范围
可通过`.MAX_VALUE`和`.MIN_VALUE`取得范围

|类型|所占字节数|
|-|-|
|`byte`|1字节|
|`short`|2字节|
|`int`|4字节|
|`long`|8字节|
|`float`|4字节|
|`double`|8字节|
|`char`|2字节|
|`boolean`|Java中并没有规定Boolean所占字节数|

#### 1.4 Java中元注解有哪些?
答:Java中提供了`4`个元注解, 元注解的作用是负责注解其他注解

- @Target : 说明注解所修饰的对象范围,其值在ElementType中, 其源码
```java
public @interface Target { 
    ElementType[] value(); 
} 
public enum ElementType { 
    TYPE,FIELD,METHOD,PARAMETED,CONSTRUCTOR,LOCAL_VARIABLE,ANNOCATION_TYPE,PACKAGE,TYPE_PARAMETER,TYPE_USE 
} 
```
例如，如下的注解使用@Target标注，表明MyDemo注解就只能作用在类/接口和方法上。
```java
@Target({ElementType.TYPE, ElementType.METHOD}) 
public @interface MyDemo { 
}
```
- @Retention : 保留策略, 定义了该注解被保留的时间长短,其源码
```java
public @interface Retention {
    RetentionPolicy value();
}
public enum RetentionPolicy {
    SOURCE, CLASS, RUNTIME
}
```
`SOURCE`表明在源文件中保留(即源文件保留, 编译器将丢掉), `CLASS`表明在class文件中有效(即class保留, VM将丢掉), `RUNTIME`表明在运行时有效(即运行时保留)

- @Documented : 用于描述其他类型的annotation应该被作为被标注的程序成员的公共API,因此可以被javadoc此类的工具文档化, Documented是一个标记注解,没有成员,其源码
```java
public @interface Documented {
}
```
- @Inherited : 也是一个标记注解, 其表明被注解的类型是被继承的, 如果一个使用@Inherited修饰的annotation类型被用于一个class, 则这个annotation将被用于该class的子类.
```java
public @interface Inherited {
}
```

#### 1.5 Java反射机制
指在运行过程中, 对于任意一个类都能知道这个类的属性和方法; 对于任意一个对象都能调用它的任意一个方法和属性. 即动态获取信息和动态调用对象方法的功能称为为`反射机制`.  其作用:

- 在运行时, 判断任意一个对象所属的类
- 在运行时, 构造任意类的对象
- 在运行时, 判断任意对象所具有成员变量及方法
- 在运行时, 调用任意对象的方法, 生成动态代理

反射相关的类

- Class : 表示类, 用于获取类相关信息
- Field : 表示成员变量, 用于获取类的实例变量和静态变量等
- Method : 表示方法, 用于获取类中的`方法参数`和`方法类型`
- Constructor : 表示构造器, 用于获取构造器的方法参数和和类型等

反射举例及优缺点[看这个->JVM的1.5章节](https://blog.csdn.net/qq_32603745/article/details/103588358)

    ------------------------**持续更新中**-------------------