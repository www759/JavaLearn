# Map集合体系

## 一. 概述

- Map集合是一种双列集合, 每个元素包含两个数据
- 每个元素的格式:**key = value**(键值对元素)
- Map集合也被称为"**键值对集合**"

**Map集合整体格式:**

- {key1=value1, key2 = value2, key3 = value3, ...}



##  二. Map集合体系特点

![image-20220127201055984](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220127201055984.png)

**说明:**

- 使用最多的是HashMap

- 重点掌握HashMap, LinkedHashMap, TreeMap

**体系特点:**

- **Map集合的键无序, 不重复, 无索引**

- **Map集合的值不做要求, 可以重复**

- Map结合后面重复的键的值会覆盖前面重复的键的值
- Map结合的键值对都可以为null

**实现类特点:**

- HashMap:  元素按照键是无序, 不重复, 无索引, 值不做要求

- LinkedHashMap: 元素有序, 不重复, 无索引, 值不做要求

- TreeMap: 元素按照键排序, 不重复, 无索引, 值不做要求



## 三. Map集合常用API

- Map是双列集合的祖宗接口, 它的功能是全部双列集合都可以使用的

```java
public static void main(String[] args) {
        Map<String, Integer> maps = new HashMap<>(); //经典代码!

        //1. 添加元素
        maps.put("鸿星尔克", 3);
        maps.put("枸杞", 100);
        maps.put("Java", 1);
        maps.put("Java", 100);//覆盖前面的数据
        maps.put(null, null);
        maps.put(null, 1);
        maps.put("wang", null);
        System.out.println(maps);

        //2. 清空集合
//        maps.clear();
//        System.out.println(maps);

        //3. 判断集合是否为空, 为空返回true
        System.out.println(maps.isEmpty());

        //4. 根据键获取对应值 public V get(Object key)
        Integer value = maps.get("Java");
        System.out.println(value);

        //5. 根据键删除整个元素, 返回删除的键对应的值
        System.out.println(maps.remove("Java"));
        System.out.println(maps);

        //6. 判断是否包含某个键, 包含返回true
        System.out.println(maps.containsKey("Java"));
        System.out.println(maps.containsKey("wang"));

        //7. 判断是否包含某个值
        System.out.println(maps.containsValue(100));
        System.out.println(maps.containsValue(0));

        //8. 获取全部键的集合: public Set<K> keySet()
        Set<String> sets = maps.keySet();
        System.out.println(sets);

        //9. 获取全部值的集合: Collection<V> values();
        Collection<Integer> values = maps.values();
        System.out.println(values);

        //10. 集合的大小
        System.out.println(maps.size());

        //11. 合并其他Map集合
        Map<String, Integer> map1 = new HashMap<>();
        map1.put("Java", 1);
        map1.put("Java2", 100);
        Map<String, Integer> map2 = new HashMap<>();
        map2.put("Java", 11);
        map2.put("Java3", 100);
        map1.putAll(map2);
        System.out.println(map1);
        System.out.println(map2);

    }
```





## 四. Map集合的遍历方式

**方式一:  键找值** 

- 先获取Map集合的全部键的集合Set
- 遍历键的集合Set, 然后通过键提取对应值

```java
public static void main(String[] args) {
    Map<String, Integer> maps = new HashMap<>();
    //1. 添加元素
    maps.put("鸿星尔克", 3);
    maps.put("枸杞", 100);
    maps.put("Java", 100);
    maps.put("wang", null);
    System.out.println(maps);

    //1. 键找值 第一步: 先拿到集合的全部键
    Set<String> keys = maps.keySet();
    //2. 第二部: 遍历每个键, 根据键提取值
    for (String key : keys){
        Integer value = maps.get(key);//注意如果用int会出bug
        System.out.println(key + "==>" + value);
    }
}
```

**方式二: 键值对**

- 先把Map集合转换成Set集合, Set集合中每个元素都是键值对实体类型
- 遍历Set集合, 然后提取键, 提取值

- 涉及到的api:

  ```java
  Set<Map.Entry<K, V>> entrySet();
  ```

```java
public static void main(String[] args) {
    Map<String, Integer> maps = new HashMap<>();
    maps.put("鸿星尔克", 3);
    maps.put("枸杞", 100);
    maps.put("Java", 100);
    maps.put("wang", null);
    System.out.println(maps);

    /**
     * 通过调用Map的方法, entrySet把Map集合转换为Set集合形式
     */
    Set<Map.Entry<String, Integer>> entries = maps.entrySet();
    for(Map.Entry<String, Integer> entry : entries){
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.println(key + "=" + value);
    }
```

**方式三: lambda表达式**

```java
public static void main(String[] args) {
    Map<String, Integer> maps = new HashMap<>();
    maps.put("鸿星尔克", 3);
    maps.put("枸杞", 100);
    maps.put("Java", 100);
    maps.put("wang", null);
    System.out.println(maps);

    maps.forEach(new BiConsumer<String, Integer>() {
        @Override
        public void accept(String key, Integer value) {
            System.out.println(key + "==>" + value);
        }
    });

    maps.forEach((key, value) -> System.out.println(key + "-" + value));
}
```

## 五. 案例- 统计投票人数

```java
public static void main(String[] args) {
    //1. 把80个学生选择的数据拿进来
    String[] selects = {"A", "B", "C", "D"};
    StringBuilder sb = new StringBuilder();

    Random r = new Random();
    for(int i = 0; i < 80; ++i){
        sb.append(selects[r.nextInt(selects.length)]);
    }
    System.out.println(sb);

    //2. 定义一个Map集合记录统计的结果
    Map<Character, Integer> infos = new HashMap<>();

    //3. 遍历80个学生选择的数据
    for (int i = 0; i < sb.length(); i++) {
        //4. 提取当前字符
        char ch = sb.charAt(i);
        //5. 判断Map集合中是否存在键ch
        if(infos.containsKey(ch)){
            //让其值加1
            infos.put(ch, infos.get(ch)+1);
        }else{
            // 此键第一次被选则
            infos.put(ch, 1);
        }
    }

    //4. 输出Map集合
    System.out.println(infos);
}
```

![image-20220128211551161](C:\Users\26297\AppData\Roaming\Typora\typora-user-images\image-20220128211551161.png)



## 六. Map集合的实现类

### 一. HashMap

- HashMap特点: 无需, 不重复, 无索引
- HashMap跟HashSet的底层原理一样, 都是哈希表结构, 只是HashMap的每个元素包含两个值

- **实际上, Set系列集合的底层就是Map实现的, 只是Set集合中的元素只要键数据, 不要值数据**

```java
public HashSet(){
	map = new HashMap<>();
}
```

- 依赖hashCode和equals方法保证键的唯一

- 如果键要存储的是自定义对象, 需要重写hashCode和equals方法

- 基于哈希表, 增删改查的性能都很好

### 二. LinkedHashMap

- 由键决定: **有序**, 不重复, 无索引
- 原理: 底层数据结构依然是哈希表, 只是每个键值对又额外多了一个双链表的机制记录存储的顺序

### 三. TreeMap

- 由键决定特性: **不重复**, 无索引, **可排序**

- **只能对键排序**
- 注意: TreeMap一定要排序, 可以默认排序, 也可以将键按照指定的规则进行排序
- TreeMap和TreeSet底层原理是一样的

