# BigDecimal

## 使用步骤

- 创建对象BigDecimal封装浮点型数据, 但不建议如此做

  ```java
  public static BigDecimal valueOf(double val);//包装浮点数成为BigDecimal对象
  ```

  

![image-20220117153627190](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220117153627190.png)

```java
public static BigDecimal valueOf(double val) {
    // Reminder: a zero double returns '0.0', so we cannot fastpath
    // to use the constant ZERO.  This might be important enough to
    // justify a factory approach, a cache, or a few private
    // constants, later.
    return new BigDecimal(Double.toString(val));
}
```





- **Demo:**

```java
public static void main(String[] args) {
    double a = 0.1;
    double b = 0.2;
    System.out.println(a + b);//0.30000000000000004

    BigDecimal a1 = BigDecimal.valueOf(a);
    BigDecimal b1 = BigDecimal.valueOf(b);
    BigDecimal c1 = a1.add(b1);
    System.out.println(c1);//0.3
}
```



- BigDecimal转为double

  ```java
  double rs = c1.doubleValue();
  ```

  

- **注意: BigDecimal一定是要精度运算, 无法精确运算会报错**

```java
		//注意: BigDecimal一定要精度运算
        BigDecimal a11 = BigDecimal.valueOf(10.0);
        BigDecimal b11 = BigDecimal.valueOf(3.0);
		/**
         * 参数一: 除数 参数二: 保留小数位数 参数三: 舍入模式
         */
        BigDecimal c11 = a11.divide(b11, 2, RoundingMode.HALF_UP);//保留两位 四舍五入
        System.out.println(c11);
```

