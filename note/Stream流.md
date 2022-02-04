# Stream流

## 一. 概述

- 在Java8中, 得益于Lambda所带来的函数式编程, 引入了一个全新的Stream概念
- **目的: 用于简化集合和数组操作的API**

![image-20220129231443464](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220129231443464.png)

```java
public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        Collections.addAll(names, "张三丰", "张无忌", "周芷若", "赵敏", "张强");
        System.out.println(names);

//        //1. 从集合中找出姓张的放到新集合
//        List<String> zhangList = new ArrayList<>();
//        for (String name : names) {
//            if(name.startsWith("张")){
//                zhangList.add(name);
//            }
//        }
//        System.out.println(zhangList);
//
//        //2. 找名称长度是3的姓名
//        List<String> zhangThreeList = new ArrayList<>();
//        for (String s : zhangList) {
//            if(s.length() == 3){
//                zhangThreeList.add(s);
//            }
//        }
//        System.out.println(zhangThreeList);

        //3. 使用Stream实现
        names.stream().filter(s -> s.startsWith("张")).filter(s -> s.length() == 3).forEach(s-> System.out.println(s));
    }
```



## 二. Stream流的思想

- 如工厂流水线一般将数据一层一层过滤

  

- **获取Stream流**
  - 创建一条Stream流, 并把数据放到流水线上准备进行操作

- **中间方法**
  - 流水线上的操作, 一次操作完毕之后还可以继续进行其他操作

- **终结方法**
  - 一个Stream流只能有一个终结方法, 是流水线上的最后一个操作



## 三. Stream流的获取

**集合获取Stream流的方式**

- 可以使用Collection接口中的默认方法stream()生成流

  ```java
  default Stream<E> stream();
  ```

**数组获取Stream流的方式**

- 调用Arrays类的静态方法

```java
public static <T> Stream<T> stream(T() array);
```

- 调用Stream类的静态方法

```java
public static<T> Stream<T> of(T... values)
```

```java
public static void main(String[] args) {
    /**----------------Collection集合获取流------------------*/
    Collection<String> list = new ArrayList<>();
    Stream<String> s = list.stream();

    /**-----------------Map集合获取流--------------------*/
    Map<String, Integer> maps = new HashMap<>();

    //键流
    Stream<String> keyStream = maps.keySet().stream();
    //值流
    Stream<Integer> valueStream = maps.values().stream();
    //键值对流
    Stream<Map.Entry<String, Integer>> keyAndValueStream = maps.entrySet().stream();

    /**-----------------数组获取流---------------*/
    String[] names = {"wang", "chu", "fei", "xu", "x"};
    Stream<String> nameStream = Arrays.stream(names);

    Stream<String> nameStream2 = Stream.of(names);
}
```



## 四. Stream流的常用API

```java
public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        Collections.addAll(list, "张无忌", "周芷若", "赵敏", "张强", "张三丰", "张三丰");

        //Stream<T> filter(Predicate<? super T> predicate);
//        list.stream().filter(new Predicate<String>(){
//            @Override
//            public boolean test(String s){
//                return s.startsWith("张");
//            }
//        });
        //1. 集合过滤
        list.stream().filter(s->s.startsWith("张")).forEach(System.out::println);

        long size = list.stream().filter(s->s.length()==3).count();
        System.out.println(size);
        
        //2. 取前两个元素
        list.stream().limit(2).forEach(System.out::println);
        

//        list.stream().map(new Function<String, String>() {
//            @Override
//            public String apply(String s) {
//                return "wang" + s;
//            }
//        });
        //Map加工方法
        //3. 给集合元素的前面加上一个： wang
        list.stream().map(s -> "wang" + s).forEach(System.out::println);

        //4.把所有的名称加工成一个学生对象
        list.stream().map(s->new Student(s)).forEach(System.out::println);
        list.stream().map(Student::new).forEach(System.out::println);//构造器引用 方法引用

        //5. 合并流
        Stream<String> s1 = list.stream().filter(s->s.startsWith("张"));
        Stream<String> s2 = Stream.of("Java1", "Java2");
        //public static <T> Stream<T> concat(Stream<? extends T> a, Stream<? extends T> b);
        Stream<String> s3 = Stream.concat(s1, s2);
        s3.forEach(System.out::println);
    }
```

**注意: 在Stream流中无法直接修改集合, 数组中的数据**



## 五. 终结流的常见终结方法

```java
void forEach(Consumer action);//对此流的每个元素执行遍历操作
```

```java
long count();//返回流中的元素数
```

- 原因: 不会返回Stream



## 六. Stream流的常见应用

![image-20220130225457397](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220130225457397.png)

