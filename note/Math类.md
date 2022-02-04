# Math类

## 概述

- 包含执行基本数字运算的方法, 没有公开的构造器, 不能对外创建对象

- 类的成员都是静态的, 通过类名就可以访问

https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Math.html

**demo: **

```java
public static void main(String[] args) {
    //1. 取绝对值
    System.out.println(Math.abs(-100.1));//100.1

    //2. 向上取整
    System.out.println(Math.ceil(100.0001));//101

    //3. 向下取整
    System.out.println(Math.floor(99.9999));//99.0

    //4. 求指数
    System.out.println(Math.pow(2, 3));//8

    //5. 四舍五入
    System.out.println(Math.round(4.49999));//4

    //6. 比较大小
    System.out.println(Math.max(100.1, 100));

    //7. 随机数
    System.out.println(Math.random());//[0.0-1.0)
}
```

