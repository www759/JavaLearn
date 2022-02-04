# Arrays类

- 数组操作工具类, 专门用于操作数组元素的
- 主要操作: 

```java
public static void main(String[] args) {
    //目标: 学会使用Arrays类的常用API, 并理解其原理
    int[] arr = {10, 2, 33, 123, 123};
    System.out.println(arr);//[I@7c30a502

    //1. 返回数组内容的String
    System.out.println(Arrays.toString(arr));//[10, 2, 33, 123, 123]

    //2. 排序(默认升序)
    Arrays.sort(arr);
    System.out.println(Arrays.toString(arr));//[2, 10, 33, 123, 123]

    //3. 二分搜索(前提是必须排好序)
    int index = Arrays.binarySearch(arr, 55);
    System.out.println(index);//-(insert position)-1

    index = Arrays.binarySearch(arr, 10);
    System.out.println(index);//index

    
}
```





- 排序

  ```java
  public static void main(String[] args) {
      //自定义数组的排序规则: Comparator比较器对象
      //1. Arrays的sort方法对于有值特性的数组是默认升序排序
      int[] ages = {123, 12, 123, 143, 11};
      Arrays.sort(ages);
      System.out.println(Arrays.toString(ages));
  
      //2. 降序排序 (自定义比较器对象, 只能支持引用类型的排序)
      /**
          参数一: 被排序的数组, 必须是引用类型
          参数二: 匿名内部类对象, 代表了一个比较器对象
       */
      Integer[] ages1 = {123, 12, 123, 143, 11};
      Arrays.sort(ages1, new Comparator<Integer>(){
          @Override
          public int compare(Integer o1, Integer o2){
              //return o1 - o2;//默认升序
              return o2 - o1;//降序
          }
      });
      System.out.println(Arrays.toString(ages1));
  
      System.out.println("-------------------");
      Student[] students = new Student[3];
      students[0] = new Student("wang", 23, 175.5);
      students[1] = new Student("feng", 18, 178);
      students[2] = new Student("chu", 22, 180.5);
  
      System.out.println(Arrays.toString(students));
  
      Arrays.sort(students, new Comparator<Student>() {
          @Override
          public int compare(Student o1, Student o2) {
              //return o1.getAge() - o2.getAge();//按照年龄升序排序
              return Double.compare(o2.getHeight(), o1.getHeight());//比较浮点型必须这样调用
          }
      });
      System.out.println(Arrays.toString(students));
  }
  ```

  ```java
  package d7_Arrays;
  
  public class Student {
      private String name;
      private int age;
      private double height;
  
      public Student() {
      }
  
      public Student(String name, int age, double height) {
          this.name = name;
          this.age = age;
          this.height = height;
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
  
      public double getHeight() {
          return height;
      }
  
      public void setHeight(double height) {
          this.height = height;
      }
  
      @Override
      public String toString() {
          return "Student{" +
                  "name='" + name + '\'' +
                  ", age=" + age +
                  ", height=" + height +
                  '}';
      }
  }
  ```
