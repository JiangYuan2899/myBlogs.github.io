---
layout: post
title: Static相关问题
category: other
tags: [other]
---
# Static 关键字解读
 
 看下面的代码看看会输出什么，为什么是这个结果？

 ```
public class Test {
	static int a;
	static Integer b;
	int c;
	public static void main(String[] args) {
		System.out.println(a);
		System.out.println(b);
		System.out.println(c);
	}
}
 ```

 答案分别是：0;null;会报错

 答案解读：
 
 首先说第三个基本数据类型初始化必须赋值。不赋值就报错。
 
 第二个是一个包装类，类的初始值都是null;
 
 然后重点是第一个，java中普通声明的变量在未初始化赋值的情况下无法直接使用，但是类中以static修饰的变量（即静态变量）会在编译期间由系统自动初始化并赋予默认值，所以在我们没有实例化的情况也可以使用。这是因为java中任何变量会在程序运行中通过实例化由系统动态的自动分配内存空间，而静态是在编译后便已分配了内存，并且随着类的存亡而存亡，只要类没有结束退出，静态变量所占有的内存就会一直存在。这么做的目的就是使得静态方法或者静态变量在没有实例化的情况下也可以被直接使用。

 ```
//我们先依次定义八个基本数据类型的变量，再直接输出查看结果
	public static byte bt;
	public static int num;
	public static float fl;
	public static long l;
	public static short s;
	public static double d;
	public static char c;
	public static boolean flag;
 ```

 输出结果分别是：
bt = 0

num  = 0

fl = 0.0

l = 0

s = 0

d = 0.0

c = 

flag = false

其中char类型的c输出的是空，对应ASCII码中的0

接下来在做测试String类型和StringBuffer类型的数据默认赋值是null；