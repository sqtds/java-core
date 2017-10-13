# java新特性

## 一，概述 {#一，概述}

[![](http://sqtds.github.io/img/2015/%E6%96%B0%E7%89%B9%E6%80%A7.png)](http://sqtds.github.io/img/2015/%E6%96%B0%E7%89%B9%E6%80%A7.png)

## 二，泛型 {#二，泛型}

### 2.1 协变 {#2-1_协变}

数组是协变的，因为 Integer 是 Number 的子类型，数组类型 Integer\[\] 是 Number\[\] 的子类型，因此在任何需要 Number\[\] 值的地方都可以提供一个 Integer\[\] 值。另一方面，泛型不是协变的， List不是 List的子类型，试图在要求 List的位置提供 List是一个类型错误。这不算很严重的问题 — 也不是所有人都认为的错误 — 但泛型和数组的不同行为的确引起了许多混乱。

### 2.2 类型推断 {#2-2_类型推断}

当解析一个泛型方法的调用时，编译器将设法推断类型参数它能达到的最具体类型。 例如，对于下面这个泛型方法：  


```
public static<T> T identity(T arg) { return arg };
```

和它的调用：  


```
Integer i = 3;
System.out.println(identity(i));
```



编译器能够推断 T 是 Integer、Number、 Serializable 或 Object，但它选择 Integer 作为满足约束的最具体类型。

## 三，注解 {#三，注解}

注解早在J2SE1.5就被引入到Java中，主要提供一种机制，这种机制允许程序员在编写代码的同时可以直接编写元数据。  
注解基本上可以在Java程序的每一个元素上使用：类，域，方法，包，变量，等等。

### 3.1 @Retention {#3-1_@Retention}

这个注解注在其他注解上，并用来说明如何存储已被标记的注解。这是一种元注解，用来标记注解并提供注解的信息。可能的值是：

* SOURCE：表明这个注解会被编译器忽略，并只会保留在源代码中。
* CLASS:表明这个注解会通过编译驻留在CLASS文件，但会被JVM在运行时忽略,正因为如此,其在运行时不可见。
* RUNTIME：表示这个注解会被JVM获取，并在运行时通过反射获取。

### 3.2 @Target {#3-2_@Target}

这个注解用于限制某个元素可以被注解的类型。例如：

* ANNOTATION\_TYPE 表示该注解可以应用到其他注解上
* CONSTRUCTOR 表示可以使用到构造器上
* FIELD 表示可以使用到域或属性上
* LOCAL\_VARIABLE表示可以使用到局部变量上。
* METHOD可以使用到方法级别的注解上。
* PACKAGE可以使用到包声明上。
* PARAMETER可以使用到方法的参数上
* TYPE可以使用到一个类的任何元素上。

### 3.3 @Documented {#3-3_@Documented}

被注解的元素将会作为Javadoc产生的文档中的内容。注解都默认不会成为成为文档中的内容。这个注解可以对其它注解使用。

### 3.4 @Inherited {#3-4_@Inherited}

在默认情况下，注解不会被子类继承。被此注解标记的注解会被所有子类继承。这个注解可以对类使用。

### 3.5 @Repeatable {#3-5_@Repeatable}

说明该注解标识的注解可以多次使用到同一个元素的声明上。

### 3.6 @FunctionalInterface {#3-6_@FunctionalInterface}

这个注解表示一个函数式接口元素。函数式接口是一种只有一个抽象方法（非默认）的接口。编译器会检查被注解元素，如果不符，就会产生错误。

## 四，lambada表达式 {#四，lambada表达式}

## 五，参考资料 {#五，参考资料}

1. [Java 理论与实践: 使用通配符简化泛型使用](http://www.ibm.com/developerworks/cn/java/j-jtp04298.html)
2. [Java 理论和实践: 了解泛型](http://www.ibm.com/developerworks/cn/java/j-jtp01255.html)
3. [java泛型](http://www.cnblogs.com/panjun-Donet/archive/2008/09/27/1300609.html)
4. [Java 注解指导手册 – 终极向导](http://www.importnew.com/14227.html)
5. [Java深度历险（六）——Java注解](http://www.infoq.com/cn/articles/cf-java-annotation/)
6. [深入理解Java 8 Lambda（语言篇——lambda，方法引用，目标类型和默认方法）](http://www.cnblogs.com/figure9/archive/2014/10/24/4048421.html)



