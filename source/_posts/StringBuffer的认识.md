---
title: StringBuffer的认识
date: 2018-11-23 12:21:12
categories: 
  - 学习笔记
---

### StringBuffer的构造方法：

   * public StringBuffer():无参构造方法
     * public StringBuffer(int capacity):指定容量的字符串缓冲区对象
     * public StringBuffer(String str):指定字符串内容的字符串缓冲区对象
   * B:StringBuffer的方法：
     * public int capacity()：返回当前容量。	理论值(不掌握)
     * public int length():返回长度（字符数）。 实际值

### StringBuffer的添加功能
- public StringBuffer append(String str):
  - 可以把任意类型数据添加到字符串缓冲区里面,并返回字符串缓冲区本身

- public StringBuffer insert(int offset,String str):
  - 在指定位置把任意类型的数据插入到字符串缓冲区里面,并返回字符串缓冲区本身
    StringBuffer是字符串缓冲区,当new的时候是在堆内存创建了一个对象,底层是一个长度为16的字符数组
    当调用添加的方法时,不会再重新创建对象,在不断向原缓冲区添加字符




### StringBuffer的删除功能
* public StringBuffer deleteCharAt(int index):
  删除指定位置的字符，并返回本身

* public StringBuffer delete(int start,int end):

* 删除从指定位置开始指定位置结束的内容，并返回本身

  ```java
  StringBuffer sb = new StringBuffer();
  sb.append("heima");
  sb.delete(0, sb.length());			//清空缓冲区
  System.out.println(sb);
  //sb = new StringBuffer();	//不要用这种方式清空缓冲区,原来的会变成垃圾,浪费内存
  //System.out.println(sb);
  ```

  

### StringBuffer的替换功能
- public StringBuffer replace(int start,int end,String str):	#从start开始到end用str替换

### StringBuffer的反转功能
- public StringBuffer reverse():	#字符串反转

### StringBuffer的截取功能
* public String substring(int start):	#从指定位置截取到末尾
* public String substring(int start,int end):	#截取从指定位置开始到结束位置，包括开始位置，不包括结束位置
* 注意事项
  * 返回值类型不再是StringBuffer本身


### String -- StringBuffer转换

- 通过构造方法 
  
  ```java
  StringBuffer sb = new StringBuffer("abc");
  sout(sb);
  ```
  
- 通过append()方法
  
  ```java
  StringBuffer sb1 = new StringBuffer();
  sb1.append("abc");
  sout(sb1);
  ```
  
  

### StringBuffer -- String转换

- 通过构造方法

  ```java
  StringBuffer sb1 = new StringBuffer("abc");
  Stirng s = new String(sb1)
  sout(s);
  ```

  

- 通过toString()方法

  ```java
  String s1= sb1.toString();
  sout(s1);
  ```

  

- 通过subString(0,length);

  ```java
  String s2 =sb1.substring(0,sb.length());
  sout(s2);
  ```

  