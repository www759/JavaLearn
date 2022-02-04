# ArrayList

集合类似与数组, 也是一种容器, 用于装数据

- 数组的特点: 类型确定, 长度固定
- 集合的大小不固定, 启动后可以动态变化, 类型也可以选择不固定

**集合非常适合做元素个数不确定, 且要进行增删的业务场景**

**集合提供了许多丰富好用的功能, 而数组的功能很单一**

**集合中存储的是对象的地址, 而不是对象本身**

## 一. ArrayList概述

- ArrayList是集合的一种, 支持索引
- 对象获取: public ArrayList()

- 添加元素:

  ```java
  public static void main(String[] args) {
      //创建ArrayList的对象
      ArrayList list = new ArrayList();
  
      //添加数据
      list.add("Java");
      list.add(100);
      list.add(10.1);
      list.add(false);
      list.add('中');
  
      System.out.println(list);
      
      //给指定索引位置插入元素
      list.add(1, "guang");
      System.out.println(list);
  }
  ```

##  二. 泛型

- 不推荐ArrayList存储任意类型的元素

- ArrayList<E>: 其实就是一个泛型类, 可以在编译阶段约束集合对象只能操作某种数据类型

- **集合中只能存储引用类型, 不支持基本数据类型**

  ArrayList<Integer> 存储整数类型

```java
public static void main(String[] args) {
    //后面的String可以不写, JDK1.7开始后面可以不写
    ArrayList<String> list = new ArrayList<>();
    list.add("add");

    ArrayList<Integer> list2 = new ArrayList<>();
    list2.add(100);
    
}
```

## 三. ArrayList常用API

![image-20220104135407448](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220104135407448.png)

```java
public static void main(String[] args) {
    //后面的String可以不写, JDK1.7开始后面可以不写
    ArrayList<String> list = new ArrayList<>();
    list.add("add");
    list.add("java");
    list.add(1, "MySql");
    list.add("MySql");

    //1. 获取某个索引位置处的元素值
    String e = list.get(1);
    System.out.println(e);

    //2. 获取集合大小
    int size = list.size();
    System.out.println(size);

    //3. 集合的元素遍历
    for (int i = 0; i < list.size(); i++) {
        System.out.println(list.get(i));
    }

    //4. 删除索引位置出的元素值, 并返回被删除的元素值
    System.out.println(list);
    String s = list.remove(2);
    System.out.println(list);
    System.out.println(s);

    //5. 直接删除元素值, 只会删除最前面的一个, 删除成功返回true
    list.remove("Mysql");
    System.out.println(list);

    //6. 修改索引处的元素值, 返回修改前的值
    String s1 = list.set(0, "woaini");
    System.out.println(list);
    System.out.println(s1);
}
```

## 四. 遍历删除元素

**注意删除时后面的元素会往前移动**

```java
public static void main(String[] args) {
        ArrayList<Integer> scores = new ArrayList<>();
        scores.add(100);
        scores.add(60);
        scores.add(89);
        scores.add(10);
        scores.add(70);
        scores.add(99);
        scores.add(86);


//        错误做法, 因为删除后后方所有元素的索引就变了
//        for(int i = 0; i < scores.size(); ++i){
//            int score = scores.get(i);
//            if(score < 80){
//                scores.remove(i);
//            }
//        }
//
//        System.out.println(scores);

//        for(int i = 0; i < scores.size(); ++i){
//            int score = scores.get(i);
//            if(score < 80){
//                scores.remove(i);
//                --i;
//            }
//        }
//        System.out.println(scores);

        //从后面开始删除
        for(int i = scores.size() - 1; i >= 0; --i){
            int score = scores.get(i);
            if(score < 80){
                scores.remove(i);
            }
        }
        System.out.println(scores);
    }
```

## 五. 集合存储自定义类型对象

```java
package ArrayListDemo;

import java.util.ArrayList;

public class Demo {
    public static void main(String[] args) {
        ArrayList<Movie> movies = new ArrayList<>();

        movies.add(new Movie("肖申克的救赎", 9.9, "罗宾斯"));
        movies.add(new Movie("霸王别姬", 9.8, "张国荣"));
        System.out.println(movies);

        for(int i = 0; i < movies.size(); ++i){
            Movie movie = movies.get(i);
            System.out.println("片名" + movie.getName());
            System.out.println("评分" + movie.getScore());
            System.out.println("主演" + movie.getActor());
        }
    }
}
```

```java
package ArrayListDemo;

public class Movie {
    String name;
    double score;
    String actor;

    public Movie() {
    }

    public Movie(String name, double score, String actor) {
        this.name = name;
        this.score = score;
        this.actor = actor;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public double getScore() {
        return score;
    }

    public void setScore(double score) {
        this.score = score;
    }

    public String getActor() {
        return actor;
    }

    public void setActor(String actor) {
        this.actor = actor;
    }
}
```

## 六. 内存模型

![image-20220104151620432](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220104151620432.png)

