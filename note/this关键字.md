# this关键字

**作用: 出现在成员方法, 构造器中代表当前对象的地址, 用于访问当前对象的成员变量, 成员方法**

**自我理解: 区分形参与成员变量**

## 一. this出现在有参构造器中

```java
public class Car{
	String name;
    double price;
    
    //见名知意
    public Car(String name, double price){
        this.name = name;
        this.price = price;
    }
}
```

### 错误demo

```java
package demo;

public class Car {
    String name;
    double price;
    public Car(){
        System.out.println("无参构造器");
    }

    public Car(String name, double price){
        System.out.println("有参构造器");
        name = name;
        price = price;
    }
}
```

**此时调用有参构造器, 并不会对成员变量进行赋值**

## 二. this出现在成员方法中

```java
public class Car{
    String name;
    double price;
    
    public void goWith(String name){
        System.out.println(this.name + "正在和" + name + "一起比赛!");
    }
}
```

**区分形参和成员变量**

### 三. this存储的成员变量的地址

```java
public class Car {
    String name;
    double price;
    public Car(){
        System.out.println("无参构造器");
        System.out.println(this);//demo.Car@7c30a502
    }
}

package demo;

public class Test {
    public static void main(String[] args) {
        Car c = new Car();
        System.out.println(c);//demo.Car@7c30a502
    }
}
```

**this与成员变量的值相同, 存储了同样的地址**
