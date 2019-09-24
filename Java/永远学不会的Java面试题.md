# Java基础

## JDK和JRE有什么区别？

- `JDK`：`Java Development Kit`的缩写，Java开发工具包，包含了JAVA的运行环境（`JVM`+Java系统类库）和JAVA工具。
- `JRE`：`Java Runtime Environment`的缩写，Java运行环境，包含JVM标准实现及Java核心类库。

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

