# Java基础

## JDK和JRE有什么区别？

- `JDK`：`Java Development Kit`的缩写，Java开发工具包，包含了JAVA的运行环境（`JVM`+Java系统类库）和JAVA工具。
- `JRE`：`Java Runtime Environment`的缩写，Java运行环境，包含JVM标准实现及Java核心类库。

## java数据类型声明后可以直接输出吗？

Java中所有定义的基本类型或对象都必须初始化才能输出值；

否则无法通过编译

## ==和equals的区别

- `==`是一个比较运算符
  - 对于基本类型，比较的是具体的数值（`int`、`double`...）；
  - 对于引用类型，比较的是引用,也就是对应的内存地址；
- `equals`是超类`Object`就具有的方法，因此所有的引用类型都具有这个方法，只用来比较引用数据类型；
- `equals`方法默认比较对象内存地址，如果重写该方法（例如`String`，`Integer`等）比较的也是对应的属性值；

`Object`类中`equals`方法源码：

由此可见，`equals`方法判断的本质也是`==`。当两个对象进行判断时，归根结底是判断这二者的内存地址是否相同（重写`equals`方法的类对象(`String`,`Integer`...)除外）。

```java
public boolean equals(Object obj) {
    return (this == obj);
}
```

`new`对象是在堆内存中开辟一份空间，所以其引用变量就是指向了堆内存的地址;

图示：

![equalsVS==](https://zsy0216.github.io//image/java/javase/javases/equalsVS==.png)

示例：

```java
String a = "abcde";
String b = "abcde";
String c = new String("abcde");

System.out.println(a==b); //true
System.out.println(a==c); //false
System.out.println(a.equals(b)); //true
System.out.println(a.equals(c)); //true
```

如上述所说，`String`类重写了父类`Object`的`equals`方法，所以最后一个`equals`方法判断的也是属性值；

`String`类中`equals`方法源码：

```java
public boolean equals(Object anObject) {
    if (this == anObject) { 
        return true;
    }
    if (anObject instanceof String) {
        String anotherString = (String)anObject;
        int n = value.length;
        if (n == anotherString.value.length) {
            char v1[] = value;
            char v2[] = anotherString.value;
            int i = 0;
            while (n-- != 0) {
                if (v1[i] != v2[i])
                    return false;
                i++;
            }
            return true;
        }
    }
    return false;
}
```

## 两个对象的hashCode()相同，则equals()一定为true？

- 如果两个对象相等(`equals()`值为true)，那么它们的`hashCode()`一定相同；
- 如果两个对象的`hashCode()`相同，它们的`equals()`**不一定**为true；

## final在Java中有什么作用?

- `final`可以用来修饰类、方法、变量（包括成员变量和局部变量)。

1. **被final修饰的类不能被继承**；
2. **被final修饰的方法不能被重写（覆盖）**
   * `final`类中的成员方法都会被隐式地指定为`final`方法；
   * 如果父类中`final`修饰的方法同时访问控制权限为`private`，将会导致子类中不能直接继承到此方法，因此，此时可以在子类中定义相同的方法名和参数，此时不再产生重写与`final`的矛盾，而是在子类中重新定义了新的方法。
   * `private`方法通常被自动认为是`final`方法（并不是）
3. **被final修饰的成员变量表示为常量**，必须在初始化时(声明时，或构造函数中)赋值，赋值后不可变；
   - `final`修饰的类属性可以是基本类型也可以是引用类型，如果是基本类型就不能再被赋值；
   - 如果是引用类型，就不能再指向其他引用地址，但**该引用指向的对象内容是可以改变的**。
   - 本质上是一回事，因为引用的值是一个地址，`final`要求值，即地址的值不发生变化。

## String属于基本数据类型吗？

- String 是特殊的引用类型

Java八大基础数据类型有：byte、boolean、char、short、int、float、long、double；

1. `String`是特殊的引用类型并且是`final`的，JVM使用字符串常量池储存字符串数据。创建新的字符串时，JVM首先会去字符串常量池中寻找有没有该字符串，如果没有就添加到该常量池，如果有就返回该字符串在常量池中的引用。
2. `String s = "a"; s += "b";`，这段代码执行前后，字符串常量池中将出现`a`和`ab`两个字符串常量，而原本`s`变量的引用指向了常量池中`ab`。
3. `String s = new String("ab")`，这段代码一共创建了几个对象？**一个或两个**。如果字符串常量池中有了`ab`这个字符串(比如在此之前已经使用了`String str = "ab"`)，那么新的`s`对象引用其实仅仅是指向了字符串常量中的`ab`，并没有创建新的字符串对象。**但是**，每次调用`new`都会在堆内存开辟空间，创建一个String对象，这是肯定的。

### String、StringBuffer、StringBuilder的区别

1. `String`是不可变字符串，`StringBuffer`和`StringBuilder`是可变字符串。如果经常改变字符串的原始数据，最好使用`StringBuilder`代替。
2. `String`默认重写了`equals`方法和`hashCode()`方法；而`StringBuffer`没有重写`equals`方法，使用`new StringBuffer("")`会直接在堆内存中开辟空间储存对象。因此将`StringBuffer`对象储存进Java集合中可能会出现问题。
3. `StringBuffer`是线程安全的，效率低；`StringBuilder`是线程不安全的，效率高。

### String str="i"与 String str=new String("i")一样吗？

不一样，因为内存的分配方式不一样。String str="i"的方式，java 虚拟机会将其分配到常量池中；而 String str=new String("i") 则会被分到堆内存中。

### 如何将字符串反转？

- 使用 StringBuilder 或者 stringBuffer 的 reverse() 方法。

示例代码：

```java
// StringBuffer reverse
StringBuffer stringBuffer = new StringBuffer();
stringBuffer.append("abcdefg");
System.out.println(stringBuffer.reverse()); // gfedcba

// StringBuilder reverse
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("abcdefg");
System.out.println(stringBuilder.reverse()); // gfedcba
```

###  String 类的常用方法都有那些？

- indexOf()：返回指定字符的索引。
- charAt()：返回指定索引处的字符。
- replace()：字符串替换。
- trim()：去除字符串两端空白。
- split()：分割字符串，返回一个分割后的字符串数组。
- getBytes()：返回字符串的 byte 类型数组。
- length()：返回字符串长度。
- toLowerCase()：将字符串转成小写字母。
- toUpperCase()：将字符串转成大写字符。
- substring()：截取字符串。
- equals()：字符串比较。

