# java对象大小

## 对象大小 {#对象大小}

[![](http://sqtds.github.io/img/2015/java_object_size.png)](http://sqtds.github.io/img/2015/java_object_size.png)

### 1，64位系统 {#1，64位系统}

new java.lang.Object\(\) 占用了 16 bytes  
new byte\[0\] 占用了 24 bytes

```
class A {
 byte x;
}
class B extends A {
 byte y;
}
```

new A\(\) 占用了 24 bytes  
new B\(\) 占用了 32 bytes= 24+pad A\(8\)

```
class C {
 Object obj = new Object();
}
```

new C\(\) takes 40 bytes= obj+obj ref +C = 16+8+16

### 2，参考资料 {#2，参考资料}

* [一个Java对象到底占多大内存？](http://www.importnew.com/14948.html)
* [everything-i-ever-learned-about-jvm-performance-tuning-twitter](http://www.infoq.com/presentations/JVM-Performance-Tuning-twitter)

  




