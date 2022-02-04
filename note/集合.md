# 集合概述及Collection体系

## 一. 集合概述

- **存储对象数据的一种容器**

- **特点:**
  - 大小不固定, 类型可以选择不固定
  - 适合元素的增删操作
  - 只能存储引用数据类型, 如果存储基本数据类型可以选用包装类

- **使用的场景:** 

  - 数据的个数不确定, 需要进行增删元素的时候

  

## 二. 集合的体系特点

- Collection单列集合, 每个元素(数据)只包含一个值
- Map双列集合, 每个元素包含两个值(键值对)





## 三. Collection体系

![image-20220123220825869](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220123220825869.png)

- Collection集合特点

  - List系列集合: 添加的元素是有序, 可重复, 有索引(有序是指按照添加的顺序组织顺序)
    - ArrayList, LinkedList: 有序, 可重复, 有索引

  - Set系列集合: 添加的元素是无序, 不重复, 无索引

    - HashSet: 无需, 不重复, 无索引 LinkedHashSet: 有序, 不重复, 无索引

    - TreeSet: 按照大小默认升序排序, 不重复, 无索引

```java
public static void main(String[] args) {
    //有序 可重复 有索引
    Collection list = new ArrayList();
    list.add("Java");
    list.add("Java");
    list.add(23);
    list.add(false);
    System.out.println(list);

    //无需 不重复 无索引
    Collection list1 = new HashSet();
    list1.add("Java");
    list1.add("Java");
    list1.add(23);
    list1.add(false);
    System.out.println(list1);
}
```



## 四. 集合对于泛型的支持

- 集合都是支持泛型的, 可以在编译阶段约束集合只能操作某种数据类型

```java
Collection<String> lists = new ArrayList<>();
```



## 五. Collection的常用API

- Collection是单列集合的祖宗接口, 它的功能是全部单列集合都可以继承使用的

```java
public static void main(String[] args) {
        Collection<String> list = new ArrayList<>();

        //1. 添加元素, 添加成功返回true
        list.add("wang");
        list.add("Java");
        list.add("Java");
        System.out.println(list.add("MySQL"));//true
        System.out.println(list);//[wang, Java, Java, MySQL]


        //2. 清空集合元素
//        list.clear();
//        System.out.println(list);//[]

        //3. 判断集合是否为空, 时空返回true
        System.out.println(list.isEmpty());//false

        //4. 获取集合大小
        System.out.println(list.size());//4

        //5.判断集合中是否包含某个元素
        System.out.println(list.contains("Java"));//true
        System.out.println(list.contains("java"));//false

        //6. 删除某个元素: 如果有多个重复元素默认删除第一个
        System.out.println(list.remove("java"));//false
        System.out.println(list.remove("Java"));//true
        System.out.println(list);//[wang, Java, MySQL]

        //7. 把集合转换成数组
        Object[] arrs = list.toArray();
        System.out.println(Arrays.toString(arrs));//[wang, Java, MySQL]


        System.out.println("-------------拓展--------------");
        Collection<String> list2 = new ArrayList<>();
        list2.add("fei");
        list2.add("jing");

        list.addAll(list2);
        System.out.println(list);//[wang, Java, MySQL, fei, jing]
        System.out.println(list2);//[fei, jing]

    }
```



## 六. Collection集合的遍历方式

- 迭代器在Java中的代表是Iterator, 是集合的专用遍历方式

- 遍历方式:
  - 迭代器while循环遍历
  - foreach/增强for循环(可以遍历数组或集合)
  - Lambda表达式

- 增强for循环格式:

  ```java
  for(元素数据类型 变量名:数组或Collection集合){
  	//在此处使用变量即可, 该变量就是元素
  }
  ```

  

- Lambda表达式遍历集合:

  Collection结合Lambda遍历的API:

  ```java
  default void forEach(Consumer<? super T> action);
  ```

  

```java
public static void main(String[] args) {
    Collection<String> list = new ArrayList<>();
    list.add("wang");
    list.add("Java");
    list.add("Java");
    list.add("MySQL");
    System.out.println(list);

    // 得到当前集合的迭代器对象
    Iterator<String> it = list.iterator();

    //1. while循环遍历
    while(it.hasNext()){
        System.out.println(it.next());
    }

    //2.foreach/增强for循环
    for(String ele : list){
        System.out.println(ele);
    }

    System.out.println("----------------");
    double[] scores = {100, 100.2, 123};

    for (double score : scores) {
        System.out.println(score);
    }

    System.out.println("-----------------");
    //3. Lambda表达式
    list.forEach(new Consumer<String>() {
        @Override
        public void accept(String s) {
            System.out.println(s);
        }
    });

    list.forEach(s -> System.out.println(s));

    list.forEach(System.out::println);
}
```

![image-20220124125643235](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220124125643235.png)

- **注意: 集合中存储的元素对象的地址**