## Java中的Math.round(-1.5)等于多少？

等于-1，因为数轴上取值时，中间值（0.5）向右取整，所以正 0.5 是往上取整，负 0.5 是直接舍弃。

```java
System.out.println(Math.round(-1.5)); //-1
System.out.println(Math.round(1.5));  //2
System.out.println(Math.round(-1.4)); //-1
System.out.println(Math.round(1.4));  //1
System.out.println(Math.round(-1.6)); //-2
```

## static关键字？是否可覆盖？

`static`关键字表明一个成员变量或者是成员方法在没有所属的类实例变量的情况下允许被访问。被`static`标记的方法不能被覆盖，因为方法的覆盖是基于运行时动态绑定的，而`static`方法在编译时就已经和该类绑定了，也就是`static`标记的变量、方法都是随着类的加载而加载的，所以可以通过不创建对象实例就能访问该变量、方法；而不创建对象实例就访问非`static`变量、方法是不行的，因为此时变量、方法还没有被创建。

- `static`关键字修饰的变量属于类变量，不需要实例化对象就可以使用，使用方式：`类名.变量名`；
- `static`关键字修饰的方法属于类方法，不需要实例化对象就可以使用，使用方式：`类名.方法名`；
  - 被`static`标记的方法不能被覆盖（重写）；
  - `static`方法只能调用`static`属性和`static`方法；
  - `非static`方法可以调用`static`属性和`static`方法；

## Overriding和Overloaded

方法的重写Overriding和方法的重载Overloaded都是Java多态性的一种表现。

- `Overriding`: 子类中定义了和父类中名称、参数列表相同的方法；
- `Overloaded`: 方法名相同，参数列表不同；

## Java支持多继承吗？

不支持。

- Java中每个类都只能继承一个类，可以实现多个接口。但是可以通过**多层**继承实现。

- 类定义属性和方法，描述某一类事物的抽象。而接口定义了行为，并不限于任何具体意向。

- 从逻辑上说，单继承更加明确指出一个子类就应该是其父类代表的事物中某个更具体的类别。而接口则不同，接口定义了一些公共行为。因此类对接口的implements称为实现而不能称为继承。

## 关于抽象类

### 普通类和抽象类的区别

- 普通类不能包含抽象方法，抽象类可以包含抽象方法。
- 抽象类不能直接实例化，普通类可以直接实例化

### 接口与抽象类的区别

- 接口中所有的方法隐含都是抽象的。而抽象类则可以同时包含抽象和非抽象的方法；
- 接口中定义的方法必须是不包含具体实现的（即隐式的抽象方法）；但抽象类中既可以定义抽象方法（不包含具体实现）又可以定义非抽象方法 ( 包含具体实现 )；
- 类实现接口就必须实现接口中定义的所有方法；但继承一个抽象类，可以不实现抽象类中的抽象方法 ( 但前提是这个类也是抽象的 )；
- 类可以实现很多个接口，但是只能继承一个抽象类；
- 抽象类中可以有构造函数，可以有main方法并可以运行，而接口中都不存在；
- 抽象类可以是不提供接口方法实现的情况下实现接口；
- 接口中声明的变量默认都是final的；但抽象类可以包含非final的变量；
- 接口中的成员方法默认都是public的；但抽象类中的成员函数可以是private、protected、public；
- 接口是绝对抽象的，不可以被实例化；

### 抽象类能被final修饰吗？

不能，定义抽象类就是让其他类继承的，

如果定义为 final 该类就不能被继承，这样彼此就会产生矛盾。

## 引用传递和值传递

Java中采用值传递的方式。

例证：

```java
public class TestTransfer {
	public static void main(String[] args) {
		int a = 1;
		int b = 2;
		swap(a, b);
		System.out.println(a); //1
		System.out.println(b); //2

		int[] arr = { 1, 2 };
		swap(arr);
		System.out.println(Arrays.toString(arr)); //[10, 2]
	}
    private static void swap(int a, int b) {
		a = 10;
		b = 20;
	}
	private static void swap(int[] arr) {
		arr[0] = 10;
	}
}
```

方法传递时传递的参数其实仅值原参数的一个拷贝，对于基本类型传递的就是具体的值的拷贝，对于引用类型传递的是对象地址。

所以上述代码中的，基本数据类型a，b经过swap方法并没有改变其原始值；而引用类型arr数组经过swap方法就改变了原始值（引用类型直接对对象堆内存地址数据操作）。

## IO流

### Java IO流的分类

- 按功能分：输入流(Input)、输出流(Output);
- 按类型分：字节流、字符流；

### 字符流和字节流的区别？

数据的输入和输出在计算机中最终都是通过字节的形式传递的，对应通过`InputStream`和`OutputStream`实现，他们都是针对字节操作的。

而有时候通常需要读取一些完全是字符的文本数据，通常使用基于字节流的包装类字符流完成操作，他们通过`InputStreamReader`和`OutputStreamWriter`实现。

字符流是字节流的包装，即使有时候读取的是字符流，但也可能需要转换为字节写入。

字节流按 8 位传输以字节为单位输入输出数据，字符流按 16 位传输以字符为单位输入输出数据。

# 容器

## List、Set、Map之间的区别

| 比较           | List                                                    | Set                                                      | Map                                                          |
| -------------- | ------------------------------------------------------- | -------------------------------------------------------- | ------------------------------------------------------------ |
| 继承接口       | Collection                                              | Collection                                               |                                                              |
| 常见实现类     | AbstractList(其常用子类有ArrayList、LinkedList、Vector) | AbstractSet(其常用子类有HashSet、LinkedHashSet、TreeSet) | HashMap、HashTable、TreeMap                                  |
| 常见方法       | add()、remove()、clear()、get()、contains()、size()     | add()、remove()、clear()、contains()、size()             | put()、get()、remove()、clear()、containsKey()、containsValue()、keySet()、values()、size() |
| 元素是否可重复 | 可重复                                                  | 不可重复(用equals判断)                                   | 不可重复                                                     |
| 是否有序       | 有序                                                    | 无序(实际上由HashCode决定)                               |                                                              |
| 线程是否安全   | Vector线程安全                                          |                                                          | HashTable线程安全                                            |

