# Java 基础知识相关
## Java中 == 和 equals 和 hashCode 的区别

对于关系操作符 ==
- 若操作数的类型是**基本数据类型**，则该关系操作符判断的是左右两边操作数的**值**是否相等
- 若操作数的类型是**引用数据类型**，则该关系操作符判断的是左右两边操作数的**内存地址**是否相同。**也就是说，若此时返回true,则该操作符作用的一定是同一个对象。**


对于使用 equals 方法，内部实现分为三个步骤：

1. 先比较引用是否相同(是否为同一对象),
2. 再判断类型是否一致（是否为同一类型）,
3. 最后比较内容是否一致

Java 中所有内置的类的 equals 方法的实现步骤均是如此，特别是诸如 Integer，Double 等包装器类。如以下 `String` 中的 `equals` 方法实现

```Java
// String.java
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String anotherString = (String)anObject;
            int n = value.length;
            if (n == anotherString.value.length) {
                char v1[] = value;
                char v2[] = anotherString.value;
                int i = 0;
                while (n-- != 0) {
                    if (v1[i] != v2[i])
                        return false;
                    i++;
                }
                return true;
            }
        }
        return false;
    }
```

hashcode是系统用来快速检索对象而使用

equals方法本意是用来判断引用的对象是否一致

重写equals方法和hashcode方法时，equals方法中用到的成员变量也必定会在hashcode方法中用到,只不过前者作为比较项，后者作为生成摘要的信息项，本质上所用到的数据是一样的，从而保证二者的一致性

