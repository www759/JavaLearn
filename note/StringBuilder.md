# StringBuilder

## 一. 概述

- StringBuilder是一个可变的字符串类, 我们可以把它看成一个**对象容器**

- 作用: **提高字符串的操作效率**, 如拼接, 修改等
- **demo1:**

```java
public static void main(String[] args) {
    StringBuilder sb = new StringBuilder();
    sb.append("A");
    sb.append(5);
    sb.append(false);
    sb.append(5.5);
    
    System.out.println(sb);//A5false5.5
}
```

- **支持链式编程**

```java
//支持链式编程
        StringBuilder s = new StringBuilder();
        s.append("a").append("b").append("c");
        System.out.println(s);//abc
```

- **反转**

```java
//反转后拼接
s.reverse().append("100");
System.out.println(s);//cba100
```

- **取长度**

```java
System.out.println(s.length());
```

**注意: StringBuilder只是拼接字符串的手段, 效率好. 最终的结果还是要恢复成String类**



- **恢复为String**

```java
String rs = s.toString();
```



## 二. 性能好的原因

- String类拼接字符串原理图

![image-20220117113221327](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220117113221327.png)

**采用StringBuilder只需要产生一个StringBuilder对象**



## 三. 打印数组内容demo

```java
package d8_api_StringBuilder;

public class StringBuilderTest {
    public static void main(String[] args) {
        int[] arr = {10, 10, 20, 30, 40};
        System.out.println(toString(arr));//[10, 10, 20, 30, 40]
    }

    /**
     * 接受任意整形数组返回数组内容格式
     */
    public static String toString(int[] arr){
        if(arr == null) return null;
        StringBuilder sb = new StringBuilder("[");
        for(int i = 0; i < arr.length; ++i){
            sb.append(arr[i]).append(i == arr.length-1?"" : ", ");
        }
        sb.append("]");
        return sb.toString();
    }
}
```

