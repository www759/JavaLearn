# List系列集合

## 一. 特点

- 有序: 存储和取出的元素顺序一致
- 有索引: 可以通过索引操作元素
- 可重复



## 二. List集合特有方法

- List因为支持索引, 所以多了很多索引操作的独特api, 其他Collection的功能List也继承了

```java
public static void main(String[] args) {
    //1. 创建一个ArrayList集合对象
    List<String> list = new ArrayList<>();// 经典代码!
    list.add("Java");
    list.add("MySQL");
    list.add("wang");

    //2. 在某个索引位置插入元素
    list.add(2, "chu");
    System.out.println(list);

    //3. 根据索引删除元素, 返回被删除元素
    System.out.println(list.remove(2));
    System.out.println(list);

    //4. 根据索引获取元素, 返回集合中被指定位置的元素
    System.out.println(list.get(2));

    //5. 修改索引位置处的元素, 返回修改前的数据
    System.out.println(list.set(2, "fei"));

}
```



## 三. List的实现类的底层原理

### ArrayList

- ArrayList底层**基于数组**实现, 查询元素快, 增删相对慢
- 第一次创建集合并添加第一个元素时, 在底层创建一个默认长度为10的数组
- 容量不足时扩容至原来的1.5倍

### LinkedList

- LinkedList底层**基于双链表**实现的, 查询元素慢, 增删首尾元素很快

- 特有功能:

  ```java
  public static void main(String[] args) {
      //LinkedList可以完成队列结构 和 栈结构
      //栈
      LinkedList<String> stack = new LinkedList<>();
      //压栈, 入栈
      stack.push("1");
      stack.push("2");
      stack.addFirst("3");
      stack.addFirst("4");
      System.out.println(stack);
      //出栈, 弹栈
      System.out.println(stack.pop());
      System.out.println(stack.pop());
      System.out.println(stack.removeFirst());
      System.out.println(stack);
  
      System.out.println("------------------------");
      //队列
      LinkedList<String> queue = new LinkedList<>();
      //入队
      queue.offerLast("1");
      queue.addLast("2");
      queue.addLast("3");
      queue.addLast("4");
      System.out.println(queue);
      //出队
      System.out.println(queue.removeFirst());
      System.out.println(queue.removeFirst());
      System.out.println(queue.removeFirst());
      System.out.println(queue);
  }
  ```



## 四. List集合的遍历方式

- 迭代器
- 增强for
- Lambda表达式
- for循环(基于索引)



## 五. 集合的并发修改异常问题

- 当我们从集合中找出某个元素并删除是可能出现一种并发修改异常

- 哪些遍历存在问题?
  - **迭代器遍历集合且直接用集合删除元素**时
  - **增强for循环遍历集合并且直接使用集合删除元素**时
  - for循环不会报异常, 但会漏删

```java
public static void main(String[] args) {
        List<String> list = new ArrayList<>();// 经典代码!
        list.add("Java");
        list.add("MySQL");
        list.add("Java");
        list.add("Java");
        list.add("Java");
        list.add("wang");
        System.out.println(list);

        //需求: 删除全部的Java信息
        //a. 迭代器遍历删除
//        Iterator<String> it = list.iterator();
//        while(it.hasNext()){
//            String ele = it.next();
//            if("Java".equals(ele)){
//                //list.remove("Java");
//                //迭代器往前移动一个位置
//                it.remove();//删除当前所在元素, 保证不后移, 能够成功遍历到所有元素
//            }
//        }
//        System.out.println(list);

        //b. foreach遍历删除, 无法解决
        /*for(String s : list){
            if("Java".equals(s)){
                list.remove("Java");
            }
        }*/

        //c. lambda表达式, 与增强for一样
        /*list.forEach(s -> {
            if("Java".equals(s)){
                list.remove("Java");
            }
        });*/

        //d. for循环, 无异常, 但数据删除出现了问题, 会漏掉元素
        //可以倒着删除
        for(int i = list.size() - 1; i >= 0; --i){
            String ele = list.get(i);
            if("Java".equals(ele)){
                list.remove("Java");
            }
        }
        System.out.println(list);

    }
```

- 不出问题的方式:
  - 迭代器遍历结合但用迭代器自己的删除方法操作
  - 使用for循环遍历并删除元素(倒着删或者每次删完后i--)