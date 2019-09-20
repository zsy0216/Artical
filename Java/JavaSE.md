[TOC]

# 常用类

## 包装类

### 基本介绍

包装类位于`java.lang`包，用法类似；

基本数据类型与包装类的对应关系：

| 基本数据类型 |  包装类   |
| :----------: | :-------: |
|     byte     |   Byte    |
|   boolean    |  Boolean  |
|    short     |   Short   |
|     char     | Character |
|     int      |  Integer  |
|     long     |   Long    |
|    float     |   Float   |
|    double    |  Double   |

由于包装类的常用方法类似，此处拿Integer举例；

数字相关的包装类都继承了Number类；Byte,Short,Integer,Long,Float,Double等。

常用方法示例：

```java
public class TestInteger {
	public static void main(String[] args) {
		//1.基本数据类型转成包装类对象
		Integer a = new Integer(1);
		Integer b = Integer.valueOf(2);
		//2.包装类对象转成基本数据类型
		int c = b.intValue();
		double d = b.doubleValue();		
		//3.字符串转成包装类对象
		Integer e = new Integer("8888");	
		//4.包装类对象转成字符串
		String f = e.toString();	
		//5.包装类中的常量
		System.out.println("Integer中最大值："+Integer.MAX_VALUE);
		System.out.println("Size："+Integer.SIZE);
	}
}
```

### 自动装箱和拆箱

```java
// 自动装箱 
Integer a = 2;//相当于 Integer a = Integer.valueOf(2); 
// 自动拆箱
int b = a; //编译器自动转为：int b = a.intValue();
```

- 缓存问题：缓存[-128,127]之间的数字,系统初始化时创建的一个缓存数组；

```java
Integer a = 124;
Integer b = 124;
System.out.println(a==b); //true
System.out.println(a.equals(b)); //true

Integer c = 128;
Integer d = 128;
System.out.println(c==d); //false
System.out.println(c.equals(d)); //true
```

## String

```java
public final class String
    implements java.io.Serializable, Comparable<String>, CharSequence {
    /** The value is used for character storage. */
    private final char value[];   
}
```

- String类是不可变字符序列；
- 判断String对象值是否相等使用equals()方法

### 常用方法

