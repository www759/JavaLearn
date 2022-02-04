# Objects类

- Objects类与Object还是继承关系, Objects类是从JDK1.7之后才有的





## 1. equals方法

**public static boolean equals([Object](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html) a, [Object](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html) b)**

Returns `true` if the arguments are equal to each other and `false` otherwise. Consequently, if both arguments are `null`, `true` is returned. Otherwise, **if the first argument is not `null`, equality is determined by calling the [`equals`](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Object.html#equals(java.lang.Object)) method of the first argument with the second argument of this method.** Otherwise, `false` is returned.

如果第一个参数不为null, 那么该方法就会调用第一个参数的equals方法与第二个参数进行比较

```java
public static boolean equals(Object a, Object b) {
    return (a == b) || (a != null && a.equals(b));
}
```





## 2. isNull方法

判断对象是否为null, 如果是null返回true