##### 参考
- [ Java 中的 ==, equals 与 hashCode 的区别与联系](http://blog.csdn.net/justloveyou_/article/details/52464440)
- Effective Java 第 8，9 条

## int、char、long各占多少字节数
Java 基本类型占用的字节数
- 1字节： byte , boolean
- 2字节： short , char
- 4字节： int , float
- 8字节： long , double

注：1字节(byte)=8位(bits)

- [相关文章](http://blog.csdn.net/yushulinfengprc/article/details/70227156)

## int与integer的区别
- Integer是int的包装类，int则是java的一种基本数据类型
- Integer变量必须实例化后才能使用，而int变量不需要
- Integer实际是对象的引用，当new一个Integer时，实际上是生成一个指针指向此对象；而int则是直接存储数据值
- Integer的默认值是null，int的默认值是0

##### 参考
- [相关文章](https://www.cnblogs.com/guodongdidi/p/6953217.html)

## 对 Java 多态的理解
> 多态是指父类的某个方法被子类重写时，可以产生自己的功能行为，同一个操作作用于不同对象，可以有不同的解释，产生不同的执行结果。

多态的三个必要条件：

1. 继承父类。
2. 重写父类的方法。
3. 父类的引用指向子类对象

然后可以使用结合里氏替换法则进一步的谈理解

> 里氏替换法则 ---- 所有引用基类的地方必须能透明地使用其子类的对象

- 子类必须完全实现父类的方法

> 注意  在类中调用其他类时务必要使用父类或接口，如果不能使用父类或接口，则说明类的设计已经违背了LSP原则

> 注意  如果子类不能完整地实现父类的方法，或者父类的某些方法在子类中已经发生“畸变”，则建议断开父子继承关系，采用依赖、聚集、组合等关系代替继承

- 子类可以有自己的个性
- 覆盖或实现父类的方法时输入参数可以被放大
- 覆写或实现父类的方法时输出结果可以被缩小

> 在项目中，采用里氏替换原则时，尽量避免子类的“个性”，一旦子类有“个性”，这个子类和父类之间的关系就很难调和了， 把子类当做父类使用，子类的“个性”被抹杀——委屈了点;把子类单独作为一个业务来使用，则会让代码间的耦合关系变得扑朔迷离——缺乏类替换的标准

## String、StringBuffer、StringBuilder 区别
StringBuffer 和 String 一样都是用来存储字符串的，只不过由于他们内部的实现方式不同，导致他们所使用的范围不同，对于 StringBuffer 而言，他在处理字符串时，若是对其进行修改操作，它并不会产生一个新的字符串对象，所以说在内存使用方面它是优于 String 的

StringBuilder 也是一个可变的字符串对象，他与 StringBuffer 不同之处就在于它是线程不安全的，基于这点，它的速度一般都比 StringBuffer 快

**String 字符串的拼接会被 JVM 解析成 StringBuilder 对象拼接，在这种情况下 String 的速度比 StringBuffer 的速度快**

str +=”b”等同于
str = new StringBuilder(str).append(“b”).toString();

- [相关文章](http://wiki.jikexueyuan.com/project/java-enhancement/java-fourteen.html)

## 详解内部类
内部类使得多重继承的解决方案变得更加完整

使用内部类最吸引人的原因是：每个内部类都能独立地继承一个（接口的）实现，所以无论外围类是否已经继承了某个（接口的）实现，对于内部类都没有影响

使用内部类才能实现多重继承

- 内部类可以用多个实例，每个实例都有自己的状态信息，并且与其他外围对象的信息相互独立。
- 在单个外围类中，可以让多个内部类以不同的方式实现同一个接口，或者继承同一个类。
- 创建内部类对象的时刻并不依赖于外围类对象的创建。
- 内部类并没有令人迷惑的“is-a”关系，他就是一个独立的实体。
- 内部类提供了更好的封装，除了该外围类，其他类都不能访问。

> 当我们在创建某个外围类的内部类对象时，此时内部类对象必定会捕获一个指向那个外围类对象的引用，只要我们在访问外围类的成员时，就会用这个引用来选择外围类的成员

#### 成员内部类
在成员内部类中要注意两点，

- **成员内部类中不能存在任何 static 的变量和方法**
- 成员内部类是依附于外围类的，所以只有先创建了外围类才能够创建内部类

#### 静态内部类
静态内部类与非静态内部类之间存在一个最大的区别，我们知道非静态内部类在编译完成之后会隐含地保存着一个引用，该引用是指向创建它的外围内，但是静态内部类却没有。没有这个引用就意味着：
- 它的创建是不需要依赖于外围类的。
- 它不能使用任何外围类的非static成员变量和方法。

#### 参考
- [相关文章](http://wiki.jikexueyuan.com/project/java-enhancement/java-eight.html)

## 为什么Java里的匿名内部类只能访问final修饰的外部变量？

匿名内部类用法

```java
public class TryUsingAnonymousClass {
    public void useMyInterface() {
        final Integer number = 123;
        System.out.println(number);

        MyInterface myInterface = new MyInterface() {
            @Override
            public void doSomething() {
                System.out.println(number);
            }
        };
        myInterface.doSomething();

        System.out.println(number);
    }
}
```

编译后的结果

```java
class TryUsingAnonymousClass$1
        implements MyInterface {
    private final TryUsingAnonymousClass this$0;
    private final Integer paramInteger;

    TryUsingAnonymousClass$1(TryUsingAnonymousClass this$0, Integer paramInteger) {
        this.this$0 = this$0;
        this.paramInteger = paramInteger;
    }

    public void doSomething() {
        System.out.println(this.paramInteger);
    }
}
```

因为匿名内部类最终用会编译成一个单独的类，而被该类使用的变量会以构造函数参数的形式传递给该类，例如：Integer paramInteger，如果变量
不定义成final的，paramInteger在匿名内部类被可以被修改，进而造成和外部的paramInteger不一致的问题，为了避免这种不一致的情况，因为Java 规定匿名内部类只能访问final修饰的外部变量

- [匿名内部类相关文章](http://wiki.jikexueyuan.com/project/java-enhancement/java-ten.html)
- [Java 面试题集](https://github.com/guoxiaoxing/android-interview/blob/master/doc/Java%E9%9D%A2%E8%AF%95%E9%A2%98%E9%9B%86.md)

## 抽象类和接口的区别
1. 默认的方法实现抽象类可以有默认的方法实现完全是抽象的。接口根本不存在方法的实现
2. 实现抽象类使用 extends 关键字来继承抽象类。如果子类不是抽象类的话，它需要提供抽象类中所有声明的方法的实现。子类使用关键字 implements 来实现接口。它需要提供接口中所有声明的方法的实现。
3. 抽象类可以有构造器，而接口不能有构造器
4. 抽象方法可以有public、protected和default这些修饰符。接口方法默认修饰符是public。你不可以使用其它修饰符。
5. 抽象类在java语言中所表示的是一种继承关系，一个子类只能存在一个父类，但是可以存在多个接口。
6. 抽象方法比接口速度要快，接口是稍微有点慢的，因为它需要时间去寻找在类中实现的方法。
7. 如果往抽象类中添加新的方法，你可以给它提供默认的实现。因此你不需要改变你现在的代码。 如果你往接口中添加方法，那么你必须改变实现该接口的类

##### 参考
- [相关文章](https://www.jianshu.com/p/038f0b356e9a)

## 泛型中 extends 和 super 的区别

- [相关文章](https://itimetraveler.github.io/2016/12/27/%E3%80%90Java%E3%80%91%E6%B3%9B%E5%9E%8B%E4%B8%AD%20extends%20%E5%92%8C%20super%20%E7%9A%84%E5%8C%BA%E5%88%AB%EF%BC%9F/)

## 父类的静态方法能不能被子类重写
静态方法与静态成员变量可以被继承，但是不能被重写。它对子类隐藏，因此静态方法也不能实现多态

- [相关文章](https://www.jianshu.com/p/df43f5500ea3)

## 进程和线程的区别
进程和线程的主要差别在于它们是不同的操作系统资源管理方式。进程有独立的地址空间，一个进程崩溃后，在保护模式下不会对其它进程产生影响，而线程只是一个进程中的不同执行路径。线程有自己的堆栈和局部变量，但线程之间没有单独的地址空间，一个线程死掉就等于整个进程死掉，所以多进程的程序要比多线程的程序健壮，但在进程切换时，耗费资源较大，效率要差一些。但对于一些要求同时进行并且又要共享某些变量的并发操作，只能用线程，不能用进程。

- [相关文章](http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html)
- [相关文章](https://www.cnblogs.com/lmule/archive/2010/08/18/1802774.html)

## final, finally, finalize的区别
- final 用于声明属性,方法和类, 分别表示属性不可变, 方法不可覆盖, 类不可继承.

- finally 是异常处理语句结构的一部分，表示总是执行.

- finalize 是Object类的一个方法，在垃圾收集器执行的时候会调用被回收对象的此方法，可以覆盖此方法提供垃圾收集时的其他资源回收，例如关闭文件等. JVM不保证此方法总被调用.

##### 参考
- [相关文章](http://wiki.jikexueyuan.com/project/java-interview-question/java/final,_finally,_finalize.html)

## Parcelable和Serializable的区别
Serializable是Java中的序列化接口，其使用起来简单但 是开销很大，序列化和反序列化过程需要大量I/O操作。而Parcelable是Android中的序列化方式，因此更适合用在Android平台上，它的缺点就是使用起来稍微麻烦 点，但是它的效率很高，这是Android推荐的序列化方式，因此我们要首选Parcelable。Parcelable主要用在内存序列化上，通过Parcelable将对象序列化到存储设备 中或者将对象序列化后通过网络传输也都是可以的，但是这个过程会稍显复杂，因此在这两种情况下建议大家使用Serializable。

##### 参考
- [相关文章](https://www.cnblogs.com/trinea/archive/2012/11/09/2763213.html)
- Android 开发艺术探索

## 为什么非静态内部类里不可以有静态属性
- [相关文章](https://blog.csdn.net/WuchangI/article/details/79182850)

## 谈谈对kotlin的理解
因人而异，请自行整理答案

## String 转换成 integer的方式及原理
```Java
Integer a = 2;
private void test() {
    String s1 = a.toString();  //方式一
    String s2 = Integer.toString(a);  //方式二
    String s3 = String.valueOf(a);  //方式三
}
```

方式一源码：
```Java
    public String toString() {
        return toString(value);
    }

    public static String toString(int i) {
        if (i == Integer.MIN_VALUE)
            return "-2147483648";
        int size = (i < 0) ? stringSize(-i) + 1 : stringSize(i);
        char[] buf = new char[size];
        getChars(i, size, buf);
        return new String(buf, true);
    }
```

> 可以看出`方式一`最终调用的是`方式二`

**通过toString()方法，可以把整数（包括0）转化为字符串，但是 Integer 如果是 null 的话，就会报空指针异常**

方式三源码：

```Java
public static String valueOf(Object obj) {
return (obj == null) ? "null" : obj.toString();
}
public String toString() {
return getClass().getName() + "@" + Integer.toHexString(hashCode());
}
```

> 可以看出 当 Integer 是null的时候，返回的String是 字符串 "null" 而不是 null