- `List`: `List`接口储存一组不唯一 (可以有多个元素引用引用相同的对象)，有序的对象，可插入多条`null`元素；
- `Set`: 不允许重复的集合，不允许有多个元素引用相同的对象，只允许有一个`null`元素；
- `Map`: 使用键值对存储，`Map`会维护与`Key`有关联的值，两个`Key`可以引用相同的对象，但`Key`不能重复；

## Array和ArrayList有什么区别？

- `Array`可以包含基本类型和对象类型；`ArrayList`只能包含对象类型
- `Array`大小是固定的；`ArrayList`大小是动态变化的
- `ArrayList`提供了诸如`addAll()`、`removeAll()`、`iterator()`方法等
- 对于基本数据类型，集合使用自动装箱来减少代码量；但当处理固定大小的基本类型数据时，这种方式相对较慢。

## 数组与List之间的转换

- List转换成为数组：调用ArrayList的toArray方法。
- 数组转换成为List：调用Arrays的asList方法。

## ArrayList和LinkedList的区别

1. **是否保证线程安全**： `ArrayList`和`LinkedList`都是不同步的，也就是不保证线程安全；
2. **底层数据结构**： `ArrayList`底层使用的是`Object` **数组**；`LinkedList`底层使用的是 **双向循环链表** 结构；
3. **插入和删除是否受元素位置影响？** `ArrayList`采用数组储存，所以插入和删除元素都受元素位置的影响；`LinkedList` 采用链表储存，所以插入、删除元素都不受元素位置影响。
4. **是否支持快速随机访问？** `LinkedList`因为使用链表储存，无法通过元素索引快速访问；而`ArrayList`因为底层采用`Object`数组储存，可以通过索引快速随机访问。使用下标访问一个元素，ArrayList 的时间复杂度是 O(1)，而 LinkedList 是 O(n)。
5. **内存空间占用**： `ArrayList`的空间浪费主要体现在在List列表的结尾都会预留一定的空间容量，而`LinkedList`的空间花费体现在他的每一个元素都需要消耗比`ArrayList`更多的空间（因为要储存直接后继和直接前驱以及数据）。

## ArrayList 和 Vector 的区别是什么？

- Vector是同步的，而ArrayList不是。然而，如果你寻求在迭代的时候对列表进行改变，你应该使用CopyOnWriteArrayList。 
- ArrayList比Vector快，它因为有同步，不会过载。 
- ArrayList更加通用，因为我们可以使用Collections工具类轻易地获取同步列表和只读列表。

## ArrayList和Vector和LinkedList的区别？

- ArrayList: 底层数据结构是数组，查询快，增删慢。线程不安全，效率高
- Vector: 底层数据结构是数组，查询快，增删慢。线程安全，效率低
- LinkedList: 底层数据结构是链表，查询慢，增删快。线程不安全，效率高

## 谈谈ArrayList的扩容机制

Java中基本数组都是定长的，一旦被实例化后就不能改变其长度，意味着创建数组时必须确定数组的容量大小。而很多情况下，数组的长度不是确定的，需要动态增减，`ArrayList`的出现就解决了这一问题。

`ArrayList`的扩容机制表现在`add()`方法上，先看`add()`方法的源码：

```java
public boolean add(E e) {
    ensureCapacityInternal(size + 1);  // Increments modCount!!
    elementData[size++] = e;
    return true;
}

private void ensureCapacityInternal(int minCapacity) {
    ensureExplicitCapacity(calculateCapacity(elementData, minCapacity));
}

//获取最小容量
private static int calculateCapacity(Object[] elementData, int minCapacity) {
    if (elementData == DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
        return Math.max(DEFAULT_CAPACITY, minCapacity);
    }
    return minCapacity;
}

//判断是否需要扩容
private void ensureExplicitCapacity(int minCapacity) {
    modCount++;

    // overflow-conscious code
    if (minCapacity - elementData.length > 0)
        grow(minCapacity);
}
```

当向`ArrayList`对象中添加新元素时，首先会调用`ensureCapacityInternal(size)`方法，`size`为最小扩容量；`ensureCapacityInternal()`方法会首先调用`calculateCapacity`来确定需要的最小容量；最后调用`ensureExplicitCapacity()`方法判断是否需要扩容。最后判断所需最小容量如果大于当前数组的空间大小，则需要扩容，调用`grow()`方法扩容：

```java
private void grow(int minCapacity) {
    // 获取ArrayList中elementDaata数组的长度
    int oldCapacity = elementData.length;
    // 扩容至原来的1.5倍
    int newCapacity = oldCapacity + (oldCapacity >> 1);
    // 判断新的数组容量够不够
    // 够了就直接使用这个长度创建新数组
    if (newCapacity - minCapacity < 0)
        // 不够就将数组的长度设置为需要的长度
        newCapacity = minCapacity;
    // 检查此时的最大值是否溢出
    if (newCapacity - MAX_ARRAY_SIZE > 0)
        newCapacity = hugeCapacity(minCapacity);
    // 调用Arrays.copyOf()将elementData数组数据拷贝到新数组
    // 并将elementData指向新数组newCapacity的内存地址
    elementData = Arrays.copyOf(elementData, newCapacity);
}
```

**总结**： `ArrayList`扩容的本质就是计算所需扩容size得到新的数组，将原数组中的数据复制到新数组中，最后将原数组指向新数组在堆内存的引用地址即可。

##  HashMap 和 Hashtable 有什么区别？

- `hashMap`去掉了`HashTable `的`contains`方法，但是加上了`containsValue（）`和`containsKey（）`方法；
- `HashMap`和`HashTable`都实现了`Map`接口，主要区别在线程安全性、同步、速度；
- **线程是否安全**： `HashMap`非同步线程不安全，`HashTable`同步线程安全。`HashTable`内部的方法都经过`synchronized`修饰；
- **效率**: `HashMap`线程不安全，效率高；`HashTable`线程安全，效率低；
- **对null key和null value的支持**： `HashMap`中，`null`可以作为key，这样的key只有一个，但可以有多个key对应的值为null；在`HashTable`中的key不能为null；
- **底层数据结构**： JDK1.8后的`HashMap`在解决哈希冲突时有了较大的变化，当链表长度大于阀值时（默认是8），将链表转换为红黑树，以减少搜索时间。`HashTable`没有这样的机制；

