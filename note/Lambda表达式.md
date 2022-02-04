# Lambda表达式

- **作用:** 简化匿名内部类的代码写法

  **注意: 只能简化函数式接口的匿名内部类的写法形式**

- **简化格式:**

  ```java
  (匿名内部类被重写方法的形参列表)->{
  	被重写方法的方法体代码
  }
  ```

  

- **什么是函数式接口:**
  - 首先必须是接口、其次接口中有且仅有一个抽象方法的形式
  - 通常会加上一个@FunctionalInterface注解, 标记该接口必须是满足函数式接口

- Demo:

  ```java
  package d8_Lambda;
  
  public class LambdaDemo2 {
      public static void main(String[] args) {
          //Lambda只能简化接口中只有一个抽象方法的匿名内部类形式
          Swimming s1 = new Swimming() {
              @Override
              public void swim() {
                  System.out.println("🐢 is swimming");
              }
          };
          go(s1);
  
          System.out.println("----------------------");
          Swimming s2 = () -> {
              System.out.println("🐅 is swimming");
          };
          go(s2);
  
          System.out.println("----------------------");
          go(()->{
              System.out.println("🐥 is swimming");
          });
      }
  
      public static void go(Swimming s){
          System.out.println("start");
          s.swim();
          System.out.println("finish");
      }
  }
  
  @FunctionalInterface //一旦加上这个注解必须是函数式接口, 里面只能有一个抽象方法
  interface Swimming{
      void swim();
  }
  ```

- **数组排序 比较器对象**

  ```java
  public static void main(String[] args) {
      Integer[] ages = {43, 12, 123, 234};
  
      Arrays.sort(ages, (Integer o1, Integer o2)->{
         return o2 - o1;
      });
      System.out.println(Arrays.toString(ages));
  
      
  }
  ```



- Lambda表达式的省略写法
  - 参数类型可以省略不写
  - 如果只有一个参数, 参数类型可以省略, 同时()也可以省略
  - 如果Lambda表达式的方法体代码只有一行代码. 也可以省略大括号不写, 同时要省略分号
  - 如果Lambda表达式的方法体代码只有一行代码, 可以省略大括号不写, 此时, 如果这行式return语句, **必须**省略return不写, 同时省略分号不写

```java
Arrays.sort(ages, (o1, o2)->{
   return o2 - o1;
});
```

```java
Arrays.sort(ages, (o1, o2)-> o2 - o1);
```

