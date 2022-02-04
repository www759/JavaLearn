# static关键字

## 一. static修饰成员变量

- static是静态的意思, 可以修饰成员变量,表示该成员变量在内存中只有一份, 可以被共享,访问,修改

  **所有实例共享相同成员变量**

- 成员变量可以分为两类:
  - 静态成员变量(有static修饰, 属于类, 内存中加载一次)
  - 实例成员变量(存在于每个对象中)

### 1. 访问

- 访问静态成员变量:

  **类名.静态成员变量 (推荐)**

  **对象.静态成员变量(不推荐)**

- 访问实例成员变量:

  对象.实例成员变量

```java
package d1_static;

public class User {
    //在线人数信息,静态成员变量
    public static int onlineNumber;

    //实例成员变量
    private String name;
    private int age;

    public static void main(String[] args) {
        //1. 类名访问静态成员变量
        User.onlineNumber++;
        //在同一个类中访问静态成员变量, 类名可以省略不写
        System.out.println(onlineNumber);

        //2. 实例成员变量 对象.实例成员变量
        //System.out.println(name); 报错
        User u1 = new User();
        u1.name = "wang";
        u1.age = 36;
        System.out.println(u1.name);
        System.out.println(u1.age);

        //3. 成员变量访问静态成员变量
        System.out.println(u1.onlineNumber);

        User u2 = new User();
        u2.name = "wang";
        u2.age = 36;
        u2.onlineNumber++;

        System.out.println(onlineNumber);
    }
}
```

### 2. 内存原理

加载类时会同时加载静态成员变量,在堆内存

![image-20220106000824531](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220106000824531.png)

## 二. static修饰成员方法

- 成员方法的分类:
  - 静态成员方法(有static修饰, 属于类), 建议用类名访问, 也可以用对象访问
  - 实例成员方法(属于对象), 只能用对象触发访问

- 使用场景
  - 表示对象自己行为的, 且方法中需要访问实例成员的, 则该方法声明为实例方法
  - 如果该方法是以执行一个通用功能为目的, 或者需要方便访问, 则可以声明为静态方法

```java
package d2_static_method;

public class Student {
    private String name;
    private int age;

    /**
        实例方法: 无static修饰, 属于对象的, 通常表示对象自己的行为, 可以访问对象的成员变量
     */
    public void study(){
        System.out.println(name + "在好好学习, 天天向上!");
    }

    /**
        静态方法: 有static修饰, 属于类, 可以被类和对象共享访问
     */
    public static void getMax(int a, int b){
        System.out.println(a > b? a : b);
    }

    public static void main(String[] args) {
        //1. 类名.静态方法
        Student.getMax(10, 100);
        //注意: 同一个类中访问静态成员 可以省略类名不写
        getMax(200, 100);

        //2. 对象.实例方法
        //study();错误
        Student s = new Student();
        s.name = "全蛋";
        s.study();

        //3. 对象.静态方法(不推荐)
        s.getMax(300, 210);
    }
}
```

### 内存模型

![image-20220106125723584](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220106125723584.png)

## 三. 应用: 工具类

工具类:

- 对于一些应用程序中多次需要用到的功能, 可以将这些功能封装为静态方法, 放在一个类中, 这个类就是工具类
- 工具类的作用: 一是方便调用, 二是提高了代码复用
- 建议将工具类的构造器私有, 不让工具类对外产生对象

```java
package d3_static_demo;

import java.util.Random;

public class VerifyTool {
    /**
     * 私有构造器
     */
    private VerifyTool(){
    }

    /**
     * 静态方法
     * 生成一个n位验证码
     */
    public static String createCode(int n){
        //使用Sting开发一个验证码
        String chars = "qwertyuiopasdfghjklzxcvbnmQWERTYUIOPLKJHGFDSAZXCVBNM0123456789";
        //定义一个变量存储验证码
        String code = "";

        Random r = new Random();
        for(int i = 0; i < n; ++i){
            int index = r.nextInt();
            code += chars.charAt(index);
        }

        return code;
    }
}
```

```java
package d3_static_demo;

public class ArrayUtils {
    private ArrayUtils(){
    }


    public static String toString(int[] arr){
        if(arr == null) return null;

        String s ="[";
        for(int i = 0; i < arr.length; ++i){
            s += (i != arr.length - 1? arr[i] + "," : arr[i]);
        }
        s += "]";
        return s;
    }

    //去掉最高分和最低分, 返回平均成绩
    public static double getAverage(int[] arr){
        if(arr == null || arr.length == 0) return 0.0;

        double result = 0.0;
        int max = arr[0], min = arr[0];
        for(int i = 0; i < arr.length; ++i){
            result += arr[i];
            if(max < arr[i])    max = arr[i];
            if(min > arr[i])    min = arr[i];
        }
        result = result - max - min;

        return result / (arr.length - 2);
    }
}
```

## 四. 注意事项

- **静态方法只能访问静态成员, 但不可以直接访问实例成员, 只能创建对象去访问**
- 实例方法可以访问静态的成员, 也可以访问实例成员 
- **静态方法中是不可以出现this关键字的**

```java
package d4_static_attention;

public class Test {
    public static int onlineNumber;
    public String name;

    public static void getMax(){
        //静态方法访问静态成员
        System.out.println(onlineNumber);
        //静态方法访问静态方法
        inAddr();
    }

    public static void inAddr(){
        System.out.println("==========");
    }

    public void run(){
        //实例方法访问静态成员
        System.out.println(onlineNumber);
        //实例方法访问实例成员
        System.out.println(name);
        //实例方法访问静态方法
        getMax();
        //实例方法访问实例方法
        sing();
    }

    public void sing(){

    }
    public static void main(String[] args) {

    }
}
```

