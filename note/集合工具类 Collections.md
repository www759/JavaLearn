# 集合工具类: Collections

- java.utils.Collections: 是集合工具类
- 作用: Collections并不属于集合, 是用来操作集合的工具类

**常用API**

```java
public static <T> boolean addAll(Collection<? super T> c, T...elements) //给集合对象批量添加元素
```

```java
public static void shuffle(List<?> list)
```

**排序相关API:**

- 适用范围: 只能对List集合排序

- 排序方式一:

  ```java
  public static <T> void sort(List<T> list) //将集合中元素按照默认规则排序
  ```

- 排序方式二:

  ```java
  public static <T> void sort(List<T> list, Comparator<? super T> c) //将集合元素按照指定规则排序
  ```

```java
package d3_Collections;

import d1_set.Apple;

import java.util.*;

public class CollectionsDemo1 {
    public static void main(String[] args) {
        List<String> names = new ArrayList<>();
        //批量添加数据
        Collections.addAll(names, "楚留香", "张无忌", "陆小凤");
        System.out.println(names);

        //2. 打乱List集合顺序
        Collections.shuffle(names);
        System.out.println(names);

        //3. 将List集合元素按默认规则排序(值特性的元素)
        List<Integer> list = new ArrayList<>();
        Collections.addAll(list, 20, 30, 4, 5, 5);
        Collections.sort(list);

        //4. 自定义数据类型排序
        //方式一
        List<d1_set.Apple> apples = new ArrayList<>();
        apples.add(new Apple("红富士", "红色", 10.0, 500));
        apples.add(new Apple("青苹果", "绿色", 15.1, 300));
        apples.add(new Apple("小苹果", "青色", 29.9, 400));
        apples.add(new Apple("黄苹果", "黄色", 10.0, 500));

        Collections.sort(apples);
        System.out.println(apples);

        //方式二
        Collections.sort(apples, new Comparator<Apple>() {
            @Override
            public int compare(Apple o1, Apple o2) {
                return Double.compare(o1.getPrive(), o2.getPrive());
            }
        });

        Collections.sort(apples, (o1, o2)->Double.compare(o1.getPrive(), o2.getPrive()));
        System.out.println(apples);
    }
}
```

```java
package d3_Collections;

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
        return this.weight - o.weight; //List集合不会去重, Set集合会去重
        //return this.weight - o.weight >= 0? 1 : -1;
    }
}
```
