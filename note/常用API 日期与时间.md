# 常用API: 日期与时间

## 一. Date类

- Date类的对象在Java中代表的是当前所在系统的此刻日期时间

```java
public static void main(String[] args) {
    //1. 创建一个Date类的对象: 代表系统刺客日期时间对象
    Date d = new Date();
    System.out.println(d);
}
```

- Date构造器

  - 无参构造器

  ```java
  public new Date();
  ```

  - 有参构造器

  ```java
  public Date(long time);//把时间毫秒值转换成Date日期对象
  ```

  

- Date类常用方法

  - 获取对象时间毫秒值

  ```java
  public long getTime(); //获取时间对象的毫秒值
  ```

  - 设置日期对象的时间为当前时间毫秒值对应的时间

  ```java
  public void setTime(long time) 
  ```

  

- 案例: 计算当前时间往后走1小时121秒后的时间

```java
//案例: 计算当前时间往后走1小时121秒后的时间
//1. 输出当前时间毫秒值
Date d1 = new Date();
System.out.println(d1);

//2. 计算当前时间 + 1小时121秒对应的毫秒值
long time2 = System.currentTimeMillis();
time2 += (60 * 60 + 121) * 1000;

//3. 把时间毫秒值转换成对应的时间日期对象
Date d2 = new Date(time2);
System.out.println(d2);


//案例第二种实现
Date d3 = new Date();
d3.setTime(time2);
System.out.println(d3);
```





## 二. SimpleDateFormat类

- 可以把**Date对象或时间毫秒值格式化成我们喜欢的时间形式**
- 也可以把字符串的时间形式**解析**成日期对象

![image-20220117192313489](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220117192313489.png)

![image-20220117195540071](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220117195540071.png)

- 格式化日期对象: format方法

  ```java
  public static void main(String[] args) {
          //1. 日期对象
          Date d = new Date();
          System.out.println(d);
  
          //2. 格式化这个日期对象
          SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss EEE a");
          String rs = sdf.format(d);
  
          System.out.println(rs);
  }
  ```



- 格式化时间毫秒值

  ```java
  //3. 格式化时间毫秒值
  long time1 = System.currentTimeMillis();
  String rs2 = sdf.format(time1);
  System.out.println(rs2);
  ```



- 解析字符串时间: parse方法

  ```java
  //案例: 计算2021年08月06日11点11分11秒, 往后走2天14小时49分06秒后的时间
          String dateStr = "2021.08.06 11:11:11";
  
          SimpleDateFormat sdf2 = new SimpleDateFormat("yyyy.MM.dd HH:mm:ss");
          Date d4 = sdf2.parse(dateStr);
  
          long time2 = d4.getTime() + (2L*24*60*60+14*60*60+49*60+6)*1000;
          System.out.println(sdf2.format(time2));
  ```

- 小案例

```java
public static void main(String[] args) throws ParseException {
    String startTime = "2020-11-11 00:00:00";
    String endTime = "2020-11-11 00:10:00";

    String xiaoJia = "2020-11-11 00:03:47";
    String xiaoPi = "2020-11-11 00:10:11";

    SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
    Date d1 = sdf.parse(startTime);
    Date d2 = sdf.parse(endTime);
    Date d3 = sdf.parse(xiaoJia);
    Date d4 = sdf.parse(xiaoPi);

    if(d3.after(d1) && d3.before(d2)){
        System.out.println("successful");
    }else{
        System.out.println("fail");
    }

    if(d4.after(d1) && d4.before(d2)){
        System.out.println("successful");
    }else{
        System.out.println("fail");
    }
}
```

## 三. Calendar类

- 代表了系统此刻日期对应的日历对象
- Calendar是一个抽象类, 不能直接创建对象





- 获取Calendar对象

  ```java
  Calendar rightNow = Calendar.getInstance();
  ```

  

- 常用方法

  ```java
  public static void main(String[] args) {
          //1. 拿到此刻日历对象
          Calendar cal = Calendar.getInstance();
          System.out.println(cal);
  
          //2. 获取日历的信息
          int year = cal.get(Calendar.YEAR);
          System.out.println(year);
          int mm = cal.get(Calendar.MONTH) + 1;
          System.out.println(mm);
          int day = cal.get(Calendar.DAY_OF_MONTH);
          System.out.println(day);
  
          //3. 修改每个字段的信息
          cal.set(Calendar.HOUR, 18);
          System.out.println(cal.get(Calendar.HOUR));
  
          //4. 为某个字段增加/减少指定的值
          cal.add(Calendar.MINUTE, 59);
  
          //5. 拿到此刻日期对象
          Date date = cal.getTime();
          System.out.println(date);
  
          //6. 拿到此刻时间毫秒值
          long time = cal.getTimeInMillis();
          System.out.println(time);
      }
  ```

  
