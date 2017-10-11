# java反射

## 一，概述 {#一，概述}

java动态代理只能对方法进行反射，为什么？  
[![](http://sqtds.github.io/img/2015/反射.png)](http://sqtds.github.io/img/2015/反射.png)

## 二，Class {#二，Class}

getXXX\(\)返回的为public的XXX\(方法，构造方法，属性,注解\)，包括继承的和接口实现的。

getDeclaredXXX\(\)返回的为本类的所有的XXX\(方法，构造方法，属性，注解\)，不包括继承的。

### 1,Methods {#1,Methods}

* public Method\[\] getMethods\(\)  
  返回某个类的所有公用（public）方法包括其继承类的公用方法，当然也包括它所实现接口的方法。

* public Method\[\] getDeclaredMethods\(\)  
  返回某个类的所有方法，包括公共、保护、默认（包）访问和私有方法，但不包括继承的方法。当然也包括它所实现接口的方法。

### 2,Constructors {#2,Constructors}

* `public Constructor getConstructor(Class<?>… parameterTypes)`

  根据参数返回公共构造方法。

* public Constructor  
  &lt;  
  ?  
  &gt;  
  \[\] getConstructors\(\)

  得到所有的公共构造方法

* public Constructor  
  &lt;  
  ?  
  &gt;  
  \[\] getDeclaredConstructors\(\)

  得到声明的构造方法

### 3,Fields {#3,Fields}

同上

### 4,Annotations {#4,Annotations}

同上，只能获取注解集合，暂时不能获取指定参数注解。

## 三，AccessibleObject {#三，AccessibleObject}

[![](http://sqtds.github.io/img/2015/AccessibleObject.png)](http://sqtds.github.io/img/2015/AccessibleObject.png)  
从图中我们可以看到Methods，Constructors，Fields继承了这个类。  
我们看这个类的方法：  
[![](http://sqtds.github.io/img/2015/AccessibleObjectMethod.png)](http://sqtds.github.io/img/2015/AccessibleObjectMethod.png)  
从图中可以看出，这个对象主要包含了2类方法：

* 第一类是获取对象注解，getAnnotations\(\)。
* 第二类是设置对象是否可访问，setAccessible\(boolean flag\),通过设置访问标志来访问非public对象（方法，构造方法，属性）。

## 五.Modifiers {#五-Modifiers}

我们可以看到修饰符使用的是二进制来表示的，访问修饰符用01,10,100，1000来表示的，总共容纳32个修饰符，每一位使用mod&修饰符来判断当前值是否包含修饰符。

```
public
static
final
int
PUBLIC
           = 
0
x00000001;

public
static
final
int
PRIVATE
          = 
0
x00000002;

public
static
final
int
PROTECTED
        = 
0
x00000004;

public
static
final
int
STATIC
           = 
0
x00000008;
...
Modifier.isAbstract(
int
 modifiers)
Modifier.isFinal(
int
 modifiers)
Modifier.isInterface(
int
 modifiers)
Modifier.isNative(
int
 modifiers)
Modifier.isPrivate(
int
 modifiers)
Modifier.isProtected(
int
 modifiers)
Modifier.isPublic(
int
 modifiers)
Modifier.isStatic(
int
 modifiers)
Modifier.isStrict(
int
 modifiers)
Modifier.isSynchronized(
int
 modifiers)
Modifier.isTransient(
int
 modifiers)
Modifier.isVolatile(
int
 modifiers)
```

## 六.InvocationHandler {#六-InvocationHandler}

| 1 2 3 4 5 6 7 8 | Proxy publicstatic Object newProxyInstance\(ClassLoader loader,                   Class&lt;?&gt;\[\] interfaces,                   InvocationHandler h\) 上述方法与下面的方法 完全等同，都是构造一个代理实例。 Proxy.getProxyClass\(loader, interfaces\). getConstructor\(new Class\[\] { InvocationHandler.class }\). newInstance\(new Object\[\] { handler }\); |
| :--- | :--- |


从这里可以看出，任何一个代理类都会有一个以InvocationHandler为参数的构造方法，通过这个方法，我们可以创建一个代理类。  
然后我们看看InvocationHandler的接口实现：

| 1 | public Object invoke\(Object proxy, Method method, Object\[\] args\) |
| :--- | :--- |


从这里看出，参数仅仅能传Method，所以只能对方法进行代理。

## 七，Array {#七，Array}

注意不要混淆java.lang.reflect.Array与java.util.Arrays。

### 1，创建数组 {#1，创建数组}

```
int
[]
 intArray = (
int
[]
) 
Array
.newInstance(
int
.
class
, 
3
);
```

创建一个大小为3，类型为int的数组。

### 2，访问数组 {#2，访问数组}

```
Array
.
set
(intArray, 
0
, 
123
);
```

设置数组的第一个值为123。

### 3，从数组中获取类对象 {#3，从数组中获取类对象}

```
Class
 stringArrayClass = 
String
[].
class
;

Class
 intArray = 
Class
.forName(
"[I"
);

Class
 stringArrayClass = 
Class
.forName(
"[Ljava.lang.String;"
);
```

## 八，jdk8改进 {#八，jdk8改进}

反射异常ClassNotFoundException、NoSuchMethodException、IllegalAccessException，InvocationTargetExcetpion等有一个父类异常叫ReflectiveOperationExcetpion。

旧版JDK，还有个很莫名其妙的地方，就是所有反射，都拿不到参数名，无论名字叫啥，都返回arg0，arg1，所以在CXF，SpringMVC里，你都要把参数名字用annotation再写一遍：  
Person getEmployee\(@PathParam\(“dept”\) Long dept, @QueryParam\(“id”\) Long id\)  
现在，JDK8新提供的类java.lang.reflect.Parameter可以反射参数名了，编译时要加参数，如 javac -parameters xxx.java，或者Eclipse里设置。然后就可以写成:  
Person getEmployee\(@PathParam Long dept, @QueryParam Long id\)

## 九，参考资料 {#九，参考资料}

[Java Reflection Tutorial](http://tutorials.jenkov.com/java-reflection/index.html)

