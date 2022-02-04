# Set系列集合

## 一. 概述

- Set系列集合特点
  - 无序: 存取顺序不一致
  - 不重复: 可以去除重复
  - 无索引: 没有带索引的方法, 所以不能使用普通for循环遍历, 也不能通过索引来获取元素

- 实现类特点
  - HashSet: 无需, 不重复, 无索引
  - LinkedHashSet: 有序, 不重复, 无索引
  - TreeSet: 排序, 不重复, 无索引

- Set集合的功能与Collection的API基本一致

```java
public static void main(String[] args) {
    //Set集合的特点: HashSet LinkedHashSet TreeSet
    Set<String> sets = new HashSet<>(); //经典代码 无序 不重复 无索引
    //Set<String> sets = new LinkedHashSet<>(); //有序 不重复 无索引
    sets.add("wang");
    sets.add("wang");
    sets.add("Java");
    sets.add("MySQL");
    sets.add("ming");
    System.out.println(sets);
    System.out.println(sets);
    
}
```



## 二. HashSet元素无序的底层原理: 哈希表

- HashSet底层采取哈希表存储的数据

- JDK8之前, 使用 **数组 + 链表**组成
- JDK8之后, 采用**数组 + 链表 + 红黑树**组成

**哈希值:**

- 根据对象的**地址**, 按照某种规则算出来的int类型的数值

**Object类的API:**

```java
public int hashCode(); // 返回对象的哈希值
```

**对象的哈希值特点:**

- 同一个对象多次调用hashCode()方法返回的哈希值是相同的
- 默认情况下, 不同对象的哈希值不同



## 三. HashSet1.7版本原理解析: 数组 + 链表 + (组合哈希算法)

1. Set<String> sets = new HashSet<>(); 创建一个默认长度为16, 默认加载因子为0.75的数组, 数组名table
2. 根据**元素的哈希值跟数组的长度求余**计算应存入的位置(哈希算法)
3. **判断当前位置是否为null**, 如果是**null直接存入**
4. 如果**不是null, 则调用equals方法比较**
5. 如果一样, 则不存; 如果不一样, 则存入数组
   - **JDK7新元素占老元素位置**, 指向老元素
   - **JDK8新元素挂在老元素下面**, 即老元素指向新元素

6. 当数组存满到16 * 0.75 = 12时, 就自动扩容, 每次扩容至原先的两倍



## 四. JDK1.8版本HashSet原理解析

- 底层结构: 数组 + 链表 + 红黑树
- 当挂在元素下面的数据过多时, 查询性能降低, 从JDK8开始后, **当链表长度超过8时, 自动转换为红黑树**



## 五. HashSet元素去重复的原理

- **如果希望Set集合认为两个内容一样的对象是重复的, 必须重写对象的hashCode()和equals()方法**

- 原因: 
  - 内容一样, 但地址可能不一样, 其哈希值也不一样, 则其存入的位置也不一样, 所以要重写hashCode()

- Demo:

  ![image-20220125190822683](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220125190822683.png)

```java
package d1_set;

import java.util.Objects;

public class Student {
    private String name;
    private int age;
    private char sex;

    public Student() {
    }

    public Student(String name, int age, char sex) {
        this.name = name;
        this.age = age;
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", sex=" + sex +
                '}';
    }

    /**
     * 只要两个对象内容一样, 返回结果就是true
     * @param o
     * @return
     */
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Student student = (Student) o;
        return age == student.age && sex == student.sex && Objects.equals(name, student.name);
    }

    /**
     * 内容一样,返回的哈希值就一样
     * @return
     */
    @Override
    public int hashCode() {
        return Objects.hash(name, age, sex);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public char getSex() {
        return sex;
    }

    public void setSex(char sex) {
        this.sex = sex;
    }
}

```





## 六. LinkedHashSet

- 有序, 不重复, 无索引

- **原理:**
  - 底层数据结构依然是哈希表, 只是每个元素又额外的多了一个**双链表的机制记录存储的顺序**

![image-20220125193252783](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220125193252783.png)



## 七. TreeSet

- 不重复, 无索引, 可排序
- **按照元素的大小默认升序排序**
- 底层基于**红黑树**的数据结构实现排序, 增删改查性能都较好

- TreeSet集合**一定要排序**的, 可以将**元素按照指定的规则进行排序**

**默认排序规则**

- 数值类型: Integer, Double默认升序
- 字符换类型: 按首字符编号升序排序
- 自定义类型: 无法直接排序, 需要自己指定规则

**自定义排序规则:**

- 方法一: 让自定义的类实现Comparable接口重写里面的compareTo方法来定制比较规则
- 方法二: TreeSet集合有参构造器, 可以设置Comparator接口对应的比较器对象, 来指定比较规则

```java
package d1_set;

import java.util.Comparator;
import java.util.Objects;

public class Apple implements Comparable<Apple> {
    private String name;
    private String color;
    private double prive;
    private int weight;

    public Apple() {
    }

    public Apple(String name, String color, double prive, int weight) {
        this.name = name;
        this.color = color;
        this.prive = prive;
        this.weight = weight;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Apple apple = (Apple) o;
        return Double.compare(apple.prive, prive) == 0 && weight == apple.weight && Objects.equals(name, apple.name) && Objects.equals(color, apple.color);
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, color, prive, weight);
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public double getPrive() {
        return prive;
    }

    public void setPrive(double prive) {
        this.prive = prive;
    }

    public int getWeight() {
        return weight;
    }

    public void setWeight(int weight) {
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "Apple{" +
                "name='" + name + '\'' +
                ", color='" + color + '\'' +
                ", prive=" + prive +
                ", weight=" + weight +
                '}';
    }

    /**
     * 方式一: 类自定义比较规则
     * @param o
     * @return
     */
    @Override
    public int compareTo(Apple o) {
        //按照重量升序
        //return this.weight - o.weight; //去掉重量重复的对象
        return this.weight - o.weight >= 0? 1 : -1;//保留重量重复的元素
    }
}
```

```java
package d1_set;

import java.util.Comparator;
import java.util.Set;
import java.util.TreeSet;

/**
 * TreeSet排序
 */
public class SetDemo4 {
    public static void main(String[] args) {
        Set<Integer> sets = new TreeSet<>();
        sets.add(23);
        sets.add(1);
        sets.add(123);
        System.out.println(sets);

        Set<String> sets1 = new TreeSet<>();
        sets1.add("wang");
        sets1.add("Java");
        sets1.add("java");
        System.out.println(sets1);

        System.out.println("----------------------");
        //方式二 集合自带比较器对象进行规则指定
        //优先用集合自带的比较器
        Set<Apple> apples = new TreeSet<>(new Comparator<Apple>() {
            @Override
            public int compare(Apple o1, Apple o2) {
                //return o1.getWeight() - o2.getWeight() > 0? -1 : 1;
                return Double.compare(o2.getPrive(), o1.getPrive());//降序
            }
        });
        apples.add(new Apple("红富士", "红色", 10.0, 500));
        apples.add(new Apple("青苹果", "绿色", 15.1, 300));
        apples.add(new Apple("小苹果", "青色", 29.9, 400));
        apples.add(new Apple("黄苹果", "黄色", 10.0, 500));
        System.out.println(apples);

    }
}
```

