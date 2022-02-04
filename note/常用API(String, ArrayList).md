# 常用API(String)

## 一. String

- 存储字符串, 操作字符串

  ### 1. String的内存原理

- **以""方式给出的字符串对象, 在字符串常量池中(存在于堆中)存储**

![image-20220103162636918](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220103162636918.png)

- **String对象中存储的依然是地址**

- **原来的字符串对象都是没有改变的, 所以称为不可变字符串**

  ### 2. String创建字符串对象的两种方式

- 方式一: 直接使用" " 定义(String name = "woaini"; )

- 方式二: 通过String类的构造器创建对象

  

![image-20220103163222542](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220103163222542.png)

```java
public static void main(String[] args) {
    char[] chars = {'a', 'b', 'c', 'd'};

    String s = new String(chars);
    System.out.println(s);//abcd
}
```

```java
public static void main(String[] args) {
    byte[] bytes = {97, 98, 99, 65};
    String s = new String(bytes);
    System.out.println(s);//abcA
}
```

	### 3. 注意事项

- 以""给出的字符串对象, 在字符串常量池中存储, 而且**相同内容只会在其中存储一份**
- 通过构造器new对象, **每new一次产生一个新对象, 放在堆内存中**
- **注意注意: ==比较的是地址**

```java
public static void main(String[] args) {
    String s1 = "abc";
    String s2 = "abc";
    System.out.println(s1 == s2);//ture

    String s3 = new String("abc");//实际创建了两个对象 abc和s3
    String s4 = "abc";//创建了0个对象
    System.out.println(s3 == s4);//false
}
```

- **理解: 构造器总是在堆中申请一片新的空间, 将拷贝对象的值赋给新的对象, 返回新对象的地址**

![image-20220103165025898](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220103165025898.png)

```java
public static void main(String[] args) {
    String s1 = "abc";
    String s2 = "a" + "b" + "c";
    System.out.println(s1 == s2);//true
}
```

- **java存在编译优化机制, 程序在编译时就会将"a"+"b"+"c"会直接转为"abc", 因为这已经时确定的值, 没必要放到执行阶段再转化**

### 4. String类常用API

#### 	1. 比较字符串是否相等

- **注意不能使用====来比较, 基本数据类型使用====**

- **使用equals**

![image-20220103174154461](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220103174154461.png)

```java
public static void main(String[] args) {
    String name = "abc";
    String key = "Abc";

    System.out.println(name.equals(key));//false
    
    //忽略大小写比较, 一般用于验证码比较
    System.out.println(name.equalsIgnoreCase(key));//true
}
```

#### 	2. 其他常用API

![image-20220103174657500](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220103174657500.png)

```java
public static void main(String[] args) {
    String name = "abclove you";
    //获取字符串长度
    System.out.println(name.length());//11

    //取某一字符
    char c = name.charAt(1);
    System.out.println(c);
    //遍历字符串的每个字符
    for (int i = 0; i < name.length(); i++) {
        char ch = name.charAt(i);
        System.out.println(ch);
    }

    //转换为字符数组
    char[] chars = name.toCharArray();
    System.out.println(chars);

    //截取, 包前不包后
    String name2 = name.substring(0, 5);
    System.out.println(name2);

    //从当前索引截到最后
    String name3 = name.substring(1);
    System.out.println(name3);

    //替换
    String name4 = "金三胖是最厉害的80后, 金三胖棒棒的!";
    String rs = name4.replace("金三胖", "***");
    System.out.println(rs);

    //匹配
    System.out.println(name4.contains("金三胖"));

    //判断是否以某个字符串开始
    System.out.println(name4.startsWith("金三胖"));
    System.out.println(name4.startsWith("金二胖"));

    //按传入的规则切割字符串
    String name5 = "王宝强,贾乃亮,陈羽凡";
    String[] names = name5.split(",");

    for (int i = 0; i < names.length; i++) {
        System.out.println(names[i]);
    }
}
```