[JDK8 API](https://docs.oracle.com/javase/8/docs/api/index.html)

| 方法                                           | 解释说明                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| char charAt(int index)                         | 返回字符串中索引为index的字符，索引范围(0,lenth()-1);        |
| boolean equals(Object anObject)                | 如果字符串与anObject相等，返回true；否则返回false；          |
| boolean equalsIgnoreCase(String other)         | 判断字符串是否相等（忽略大小写）；                           |
| int indexOf(String str)                        | 返回从头开始查找的第一个子字符串str在字符串中的索引位置。如果未找到子字符串str，则返回-1； |
| int lastIndexOf()                              | 返回从末尾开始查找的第一个子字符串str在字符串中的索引位置。如果未找到子字符串str，则返回-1； |
| int length()                                   | 返回字符串的长度；                                           |
| String replace(char oldChar,char new Char)     | 返回一个新串，它是通过用newChar替换此字符串中出现的所有oldChar而生成的； |
| boolean startsWith(String prefix)              | 如果字符串以prefix开始，则返回true；                         |
| boolean endsWith(String prefix)                | 如果字符串以prefix结尾，则返回true；                         |
| String substring(int beginIndex)               | 返回一个新字符串，该串包含从原始字符串beginIndex到串尾；     |
| String substring(int beginIndex, int endIndex) | 返回一个新字符串，该串包含从原始字符串beginIndex到串尾或endIndex-1的所有字符； |
| String toLowerCase()                           | 返回一个新字符串，该串将原始字符串中的所有大写字母改成小写字母； |
| String toUpperCase()                           | 返回一个新字符串，该串将原始字符串中的所有小写字母改成大写字母； |
| String trim()                                  | 返回一个新字符串，该串删除了原始字符串头部和尾部的空格       |

## StringBuilder和StringBuffer

StringBuilder和StringBuffer共同的父类AbstractStringBuilder

```java
abstract class AbstractStringBuilder implements Appendable, CharSequence {
    /**
     * The value is used for character storage.
     */
    char[] value;
}
```

* StringBuilder和StringBuffer是可变字符序列；
* StringBuffer JDK1.0版本提供的类，线程安全，做线程同步检查，效率较低；
* StringBuilder JDK1.5版本提供的类，线程不安全，不做线程同步检查，因此效率较高，常用;

### 常用方法

| 方法                                           | 解释说明                                                     |
| ---------------------------------------------- | ------------------------------------------------------------ |
| public StringBuilder append(...)               | 重载的方法，可以为StringBuilder对象添加字符序列，仍然返回自身对象； |
| public StringBuilder delete(int start,int end) | 可以删除从start到end-1为止的一段字符串序列，仍然返回自身对象； |
| public StringBuilder deleteCharAt(int index)   | 移除此序列索引为index的字符，仍然返回自身对象；              |
| public StringBuilder insert(...)               | 重载的方法，可以为StringBuilder对象在指定位置插入字符序列，仍然返回自身对象； |
| public StringBuilder reverse()                 | 将字符串序列逆序，仍然返回自身对象；                         |
| public String toString()                       | 返回此序列中数据的字符串表示形式；                           |
| public void setCharAt(int index,char c)        | 将c替换到原字符序列索引为index的位置；                       |
| indexOf(),substring(),length(),charAt()        | 类似String类中的方法；                                       |

## 时间处理相关类

- 以1970年1月1日 00:00:00定为基准时间，度量单位为毫秒（千分之一秒）

- 用long类型的变量保存时间；

- 获得现在时刻的“时刻数值”，

  ```java
  long now = System.currentTimeMillis();
  ```

### Date

```java
public class Date
    implements java.io.Serializable, Cloneable, Comparable<Date>
{
	public Date() {
        this(System.currentTimeMillis());
    }
}
```

- Date的构造方法返回了当前时间的毫秒值，说明新创建的Date对象的时间为当前时间；
- Date带参构造，参数是一个毫秒数，表示从基准时间过了参数毫秒值的时间；
- boolean after(Date when) 判断此时间是否在指定时间之后；
- boolean before(Date when) 判断此时间是否在指定时间之前；
- boolean equals(Date when)判断两个日期的相等性；
- long getTime() 返回自基准时间到该Date对象时间所经过的毫秒数；
- String toString() 把Date对象转换成java源码中设定格式的字符串输出；

### DateFormat

- 把时间对象转化成指定格式的字符串，反之，把指定格式的字符串转化成时间对象；
- DateFormat是一个抽象类，一般使用它的子类SimpleDateFormat类来实现

方法：

| 方法                            | 解释说明                                 |
| ------------------------------- | ---------------------------------------- |
| public String format(Date date) | 返回将date转化为指定格式的字符串；       |
| public Date parse(String str)   | 将指定格式的字符串转化为Date对象并返回； |

```java
//把时间对象转成字符串
DateFormat df = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss");
Date date = new Date();
String str = df.format(date);
//把字符串装成时间对象
Date date = df.parse("2019-9-9 11:11:11"); //需要捕获异常
```

其他格式化字符：[SimpleDateFormat](https://docs.oracle.com/javase/8/docs/api/java/text/SimpleDateFormat.html)

比如 D 表示当前时间是当前年份的第多少天

### Calendar

- Calendar类是一个抽象类，为我们提供了关于日期计算的相关功能，比如：年月日时分秒的展示和计算；
- GregorianCalendar是Calendar的一个具体子类，提供了世界上大多数国家/地区使用的标准日历系统；
- 注意：0-11表示对应的月份，0是1月，11代表12月；
- 1-7表示星期，1是星期日，7代表星期六

方法：

- get(param)：根据传的参数，获得对应的日期属性；
- set(params)：根据参数，给日期对象设置日期属性；
- add(params)：根据参数，进行日期的计算；
- Date getTime()：转化成一个时间类对象；
- Calender setTime(Date date)：将时间类对象转化为日期类；

可视化日历程序

```java
		String str = "2020-2-29";  //平年天数超出不做判断
		DateFormat df = new SimpleDateFormat("yyyy-MM-dd");
		Date date = df.parse(str); // 有异常
		Calendar c = new GregorianCalendar();
		c.setTime(date);
		int today = c.get(Calendar.DAY_OF_MONTH);// 保存当前日期为哪一天

		System.out.println(" 日\t一\t二\t三\t四\t五\t六");

		c.set(Calendar.DAY_OF_MONTH, 1); // 从当前月的第一天开始

		// 计算出当前月的第一天是周几
		for (int i = 0; i < c.get(Calendar.DAY_OF_WEEK) - 1; i++) {
			System.out.print("\t");
		}

		int days = c.getActualMaximum(Calendar.DATE); // 当前月一共有多少天
		for (int i = 1; i <= days; i++) {
			if (i == today) // 如果是今天，则在日期后面加上*
				System.out.print(c.get(Calendar.DAY_OF_MONTH) + "*\t");
			else
				System.out.print(c.get(Calendar.DAY_OF_MONTH) + "\t");

			// 周六换行
			if (c.get(Calendar.DAY_OF_WEEK) == Calendar.SATURDAY) {
				System.out.println();
			}
			c.add(Calendar.DAY_OF_MONTH, 1);
		}
```

## Math

- abs ：绝对值
- sqrt ：平方根
- pow(double a, double b) ：a的b次幂
- max，min ：最大最小值
- ceil(double a) ：大于a的最小整数
- floor(double a) ：小于a的最大整数
- random() ：返回0.0到1.0的随机数
- long round(double a) ：double类型的数据a转成long型（四舍五入）

### Random

专门用于生成随机数的类；

```java
Random rand = new Random();
// 随机生成[0,1)之间的double类型的数据
System.out.println(rand.nextDouble());

// 随机生成int类型允许范围之内的整型数据
System.out.println(rand.nextInt());
// 随机生成[0,1)之间的float类型的数据
System.out.println(rand.nextFloat());

// 随机生成false或者true
System.out.println(rand.nextBoolean());

// 随机生成[0,10)之间的int类型的数据
System.out.println(rand.nextInt(10));
// 随机生成[20,30)之间的int类型的数据
System.out.println(20 + rand.nextInt(10));
// 随机生成[20,30)之间的int类型的数据（此种方法计算较为复杂）
System.out.print(20 + (int) (rand.nextDouble() * 10));
```

## File

- java.io.File类：代表文件和目录；
- 构造方法：传入一个目录地址；

```java
File f = new File("d:\\a.txt");
//File f = new File("d:/a.txt");
```

### 常用方法：

| 方法                         | 说明                 |
| ---------------------------- | -------------------- |
| public boolean exists()      | 判断File是否存在     |
| public boolean isDirectory() | 判断File是否为目录   |
| public boolean isFile()      | 判断File是否为文件   |
| public long lastMidified()   | 返回File最后修改时间 |
| public long length()         | 返回File大小         |
| public String getName()      | 返回文件名           |
| public String getPath()      | 返回文件的目录路径   |

创建新目录：

```java
File f = new File("d:/电影/华语/大陆");
boolean flag = f.mkdir(); // false 目录结构中有一个不存在，则不会创建整个目录树
boolean flag = f.mkdirs(); //创建整个目录树
```

递归遍历目录树结构：

```java
public static void main(String[] args) {
		File  f = new File("D:\\2345\\Downloads");
		printFile(f,0);
	}
	static void  printFile(File  file, int  level){	
		//输出层数
		for(int i=0;i<level;i++){
			System.out.print("-");
		}	
		System.out.println(file.getName());
		if(file.isDirectory()){
			File[] files = file.listFiles();
			
			for(File  temp: files){
				printFile(temp,level+1);
			}	
		}
	}
```

## 枚举

- JDK1.5引入了枚举类型，枚举类型的定义包括枚举声明和枚举体。

  ```java
  enum 枚举名{
  	枚举体(常量列表)
  }
  ```

- 枚举体就是放置一些常量；

- 枚举类型隐性地继承自java.lang.Enum。

- 枚举实质上还是类，而每个被枚举的成员实质就是一个枚举类型的实例，他们默认都是public static final修饰的，提议直接通过枚举类型名使用它们；

```java
enum Season {
	SPRING, SUMMER, AUTUMN, WINTER
}
public class TestEnum {
	public static void main(String[] args) {
		Season a = Season.AUTUMN;
		switch(a){
		case SPRING:
			System.out.println("春天");
			break;
		case SUMMER:
			System.out.println("夏天");
			break;
		case AUTUMN:
			System.out.println("秋天");
			break;
		case WINTER:
			System.out.println("冬天");
			break;
		}
	}
}
```



# 容器/集合

## 1.Collection

Collection：容器，集合。

子接口List可以重复，有顺序；Set不可重复，无顺序。

![collection](https://zsy0216.github.io/image/java/javase/collection.png)

## 2.Collection接口方法

List和Set作为Collection的子接口，继承Collection的所有方法。

![collection-method](https://zsy0216.github.io/image/java/javase/collection-method.png)

## 3.List接口的实现类

ArrayList：数组实现，适合随机查找和遍历，不适合插入和删除。

LinkList：链表结构，适合数据的动态插入和删除，随机访问和遍历速度比较
慢。

Vector：数组实现，保证了线程同步

接口方法：

![list-method](https://zsy0216.github.io/image/java/javase/list-method.png)

## 4.Map接口

​	存储键值对，key-value形式，键不可重复。

接口方法：

![map-method](https://zsy0216.github.io/image/java/javase/map-method.png)

## 5.使用迭代器Iterator遍历集合

### 遍历List

```java
List<String> list = new ArrayList<>();
//add elements ...
for(Iterator<String> iter=list.iterator();iter.hasNext();{
    String temp = iter.next();
    System.out.println(temp);
}
```

### 遍历Map

第一种方式：

```java
Map<Integer,String> map = new HashMap<>();
//put elements ...
Set<Entry<Integer,String>> ss = map.entrySet();
for(Iterator<Entry<Integer,String>> iter=ss.iterator();iter.hasNext();){
	Entry<Integer,String> temp = iter.next();
    System.out.println(temp.getKey()+":"+temp.getValue());
}
```

第二种方式：

```java
Map<Integer,String> map = new HashMap<>();
Set<Integer> keySet = map.keySet();
for(Iterator<Integer> iter = keySet.iterator();iter.hasNext();)[{
    Integer key = iter.next();
    System.out.println(key+":"+map.get(key));
}
```

### 遍历Set

```java
Set<String> set = new HashSet<>();
//add elements ...
for(Iterator<String> iter=set.iterator();iter.hasNext();{
    String temp = iter.next();
    System.out.println(temp);
}
```

# 多线程

## 1.创建线程常用的两种方法

### 继承Thread类

```java
package com.tassel.thread;

/**
 * 实现线程的第一种方法 继承Thread类,重写run()方法,调用start()方法.
 */
public class MyThread extends Thread {

	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println(i + "---唱跳rap篮球");
		}
	}

	public static void main(String[] args) {
		MyThread myThread = new MyThread();
		myThread.start();

		for (int i = 0; i < 10; i++) {
			System.out.println(i + "---music!");
		}
	}
}
```

### 实现Runable接口

```java
package com.tassel.thread;

/**
 * 实现线程的第二种方法：实现Runable接口,重写run()方法.
 */
public class MyRunable implements Runnable {

	@Override
	public void run() {
		for (int i = 0; i < 10; i++) {
			System.out.println(i + "---music!");
		}
	}

	public static void main(String[] args) {
//		new Thread(new MyRunable()).start();
		MyRunable myRunable = new MyRunable();
		Thread thread = new Thread(myRunable);
		thread.start();
		
		for (int i = 0; i < 10; i++) {
			System.out.println(i + "---唱跳rap篮球");
		}
	}
}
```

## 2.java设计模式——静态代理

- RealSubject：真实角色，是实现抽象接口的类。
- Proxy：代理角色，内部含有对真实对象`RealSubject`的引用，从而可以操作真实对象。代理对象提供与真实对象相同的接口，以便在任何时刻都能代替真实对象。同时，代理对象可以在执行真实对象操作时，附加其他的操作，相当于对真实对象进行封装。
- Subject : 接口，是对象和它的代理共用的接口，让`RealSubject`和`Proxy`具有一致性。

![](https://upload-images.jianshu.io/upload_images/11247007-677518be6c8bf05c.png?imageMogr2/auto-orient/strip|imageView2/2/w/758/format/webp)

```java
package com.tassel.thread;

/**
 * 实现静态代理 公共接口： 1.真实角色（对象） 2.代理角色（对象）
 *
 */
public class StaticProxy {
	public static void main(String[] args) {
		new WeddingCompany(new You()).happyMarry();

		// 类似多进程的
		// new Thread(线程对象).start();
	}
}

// 公共接口
interface Marry {
	void happyMarry();
}

// 真实角色
class You implements Marry {
	@Override
	public void happyMarry() {
		System.out.println("真实角色：我要结婚");
	}
}

// 代理角色
class WeddingCompany implements Marry {

	private Marry target;

	public WeddingCompany(Marry target) {
		this.target = target;
	}

	@Override
	public void happyMarry() {
		System.out.println("代理角色：有人要结婚接个活");
		target.happyMarry();
		System.out.println("代理角色：人家结完婚了，咱该走了");
	}
}
```

## 3.jdk8 lambda表达式

### 介绍

* λ：希腊字母表中排序第十一位的字母，英语名称为Lambda。最早出现是用于计算的[λ演算`lambda calculus`，后来被函数式编程语言广泛采用。

* 语法：

  - 前面的小括号内可以传一个到多个参数，参数类型可以省略，lambda表达式会自动匹配对应的类型；

    参数只有一个时，可省略参数类型和小括号；

  - 后面的表达式只有一行时，可省略花括号；

  - 当借口方法存在返回值，且后面的表达式只有一行，可以省略花括号和return；

```java
(parameters) -> expression
(parameters) -> statement
(parameters) -> { statements; }
```

* 注意：使用lambda需要保证实现的接口只有一个方法！

### lambda推导示例

外部类->静态内部类->局部内部类->匿名内部类->lambda表达式

```java
package com.tassel.thread;
/**
 * lambda推导
 */

//1.外部类
class Like implements ILike{
	@Override
	public void lambda() {
		System.out.println("i like lambda ");
	}
}

//lambda实现的接口方法必须只有一个，否则会报错
interface ILike{
	void lambda();
}

public class LambdaTest01 {
	//2.静态内部类
	static class Like2 implements ILike{
		public void lambda() {
			System.out.println("i like lambda2 ");
		}
	}
	public static void main(String[] args) {
		ILike like = new Like();
		like.lambda();
		
		like = new Like2();
		like.lambda();
		
		//3.局部内部类
		class Like3 implements ILike{
			public void lambda() {
				System.out.println("i like lambda3 ");
			}	
		}
		
		like = new Like3();
		like.lambda();
		//4.匿名内部类
		like = new ILike() {
			public void lambda() {
				System.out.println("i like lambda4 ");
			}
		};
		like.lambda();
		//5.lambda
		like = ()-> {
			System.out.println("i like lambda5 ");
		};
		like.lambda();

		/*
		 *lambda推导必须存在类型
		()-> {
			System.out.println("i like lambda5 ");
		}.lambda();
		*/
	}
}
```

### lambda实现多线程

```java
package com.tassel.thread;
/**
 * lambda 简略实现多线程
 */
public class LambdaThread {
	public static void main(String[] args) {
        //表达式存在多行，不能省略大括号
		new Thread(()->{
			for(int i=0;i<100;i++) {
				System.out.println("一边删库");
			}
		}) .start();
		//只有一行时，可以省略大括号
		new Thread(()->	System.out.println("一边跑路")) .start();
	}
}
```

## 4.线程状态

- 初始状态：实现Runable接口或继承Thread类的子类new实例化后，线程进入初始状态；
- 就绪状态：等待调度，进入运行状态；
  - 当线程对象调用start()方法，线程进入就绪状态（并没有开始执行）；
  - 阻塞事件解除进入就绪状态；
  - 运行状态下调用yield()方法，进入就绪状态；
  - jvm将cpu从本机线程切换到其他线程，子线程进入就绪状态；
- 运行状态：就绪状态的线程被cpu所调度，进入运行状态；
- 阻塞状态：
  - sleep()
  - wait()
  - join()
  - IO操作，read，write...
- 终止状态：线程运行结束，或被终止；