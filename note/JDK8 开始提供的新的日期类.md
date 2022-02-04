# JDK8 开始提供的新的日期类



## 一. LocalDate, LocalTime, LocalDateTime

- 分别代表日期, 时间, 日期时间对象. 他们的类的实例是不可变的对象
- 特们三者构建对象和API都是通用的

- **不可变对象, 每次修改返回的是新对象, 原对象不变**





- ```java
  public static void main(String[] args) {
      //1. 获取本地日期对象
      LocalDate nowDate = LocalDate.now();
      System.out.println("date : " + nowDate);
  
      //2. 分别获取年月日
      System.out.println("year: " + nowDate.getYear());
      System.out.println("month: " + nowDate.getMonth());
      System.out.println("day: " + nowDate.getDayOfMonth());
  
      //...
  
      //----------------------//
      //指定某个时间
      LocalDate ld = LocalDate.of(1999, 8, 26);
      System.out.println(ld);
  
  }
  ```



- LocalTime, LocalDateTime与LocalDate相似



- LocalDateTime转换为另两个对象

  ```java
  LocalDate ld = localdatetime.toLocalDate();
  LocalTime lt = localdatetime.toLocalTime();
  ```

  ```java
  public LocalDate toLocalDate();
  public LocalTime toLocalTime();
  ```

  

![image-20220121191037922](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220121191037922.png)

## 二. Instant

```java
public static void main(String[] args) {
    //1. 得到一个Instant时间戳对象
    //得到的是世界标准时间
    Instant instant = Instant.now();
    System.out.println(instant);


    //2. 得到系统此刻的时间戳
    Instant instant1 = Instant.now();
    System.out.println(instant1.atZone(ZoneId.systemDefault()));

    //3. 如何返回Date对象
    Date date = Date.from(instant);
    System.out.println(date);

    Instant i2 = date.toInstant();
    System.out.println(i2);
}
```



## 三. DateTimeFormatter

- 全新的日期与时间格式器

```java
public static void main(String[] args) {
    //本地此刻日期时间对象
    LocalDateTime ldt = LocalDateTime.now();
    System.out.println(ldt);//2022-01-21T19:32:40.832930400

    //解析/格式化器
    DateTimeFormatter dtf = DateTimeFormatter.ofPattern("yyyy-MM-dd HH-mm-ss EEE a");
    //正向格式化
    System.out.println(dtf.format(ldt));//2022-01-21 19-32-40 周五 下午

    //逆向格式化
    System.out.println(ldt.format(dtf));//2022-01-21 19-32-40 周五 下午

    //解析字符串时间
    DateTimeFormatter dtf1 = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    //解析当前字符串时间为本地日期时间对象
    LocalDateTime ldt1 = LocalDateTime.parse("2019-11-11 11:11:11", dtf1);

    System.out.println(ldt1.getDayOfYear());
    System.out.println(ldt1.getDayOfMonth());
}
```





## 四. Duration/Period

- Period
  - 计算日期间隔差异: java.time.Period
  - 方法: getYears(), getMonths(), getDays()来计算, 只能精确到年月日
  - 用于LocalDate之间的比较

```java
public static void main(String[] args) {
    //当前本地 年月日
    LocalDate today = LocalDate.now();
    System.out.println(today);

    //生日的年月日
    LocalDate birthDate = LocalDate.of(1999, 8, 26);
    System.out.println(birthDate);

    Period period = Period.between(birthDate, today);
    System.out.println(period.getYears());
    System.out.println(period.getMonths());
    System.out.println(period.getDays());
}
```

- Duration
  - 计算时间间隔差异: java.time.Duration
  - 提供了使用基于时间的值测量时间量的方法
  - 用于LocalDateTime之间的比较, 也可用于Instant之间的比较

```java
public static void main(String[] args) {
    //本地日期时间对象
    LocalDateTime today = LocalDateTime.now();
    System.out.println(today);

    //出生的日期时间对象
    LocalDateTime birthDate = LocalDateTime.of(2022, 1, 22, 6, 0, 0);
    System.out.println(birthDate);

    Duration duration = Duration.between(birthDate, today);
    System.out.println(duration.toDays());
    System.out.println(duration.toHours());
    System.out.println(duration.toMinutes());
    System.out.println(duration.toMillis());
    System.out.println(duration.toNanos());
}
```



## 五. ChronoUnit