## HashMap和HashSet区别?

`HashSet`底层采用`HashMap`实现

| HashMap                          | HashSet                                                      |
| :------------------------------- | :----------------------------------------------------------- |
| 实现了Map接口                    | 实现了Set接口                                                |
| 储存键值堆                       | 仅储存对象                                                   |
| 调用`put()`向Map中添加元素       | 调用`add()`向Set中添加元素                                   |
| `HashMap`使用`Key`计算`HashCode` | `HashSet`使用成员对象来计算`hashCode`值，对于两个对象来说，`hashCode`可能相同，所以用`equals`判断对象的相等性 |

## HashSet如何检查重复？

在前面讲`hashCode`和`equals`时就提到了，`HashSet`集合同样适用。向`HashSet`中存入一个元素，`HashSet`首先会根据对象的`hashCode`值判断当期集合中此`hashCode`对应的位置有没有值，如果没有就直接添加，如果有就再调用`equals`方法比较两个对象是否相同，相同就不再储存（保证了`Set`集合不重复的特性），否则就散列到其他位置储存。

## 如何决定使用 HashMap 还是 TreeMap？

对于在Map中插入、删除和定位元素这类操作，`HashMap`是最好的选择。然而，假如你需要对一个有序的key集合进行遍历，`TreeMap`是更好的选择。基于你的`collection`的大小，也许向`HashMap`中添加元素会更快，将Map换为`TreeMap`进行有序key的遍历。

## 说一下 HashMap 的实现原理？

- **<=JDK1.7**: Table数组 + Entry链表
- **>=JDK1.8**: Table数组 + Entry链表/红黑树

`HashMap`概述：` HashMap`是基于哈希表的Map接口的非同步实现。此实现提供所有可选的映射操作，并允许使用`null`值和`null`键。此类不保证映射的顺序，特别是它不保证该顺序恒久不变。 

`HashMap`的数据结构： 在java编程语言中，最基本的结构就是两种，一个是数组，另外一个是模拟指针（引用），所有的数据结构都可以用这两个基本结构来构造的，`HashMap`也不例外。`HashMap`实际上是一个“链表散列”的数据结构，即数组和链表的结合体。

当我们往`Hashmap`中`put`元素时,首先根据`key`的`hashcode`重新计算`hash`值,根据`hash`值得到这个元素在数组中的位置(下标)，如果该数组在该位置上已经存放了其他元素，那么在这个位置上的元素将以链表的形式存放，新加入的放在链头，最先加入的放入链尾；如果数组中该位置没有元素，就直接将该元素放到数组的该位置上。

需要注意`Jdk 1.8`中对`HashMap`的实现做了优化,当链表中的节点数据超过八个之后,该链表会转为红黑树来提高查询效率,从原来的`O(n)`到`O(logn)`

## 说一下 HashSet 的实现原理？

- `HashSet`底层由`HashMap`实现
- `HashSet`的值存放于`HashMap`的`key`上
- `HashMap`的`value`统一为`PRESENT`

## Collection 和 Collections 有什么区别？

- `java.util.Collection`是一个集合接口（集合类的一个顶级接口）。它提供了对集合对象进行基本操作的通用接口方法。Collection接口在Java 类库中有很多具体的实现。Collection接口的意义是为各种具体的集合提供了最大化的统一操作方式，其直接继承接口有List与Set。
- `Collections`则是集合类的一个工具类/帮助类，其中提供了一系列静态方法，用于对集合中元素进行排序、搜索以及线程安全等各种操作。

## Comparable和Comparator的区别？

- `Comparable`接口来自`java.lang`包，提供`compareTo(Object obj)` 方法排序
- `Comparator`接口来自`java.util`包，提供`compare(Object obj1, Object obj2)`方法排序

当需要对一个集合采用一种方式排序，使用`Comparable`接口；如果需要对一个集合采用两种排序方式就使用`Comparator`接口。

## 迭代器

### 什么是迭代器？

`Iterator`接口中提供了很多对集合元素迭代的方法。每个集合中都有可以返回迭代器对象的方法`iterator()`。迭代器在迭代的过程中可以删除底层集合的元素。

### Iterator和ListIterator的区别？

- `Iterator`可以用来遍历Set和List集合，但是`ListIterator`只能遍历List
- `Iterator`对集合只能向前遍历（`next()`）；而`ListIterator`可以向前遍历（`next()`），也可以向后遍历（`previous()`）
- `ListIterator`实现了`Iterator`接口

## Java集合框架总结

### Collection

