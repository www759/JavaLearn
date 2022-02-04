# Object类

- 一个类要么默认继承了Objectlei, 要么间接继承了
- Object类的方法是一切子类都可以使用的

## 常用方法

### 1. toString

- 默认返回当前对象在堆内存中的地址信息: 类的全限名@内存地址

**问题引出:**

- 开发中输出对象地址很多时候无意义, 更多的是希望看到所指的内容信息

**解决:**

- 父类toString()方法存在的意义就是**为了被子类重写, 以便返回对象的内容信息**

```java
package d6_api_object;

/**
 * Object类中toString方法的使用
 */
public class Test {
    public static void main(String[] args) {
        Student student = new Student("wang", '男', 19);

        System.out.println(student.toString());
        //默认省略toString()不写
        System.out.println(student);

    }
}
```

```java
@Override
public String toString() {
    return "Student{" +
            "name='" + name + '\'' +
            ", sex=" + sex +
            ", age=" + age +
            '}';
}
```

### 2. equals方法

- 默认比较两个对象的地址是否相同

```java
public Boolean equals(Object o){}
```

**问题思考:**

- 直接比较两个对象的地址可以使用"=="替代equals

**解决:**

- 重写equals, 比较两个对象的内容, 可以自己制定规则

```java
@Override
public boolean equals(Object o){
    //1. 判断o是不是Student类型
    if(o instanceof Student){
        Student student = (Student)o;
        //2. 判断两个对象的内容是否相同
        return (this.name.equals(student.name)
                && this.age == student.age
                && this.sex == student.sex);
    }else return false;
}
```

```java
@Override
    public boolean equals(Object o) {
        //1. 判断是否为同一个对象
        if (this == o) return true;
        //2. 判断o是否为null 并 判断类型是否相同
        if (o == null || getClass() != o.getClass()) return false;
        //3. 判断内容
        //4. Object.equals更安全, 会判断字符串为null的情况
        Student student = (Student) o;
        return sex == student.sex && age == student.age && Objects.equals(name, student.name);
    }
```