# hello world案例

编译后形成javac字节码文件

public class HelloWorld{
	public static void main(String[] args){
		System.out.println("Hello World!");
	}
}

javac HelloWorld.java

java HelloWorld

注意: 文件名需要与类名相同

# 类名与文件名的关系

​	java中可以有多个类，但是**最多只有一个类的类名和文件名相同**;

​	**如果一个类被public修饰，那该类的类名必须和文件名相同，并且一个java文件中最多只有一个类被public修饰**
​	**主方法所在类的类名一定要与文件名一致**

# 类

java中最基本的组成单位是类

类的定义格式: **public class  类名**