![collection](https://zsy0216.github.io//image/java/javase/javases/collection.png)

#### List

- **ArrayList**: Object数组，线程不安全，查询快，增删慢，效率高
- **Vector**: Object数组，线程安全，查询快，增删慢，效率低
- **LinkedList**: 双向链表，线程不安全，查询慢，增删快，效率高

#### Set

- **HashSet**: 无序、唯一，基于HashMap实现，底层采用HashMap存储元素
- **LinkedHashSet**: LinkedHashSet继承自HashSet，并且其内部通过LinkedHashMap实现
- **TreeSet**: 有序、唯一，红黑树

### Map

![map](https://zsy0216.github.io/image/java/javase/javases/map.png)

- **HashMap**: JDK1.8之前HashMap由数组和链表组成，数组时HashMap的主体，链表是为了解决Hash冲突问题。JDK1.8之后当Table中Node数量大于8时，就将链表转换为红黑树，以减少搜索时间提高效率。
- **LinkedHashMap**: LinkedHashMap继承自HashMap，所有他的底层仍然由数组和链表/红黑树实现。另外，LinkedHashMap在上面的结构基础上，增加了一条双向链表，使得上面的结构可以保持键值对的插入顺序。
- **HashTable**: 数组+链表组成。数组时HashTable的主体，链表是为了解决Hash冲突问题
- **TreeMap**: 红黑树

# Java并发

## 并行和并发的区别？

- 并行是指两个或多个事件在同一**时刻**发生；而并发是指两个或多个事件在同一**时间间隔**内发生；
- 并行是在**不同实体**上的多个事件；并发是在**同一实体**上的多个事件；
- 在一台处理器上“同时”处理多个任务，在多台处理器上同时处理多个任务。如hadoop分布式集群。

所以并发编程的目标是充分的利用处理器的每一个核，以达到最高的处理性能。

## 线程和进程的区别？

- 进程是程序的一次执行过程，是系统运行程序的基本单位
- 线程与进程类似，但线程是一个比进程更小的执行单位。一个进程执行过程中可以产生多个线程，

在Java中，启用一个`main`方法就是启动了一个JVM进程，而`main`函数所在的线程就是这个进程中的一个线程，也称为主线程。

## 从JVM角度分析进程和线程的关系

根据JVM的内存划分，对于线程而言：多个线程**共享进程的堆、方法区资源**，但每个线程又有自己的程序计数器、虚拟机栈、本地方法栈。

![jvmthread](https://zsy0216.github.io/image/java/javase/javases/jvmthread.png)

也就是说，在一个JVM进程中，可以存在多个线程，每个线程都共享了这个JVM进程的方法区、堆；并且每个线程又都具有自己的虚拟机栈、本地方法栈、程序计数器等。

### 为什么方法区和堆是线程共享区？

- **方法区（Method Area）** 存储已被虚拟机加载的类信息、常量、静态变量等数据。方法区中又包含 **运行时常量池** ，这部分区域储存Class文件信息和编译期生成的各种字面量和符号引用。
- **堆（Heap）** 堆内存储存了对象实例（比如`new`关键字创建的实例对象），它是JVM中内存区最大的一块区域。

所以，一个进程的启动可能包含了多个线程，而这个进程中的静态变量等都是随着类加载而加载的，他应该不属于某个线程独有，所以将其存储于方法区中。对象实例都储存在Java堆内存中，作为Java最大的一块内存区域，肯定不能是某个线程独占的。

### 为什么虚拟机栈和本地方法栈是线程独占区？

- **虚拟机栈**： 每个Java方法执行的同时都会创建一个栈帧储存局部变量表、操作数栈、方法出口等。从方法的执行到结束，对应将栈帧压入Java虚拟机栈和从虚拟机栈中弹出的过程。
- **本地方法栈**： 本地方法栈类似Java虚拟机栈，只不过Java虚拟机栈为虚拟机执行Java方法服务，而本地方法栈则为虚拟机使用到的`native`方法服务。

### 程序计数器是什么？

**程序计数器（Program Counter Register）**：当前线程执行的字节码的行号指示器。每个线程都有独立的程序计数器。此内存区域是Java虚拟机中唯一一个没有任何OutOfMemoryError情况的区域。

## 使用多线程可能带来什么问题？

并发编程的目的就是提高程序的执行效率，但并发编程可能造成：内存泄漏、上下文切换、死锁等问题

## 关于线程状态

线程通常都有五种状态，创建、就绪、运行、阻塞和死亡。

- 创建状态。在生成线程对象，并没有调用该对象的start方法，这是线程处于创建状态。
- 就绪状态。当调用了线程对象的start方法之后，该线程就进入了就绪状态，但是此时线程调度程序还没有把该线程设置为当前线程，此时处于就绪状态。在线程运行之后，从等待或者睡眠中回来之后，也会处于就绪状态。
- 运行状态。线程调度程序将处于就绪状态的线程设置为当前线程，此时线程就进入了运行状态，开始运行run函数当中的代码。
- 阻塞状态。线程正在运行的时候，被暂停，通常是为了等待某个时间的发生(比如说某项资源就绪)之后再继续运行。sleep,suspend，wait等方法都可以导致线程阻塞。
- 死亡状态。如果一个线程的run方法执行结束或者调用stop方法后，该线程就会死亡。对于已经死亡的线程，无法再使用start方法令其进入就绪

![threadstatus](https://zsy0216.github.io/image/java/javase/javases/threadstatus.png)

## 什么是上下文切换？

简单来说，并发编程中实际线程的数量都可能大于CPU核心的个数，而CPU一个核心在任意时刻只能被一个线程使用，CPU为了保证并发的线程都有被执行，采用**随机分配时间片并轮转**的方式；而一个线程的时间片用户将保存并进入就绪状态直到下次分配时间片再执行，这个 **任务从保存到再加载的过程就是一次上下文切换**。

## 说说sleep()方法和wait()方法的区别？

两者最主要的区别在于：**sleep 方法没有释放锁，而 wait 方法释放了锁**

- 两者都可以暂停线程的执行
- `wait()`通常用于线程间交互/通信，`sleep()`通常用户暂停执行
- `wait()`方法被调用后，线程不会自动苏醒，需要别的线程调用同一对象上的`notify()`或者`notifyAll()`方法。`sleep()`方法执行完成后，线程会自动苏醒。

## 什么是死锁？如何避免?

举例：线程A持有资源2，线程B持有资源1，在线程A、B都没有释放自己所持有资源的情况下（锁未释放），他们都想同时获取对方的资源，因为资源1、2都被锁定，两个线程都会进入相互等待的情况，这种情况称为死锁。

![deadlock](https://zsy0216.github.io/image/java/javase/javases/deadlock.png)

栗子：

```java
public class DeadLockDemo {
    private static Object resource1 = new Object();//资源 1
    private static Object resource2 = new Object();//资源 2

    public static void main(String[] args) {
        new Thread(() -> {
            synchronized (resource1) {
                System.out.println(Thread.currentThread() + "get resource1");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource2");
                synchronized (resource2) {
                    System.out.println(Thread.currentThread() + "get resource2");
                }
            }
        }, "线程 1").start();

        new Thread(() -> {
            synchronized (resource2) {
                System.out.println(Thread.currentThread() + "get resource2");
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
                System.out.println(Thread.currentThread() + "waiting get resource1");
                synchronized (resource1) {
                    System.out.println(Thread.currentThread() + "get resource1");
                }
            }
        }, "线程 2").start();
    }
}
```

Output:

```java
Thread[线程 1,5,main]get resource1
Thread[线程 2,5,main]get resource2
Thread[线程 1,5,main]waiting get resource2
Thread[线程 2,5,main]waiting get resource1
```

线程1以`resource1`作为同步监视器，即可以轻松获取`resource1`同时也锁定了`resource1`，此时调用`sleep`让线程1等待1秒钟；此时线程2开始执行，他以`resource2`作为同步监视器同时也锁定了`resource2`，此时调用`sleep`让线程2等待1秒钟；而此时线程1等待1秒已经结束了，当他想要获取`resource2`时发现`resource2`已经被线程2锁定了，同理线程2结束等待后想要获取`resource1`时发现`resource1`已经被线程1锁定了。那么两者都无法同时获取对方的线程，便进入死锁状态。

因此产生死锁需要具备以下四个条件：

1. 互斥条件：该资源任意一个时刻只能由一个线程占用
2. 请求和保持条件：一个线程因请求资源而阻塞时，对已获取的资源保持不放
3. 不剥夺条件：线程已获取的资源在未使用完之前不能被其他线程强行剥夺，只有自己使用完毕后才使用资源
4. 循环等待条件：若干进程之前形成一种头尾相接的循环等待资源关系。

避免死锁就要破坏这四个条件中任意一个：

1. 破坏互斥条件：这个条件我们无法破坏，因为我们用锁的目的就是想让他们互斥
2. 破坏请求与保持条件：一次性申请所有资源
3. 破坏循环等待条件：按照一定顺序申请资源，避免资源的循环使用

解决方案: 修改线程2

```java
new Thread(() -> {
    synchronized (resource1) {
        System.out.println(Thread.currentThread() + "get resource1");
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(Thread.currentThread() + "waiting get resource2");
        synchronized (resource2) {
            System.out.println(Thread.currentThread() + "get resource2");
        }
    }
}, "线程 2").start();
```

Outout:

```java
Thread[线程 1,5,main]get resource1
Thread[线程 1,5,main]waiting get resource2
Thread[线程 1,5,main]get resource2
Thread[线程 2,5,main]get resource1
Thread[线程 2,5,main]waiting get resource2
Thread[线程 2,5,main]get resource2
Process finished with exit code 0
```

## 调用start()方法会执行run()方法，为什么不能直接调用run()方法？

`new`一个`Thread`，线程进入了新建状态；调用`start()`方法，会启用一个线程并使线程进入就绪状态，当分配到时间片后就可以开始执行。`start()`会执行线程的相应准备工作，然后自动执行`run()`方法的内容，这才是真正的多线程工作。而直接执行`run()`方法，会吧`run()`方法当做一个`main`线程下的一个普通方法去执行，并不会在某个线程中执行他。

**总结：调用start方法可以启动线程并使线程进入就绪状态，而run()方法只是Thread的一个普通方法调用，还是在main主线程里执行，并不会在一个新线程中执行**

## synchronized关键字

`synchronized`关键字解决多个线程之间访问资源的同步性，`synchronized`关键字可以保证它修饰的方法或代码块在任意时刻只能有一个线程执行。

**synchronized关键字最主要的三种使用方式**：

- **修饰实例方法**： 给当前对象加锁，进入同步代码块前要获取当前对象实例的锁

```java
// 此处的synchronized就相当于synchronized(this)，锁定的是当前对象
public synchronized void add() {}
```

- **修饰静态方法**： 给当前类加锁（因为静态方法没有`this`），会作用于当前类的所有对象实例，因为静态成员不属于任何一个实例对象，是一个类成员。

```java
// 此处的synchronized就相当于synzhronized(T.class)，(T的当前类)
public synchronized static void add() {}
```

- **修饰代码块**： 指定加锁对象，对给定对象加锁，进入同步代码块之前要获取给定对象的锁

# 反射

## 什么是反射？

反射主要是指程序可以访问、检测和修改它本身状态或行为的一种能力；

Java反射：

在Java运行时环境中，对于任意一个类，能否知道这个类有哪些属性和方法？对于任意一个对象，能否调用它的任意一个方法

Java反射机制主要提供了以下功能：

- 在运行时判断任意一个对象所属的类。
- 在运行时构造任意一个类的对象。
- 在运行时判断任意一个类所具有的成员变量和方法。
- 在运行时调用任意一个对象的方法。 

## 什么是 java 序列化？什么情况下需要序列化？

简单说就是为了保存在内存中的各种对象的状态（也就是实例变量，不是方法），并且可以把保存的对象状态再读出来。虽然你可以用你自己的各种各样的方法来保存object states，但是Java给你提供一种应该比你自己好的保存对象状态的机制，那就是序列化。

什么情况下需要序列化：

a）当你想把内存中的对象状态保存到一个文件中或者数据库中时候；
b）当你想用套接字在网络上传送对象的时候；
c）当你想通过RMI传输对象的时候；

## 动态代理是什么？有哪些应用？

动态代理：

当想要给实现了某个接口的类中的方法，加一些额外的处理。比如说加日志，加事务等。可以给这个类创建一个代理，故名思议就是创建一个新的类，这个类不仅包含原来类方法的功能，而且还在原来的基础上添加了额外处理的新类。这个代理类并不是定义好的，是动态生成的。具有解耦意义，灵活，扩展性强。

动态代理的应用：

- Spring的AOP
- 加事务
- 加权限
- 加日志

## 怎么实现动态代理？

首先必须定义一个接口，还要有一个InvocationHandler(将实现接口的类的对象传递给它)处理类。再有一个工具类Proxy(习惯性将其称为代理类，因为调用他的newInstance()可以产生代理对象,其实他只是一个产生代理对象的工具类）。利用到InvocationHandler，拼接代理类源码，将其编译生成代理类的二进制码，利用加载器加载，并将其实例化产生代理对象，最后返回。

# 对象拷贝

## 为什么要使用克隆？

想对一个对象进行处理，又想保留原有的数据进行接下来的操作，就需要克隆了，Java语言中克隆针对的是类的实例。

## 深拷贝和浅拷贝区别是什么？

- 浅拷贝只是复制了对象的引用地址，两个对象指向同一个内存地址，所以修改其中任意的值，另一个值都会随之变化，这就是浅拷贝（例：assign()）
- 深拷贝是将对象及值复制过来，两个对象修改其中任意的值另一个值不会改变，这就是深拷贝（例：JSON.parse()和JSON.stringify()，但是此方法无法复制函数类型）

# Java Web

## jsp和Servlet有什么区别？

- jsp经编译后就变成了Servlet。（JSP的本质就是Servlet，JVM只能识别java的类，不能识别JSP的代码，Web容器将JSP的代码编译成JVM能够识别的java类）
- jsp更擅长表现于页面显示，Servlet更擅长于逻辑控制。
- Servlet中没有内置对象，Jsp中的内置对象都是必须通过HttpServletRequest对象，HttpServletResponse对象以及HttpServlet对象得到。
- Jsp是Servlet的一种简化，使用Jsp只需要完成程序员需要输出到客户端的内容，Jsp中的Java脚本如何镶嵌到一个类中，由Jsp容器完成。而Servlet则是个完整的Java类，这个类的Service方法用于生成对客户端的响应。

## jsp 九大内置对象

- request：封装客户端的请求，其中包含来自GET或POST请求的参数；
- response：封装服务器对客户端的响应；
- pageContext：通过该对象可以获取其他对象；
- session：封装用户会话的对象；
- application：封装服务器运行环境的对象；
- out：输出服务器响应的输出流对象；
- config：Web应用的配置对象；
-  page：JSP页面本身（相当于Java程序中的this）；
- exception：封装页面抛出异常的对象。

## jsp的四种作用域

JSP中的四种作用域包括page、request、session和application，具体来说：

- **page**：代表与一个页面相关的对象和属性。
- **request**：代表与Web客户机发出的一个请求相关的对象和属性。一个请求可能跨越多个页面，涉及多个Web组件；需要在页面显示的临时数据可以置于此作用域。
- **session**：代表与某个用户与服务器建立的一次会话相关的对象和属性。跟某个用户相关的数据应该放在用户自己的session中。
- **application**：代表与整个Web应用程序相关的对象和属性，它实质上是跨越整个Web应用程序，包括多个页面、请求和会话的一个全局作用域。

## session和cookie的区别

- 由于HTTP协议是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识别具体的用户，这个机制就是Session.

  ​	典型的场景比如购物车，当你点击下单按钮时，由于HTTP协议无状态，所以并不知道是哪个用户操作的，所以服务端要为特定的用户创建了特定的Session，用于标识这个用户，并且跟踪用户，这样才知道购物车里面有几本书。

  ​	这个Session是保存在服务端的，有一个唯一标识。在服务端保存Session的方法很多，内存、数据库、文件都有。集群的时候也要考虑Session的转移，在大型的网站，一般会有专门的Session服务器集群，用来保存用户会话，这个时候 Session 信息都是放在内存的，使用一些缓存服务比如Memcached之类的来放Session。

- 思考一下服务端如何识别特定的客户？这个时候Cookie就登场了。

  ​	每次HTTP请求的时候，客户端都会发送相应的Cookie信息到服务端。实际上大多数的应用都是用 Cookie 来实现Session跟踪的，第一次创建Session的时候，服务端会在HTTP协议中告诉客户端，需要在 Cookie 里面记录一个Session ID，以后每次请求把这个会话ID发送到服务器，我就知道你是谁了。有人问，如果客户端的浏览器禁用了 Cookie 怎么办？一般这种情况下，会使用一种叫做URL重写的技术来进行会话跟踪，即每次HTTP交互，URL后面都会被附加上一个诸如 sid=xxxxx 这样的参数，服务端据此来识别用户。

- Cookie其实还可以用在一些方便用户的场景下，设想你某次登陆过一个网站，下次登录的时候不想再次输入账号了，怎么办？这个信息可以写到Cookie里面，访问网站的时候，网站页面的脚本可以读取这个信息，就自动帮你把用户名给填了，能够方便一下用户。这也是Cookie名称的由来，给用户的一点甜头。

总结一下：Session是在服务端保存的一个数据结构，用来跟踪用户的状态，这个数据可以保存在集群、数据库、文件中；Cookie是客户端（浏览器）保存用户信息的一种机制，用来记录用户的一些信息，也是实现Session的一种方式。

## session的工作原理

session是一个存在服务器上的类似于一个散列表格的文件。里面存有我们需要的信息，在我们需要用的时候可以从里面取出来。类似于一个大号的map吧，里面的键存储的是用户的sessionid，用户向服务器发送请求的时候会带上这个sessionid。这时就可以从中取出对应的值了。

## 如果客户端禁用cookie，session还能用吗？

Cookie与 Session，一般认为是两个独立的东西，Session采用的是在服务器端保持状态的方案，而Cookie采用的是在客户端保持状态的方案。但为什么禁用Cookie就不能得到Session呢？因为Session是用Session ID来确定当前对话所对应的服务器Session，而Session ID是通过Cookie来传递的，禁用Cookie相当于失去了Session ID，也就得不到Session了。

假定用户关闭Cookie的情况下使用Session，其实现途径有以下几种：

- 设置php.ini配置文件中的“session.use_trans_sid = 1”，或者编译时打开打开了“--enable-trans-sid”选项，让PHP自动跨页传递Session ID。
- 手动通过URL传值、隐藏表单传递Session ID。
- 用文件、数据库等形式保存Session ID，在跨页过程中手动调用。

## SpringMVC和structs2的区别

- **拦截机制的不同**

Struts2是类级别的拦截，每次请求就会创建一个Action，和Spring整合时Struts2的ActionBean注入作用域是原型模式prototype，然后通过setter，getter把request数据注入到属性。Struts2中，一个Action对应一个request，response上下文，在接收参数时，可以通过属性接收，这说明属性参数是让多个方法共享的。Struts2中Action的一个方法可以对应一个url，而其类属性却被所有方法共享，这也就无法用注解或其他方式标识其所属方法了，只能设计为多例。

SpringMVC是方法级别的拦截，一个方法对应一个Request上下文，所以方法直接基本上是独立的，独享request，response数据。而每个方法同时又和一个url对应，参数的传递是直接注入到方法中的，是方法所独有的。处理结果通过ModeMap返回给框架。在Spring整合时，SpringMVC的Controller Bean默认单例模式Singleton，所以默认对所有的请求，只会创建一个Controller，又因为没有共享的属性，所以是线程安全的，如果要改变默认的作用域，需要添加@Scope注解修改。

Struts2有自己的拦截Interceptor机制，SpringMVC这是用的是独立的Aop方式，这样导致Struts2的配置文件量还是比SpringMVC大。

- **底层框架的不同**

Struts2采用Filter（StrutsPrepareAndExecuteFilter）实现，SpringMVC（DispatcherServlet）则采用Servlet实现。Filter在容器启动之后即初始化；服务停止以后坠毁，晚于Servlet。Servlet在是在调用时初始化，先于Filter调用，服务停止后销毁。

- **性能方面**

Struts2是类级别的拦截，每次请求对应实例一个新的Action，需要加载所有的属性值注入，SpringMVC实现了零配置，由于SpringMVC基于方法的拦截，有加载一次单例模式bean注入。所以，SpringMVC开发效率和性能高于Struts2。

- **配置方面**　

spring MVC和Spring是无缝的。从这个项目的管理和安全上也比Struts2高。

## 如何避免sql注入

- PreparedStatement（简单又有效的方法）
- 使用正则表达式过滤传入的参数
- 字符串过滤
- JSP中调用该函数检查是否包函非法字符
- JSP页面判断代码

## 什么是 XSS 攻击，如何避免？

XSS攻击又称CSS,全称Cross Site Script（跨站脚本攻击），其原理是攻击者向有XSS漏洞的网站中输入恶意的 HTML 代码，当用户浏览该网站时，这段 HTML 代码会自动执行，从而达到攻击的目的。XSS 攻击类似于 SQL 注入攻击，SQL注入攻击中以SQL语句作为用户输入，从而达到查询/修改/删除数据的目的，而在XSS攻击中，通过插入恶意脚本，实现对用户浏览器的控制，获取用户的一些信息。 XSS是 Web 程序中常见的漏洞，XSS 属于被动式且用于客户端的攻击方式。

XSS防范的总体思路是：对输入(和URL参数)进行过滤，对输出进行编码。

## 什么是 CSRF 攻击，如何避免？

CSRF（Cross-site request forgery）也被称为 one-click attack或者 session riding，中文全称是叫**跨站请求伪造**。一般来说，攻击者通过伪造用户的浏览器的请求，向一个用户曾经访问认证过的网站发送出去，使目标网站接收并误以为是用户的真实操作而去执行命令。常用于盗取账号、转账、发送虚假消息等。攻击者利用网站对请求的验证漏洞而实现这样的攻击行为，网站能够确认请求来源于用户的浏览器，却不能验证请求是否源于用户的真实意愿下的操作行为。

**如何避免**：

1. 验证 HTTP Referer 字段

HTTP头中的Referer字段记录了该 HTTP 请求的来源地址。在通常情况下，访问一个安全受限页面的请求来自于同一个网站，而如果黑客要对其实施 CSRF攻击，他一般只能在他自己的网站构造请求。因此，可以通过验证Referer值来防御CSRF 攻击。

2. 使用验证码

关键操作页面加上验证码，后台收到请求后通过判断验证码可以防御CSRF。但这种方法对用户不太友好。

3. 在请求地址中添加token并验证

CSRF 攻击之所以能够成功，是因为黑客可以完全伪造用户的请求，该请求中所有的用户验证信息都是存在于cookie中，因此黑客可以在不知道这些验证信息的情况下直接利用用户自己的cookie 来通过安全验证。要抵御 CSRF，关键在于在请求中放入黑客所不能伪造的信息，并且该信息不存在于 cookie 之中。可以在 HTTP 请求中以参数的形式加入一个随机产生的 token，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有token或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求。这种方法要比检查 Referer 要安全一些，token 可以在用户登陆后产生并放于session之中，然后在每次请求时把token 从 session 中拿出，与请求中的 token 进行比对，但这种方法的难点在于如何把 token 以参数的形式加入请求。
对于 GET 请求，token 将附在请求地址之后，这样 URL 就变成 http://url?csrftoken=tokenvalue。
而对于 POST 请求来说，要在 form 的最后加上 <input type="hidden" name="csrftoken" value="tokenvalue"/>，这样就把token以参数的形式加入请求了。

4. 在HTTP 头中自定义属性并验证

这种方法也是使用 token 并进行验证，和上一种方法不同的是，这里并不是把 token 以参数的形式置于 HTTP 请求之中，而是把它放到 HTTP 头中自定义的属性里。通过 XMLHttpRequest 这个类，可以一次性给所有该类请求加上 csrftoken 这个 HTTP 头属性，并把 token 值放入其中。这样解决了上种方法在请求中加入 token 的不便，同时，通过 XMLHttpRequest 请求的地址不会被记录到浏览器的地址栏，也不用担心 token 会透过 Referer 泄露到其他网站中去。

# 异常模块

## throw 和 throws 的区别？

- throws是用来声明一个方法可能抛出的所有异常信息，throws是将异常声明但是不处理，而是将异常往上传，谁调用我就交给谁处理；
- throw则是指抛出的一个具体的异常类型。

## final、finally、finalize 有什么区别？

- final可以修饰类、变量、方法，修饰类表示该类不能被继承、修饰方法表示该方法不能被重写、修饰变量表示该变量是一个常量不能被重新赋值。
- finally一般作用在try-catch代码块中，在处理异常的时候，通常我们将一定要执行的代码方法finally代码块中，表示不管是否出现异常，该代码块都会执行，一般用来存放一些关闭资源的代码。
- finalize是一个方法，属于Object类的一个方法，而Object类是所有类的父类，该方法一般由垃圾回收器来调用，当我们调用System的gc()方法的时候，由垃圾回收器调用finalize(),回收垃圾。 

**76. try-catch-finally 中哪个部分可以省略？**

答：catch 可以省略

**原因：**

更为严格的说法其实是：try只适合处理运行时异常，try+catch适合处理运行时异常+普通异常。也就是说，如果你只用try去处理普通异常却不加以catch处理，编译是通不过的，因为编译器硬性规定，普通异常如果选择捕获，则必须用catch显示声明以便进一步处理。而运行时异常在编译时没有如此规定，所以catch可以省略，你加上catch编译器也觉得无可厚非。

理论上，编译器看任何代码都不顺眼，都觉得可能有潜在的问题，所以你即使对所有代码加上try，代码在运行期时也只不过是在正常运行的基础上加一层皮。但是你一旦对一段代码加上try，就等于显示地承诺编译器，对这段代码可能抛出的异常进行捕获而非向上抛出处理。如果是普通异常，编译器要求必须用catch捕获以便进一步处理；如果运行时异常，捕获然后丢弃并且+finally扫尾处理，或者加上catch捕获以便进一步处理。

至于加上finally，则是在不管有没捕获异常，都要进行的“扫尾”处理。