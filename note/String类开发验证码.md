# String类开发验证码

1.  定义一个字符串存储a~z A~Z 0~9的所有字符
2. 循环n次, 随机n个索引, 对应n个不同的字符

```java
public static void main(String[] args) {
    String s = "";
    for(int i = 0; i < 26; ++i){
        s += (char)('a' + i);
    }
    for(int i = 0; i < 26; ++i){
        s += (char)('A' + i);
    }
    for(int i = 0; i < 10; ++i){
        s += (char)('0' + i);
    }

    Random r = new Random();
    String result = "";
    for(int i = 0; i < 5; ++i){
        int id = r.nextInt(s.length());
        result += s.charAt(id);
    }

    System.out.println(result);
}
```