```java
package d3_stream_demo;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.ArrayList;
import java.util.List;
import java.util.stream.Stream;

public class Demo {
    public static double allMoney = 0.0;
    public static double allMoney2 = 0.0;

    public static void main(String[] args) {
        List<Employee> one = new ArrayList<>();
        one.add(new Employee("猪八戒",'男', 30000, 25000, null));
        one.add(new Employee("孙悟空",'男', 25000, 1000, "顶撞上司"));
        one.add(new Employee("沙僧",'男', 20000, 20000, null));
        one.add(new Employee("小白龙",'男', 20000, 25000, null));

        List<Employee> two = new ArrayList<>();
        two.add(new Employee("武松",'男', 15000, 9000, null));
        two.add(new Employee("李逵",'男', 20000, 10000, null));
        two.add(new Employee("西门庆",'男', 50000, 100000, "被打"));
        two.add(new Employee("潘金莲",'女', 3500, 1000, "被打"));
        two.add(new Employee("武大郎",'女', 20000, 0, "下毒"));

        //1.开发一部的最高工资的员工
        Topperformer t = one.stream().max((o1, o2) -> Double.compare(o1.getSalary()+o1.getBonus(), o2.getSalary()+o2.getBonus()))
                .map(e -> new Topperformer(e.getName(), e.getSalary()+e.getBonus())).get();
        System.out.println(t);

        //2. 统计平均工资, 去掉最高工资和最低工资
        one.stream().sorted((e1, e2)->Double.compare(e1.getSalary()+e1.getBonus(), e2.getSalary()+e2.getBonus()))
                .skip(1).limit(one.size()-2).forEach(e -> {
                    //求出工资总和
                    allMoney += e.getSalary() + e.getBonus();
                });
        System.out.println("开发一部的平均工资是: " + allMoney / (one.size() - 2));

        //3. 合并两个集合流在统计
        Stream<Employee> s3 = Stream.concat(one.stream(), two.stream());
        s3.sorted(((o1, o2) -> Double.compare(o1.getSalary()+ o1.getBonus(), o2.getSalary()+o2.getBonus())))
                .skip(1).limit(one.size() + two.size() - 2).forEach(e->{
                    allMoney2 += e.getSalary() + e.getBonus();
                });
        //BigDecimal
        BigDecimal a = BigDecimal.valueOf(allMoney2);
        BigDecimal b = BigDecimal.valueOf(one.size() + two.size() - 2);
        System.out.println("两部门的平均工资: " + a.divide(b, 2, RoundingMode.HALF_UP));
    }
}
```

```java
package d3_stream_demo;

public class Employee {
    private String name;
    private char sex;
    private double salary;
    private double bonus;
    private String punish;

    public Employee() {
    }

    public Employee(String name, char sex, double salary, double bonus, String punish) {
        this.name = name;
        this.sex = sex;
        this.salary = salary;
        this.bonus = bonus;
        this.punish = punish;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    public double getBonus() {
        return bonus;
    }

    public void setBonus(double bonus) {
        this.bonus = bonus;
    }

    public String getPunish() {
        return punish;
    }

    public void setPunish(String punish) {
        this.punish = punish;
    }

    @Override
    public String toString() {
        return "Employee{" +
                "name='" + name + '\'' +
                ", sex=" + sex +
                ", salary=" + salary +
                ", bonus=" + bonus +
                ", punish='" + punish + '\'' +
                '}';
    }
}
```

```java
package d3_stream_demo;

public class Topperformer {
    private String name;
    private double money;

    public Topperformer() {
    }

    public Topperformer(String name, double money) {
        this.name = name;
        this.money = money;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getMoney() {
        return money;
    }

    public void setMoney(double money) {
        this.money = money;
    }

    @Override
    public String toString() {
        return "Topperformer{" +
                "name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
}
```





## 六. Steam流的收集操作

- 收集Stream流的含义: 就是把Stream流操作后的结果数据**转回到集合或数组中去**

- Stream流的收集方法

  ```java
  R collect(Collector collector) //开始收集Stream流, 指定收集器
  ```

- Collectors工具类提供了具体的收集方式

  ```java
  public static <T> Collector toList();
  public static <T> Collector toSet();
  public static Collector toMap(Function keyMapper, Function valueMapper)
  ```


```java
public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    Collections.addAll(list, "张三丰", "张无忌", "周芷若", "赵敏", "张强");
    
    Stream<String> s1 = list.stream().filter(s->s.startsWith("张"));
    
    //转为List
    List<String> zhangList = s1.collect(Collectors.toList());
    System.out.println(zhangList);
    
    List<String> list1 = s1.toList();//从JDK16开始 不能被修改
    
    //转为Set
    Set<String> zhangSet = s1.collect(Collectors.toSet());
    System.out.println(zhangSet);
    
    //转为数组
    Object[] arrs = s1.toArray();
    String[] arrs = s1.toArray(new IntFunction<String[]>() {
            @Override
            public String[] apply(int value) {
                return new String[value];//返回长度为value的字符串数组
            }
        });
    System.out.println(Arrays.toString(arrs));
}
```

- ==**注意: 流只能使用一次**==

