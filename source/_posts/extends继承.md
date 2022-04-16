---
title: extends继承
date: 2018-11-23 12:22:12
categories: 
  - 学习笔记
---

- ### 子类中所有的构造方法默认都会访问父类中空参数的构造方法。

  - 为什么呢?
    - 因为子类会继承父类中的数据，可能还会使用父类的数据。所以，子类初始化之前，一定要先完成父类数据的初始化。

  - 其实：

### 	每一个构造方法的第一条语句默认都是：super() Object类最顶层的父类。

- ### 父类没有无参构造方法,子类怎么办?

  - super解决

  - this解决

  - 注意事项
    - super(…)或者this(….)必须出现在构造方法的第一条语句上

**new创建一个对象，子类总会先访问类中空的构造方法，默认第一句为super()；**

例子：

```java
class Test1_Extends{
	 public static void main(String[] args) {
		 Zi z=new Zi();
		 System.out.println(z.getName()+"..."+z.getAge());
		 System.out.println("————————————————————————————");
		 Fu f=new Zi_1();     //父类引用指向子类对象，向上转型;
		 //Zi_1 zi=(Zi_1)f;
		 System.out.println(f.getName()+"..."+f.getAge());
	 }
}
class Fu{
	private String name;
	private int age;
	public Fu(){
		System.out.println("空参构造");
	}
	public Fu(String name,int age){
		this.name=name;
		this.age=age;
		System.out.println("fu有参构造");
	}
	public void setName(String name){
		this.name=name;
	}
	public String getName(){
		return name;
	}
	public void setAge(int age){
		this.age=age;
	}
	public int getAge(){
		return age;
	}
}

class Zi extends Fu{		// ①父类无空构造方法时，super的方式解决
	public Zi(){
		super("张三",23);
		System.out.println("zi空参构造");
	}
}

class Zi_1 extends Fu{		// ②父类无空构造方法时，this的方式解决
	public Zi_1(){
		this("李四",24);
		System.out.println("Zi_1空参构造");
	}
	public Zi_1(String name,int age){
		super(name,age);
		System.out.println("Zi_1有参构造");
	}
}

输出为：

fu有参构造
zi空参构造
张三...23
————————————————————————————
fu有参构造
Zi_1有参构造
Zi_1空参构造
李四...24
```