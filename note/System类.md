# System类

## 概述

- System类的功能是通用的, 都是直接用类名调用即可, System不能实例化

- demo:

  ```java
  package d10_api_System;
  
  import java.util.Arrays;
  
  public class Demo {
      public static void main(String[] args) {
          System.out.println("program is start running");
  
          //System.exit(0); //JVM终止
  
          //1. 计算机认为时间有起源, 返回1970-1-1 00:00:00 走到此时的总的毫秒值
          //   时间毫秒值
          long time = System.currentTimeMillis();
          System.out.println(time);
  
          //2. 进行时间的计算, 性能分析
          long currentTime = System.currentTimeMillis();
          for(int i = 0; i < 100000; ++i){
              System.out.println(i);
          }
          long endTime = System.currentTimeMillis();
          System.out.println((endTime - currentTime) / 1000.0 + "s");
  
  
          //3. 数组拷贝
          /*arraycopy(Object src,  int  srcPos,
          Object dest, int destPos,
          int length);*/
          int[] arr1 = {10, 20, 30, 40, 50, 60, 70};
          int[] arr2 = new int[6]; //==>[0, 0, 40, 50, 60, 0]
          System.arraycopy(arr1, 3, arr2, 2, 3);
          System.out.println(Arrays.toString(arr2));
  
  
          System.out.println("program is finish");
      }
  }
  
  ```

  

