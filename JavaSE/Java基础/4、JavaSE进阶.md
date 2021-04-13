# JavaSE进阶 参考[老杜JavaSE进阶 p107开始](https://www.bilibili.com/video/BV1mE411x7Wt?p=128)

# 常用类

## 一、Arrays工具类

**java.util.Arrays**

Arrays是一个工具类

其中有一个sort()方法，可以排序。**静态方法，直接使用类名调用就行。**

主要使用排序和二分查找

## 二、String类

### 1、String类

字符串代表常量，值在创建后不可更改。具体内容可看JVM中StringTable相关内容。

String表示字符串类型，属于引用数据类型，不属于基本数据类型。

在java中使用双引号括起来的都是String对象。

### 2、常用构造方法

```java
String s = new String("");
String s = "";
String s = new String(char[] c);
String s = new String(byte[] bytes, int offset, int length)//起始下标、长度
```



### 3、equals()

```java
String str1 = "ab";
String str2 = "ab";
str1 == str2 //true
    
String str3 = new String("ab");
String str4 = new String("ab");
str1 == str2 //false
```

通过上面的案例说明，字符串对象比较不能用”==“。需要使用equals()，**但是Object类中的equals()方法如下：**

```java
public boolean equals(Object obj) {
        return (this == obj);
}
```

所以**String重写了equals方法：**

```java
    public boolean equals(Object anObject) {
        if (this == anObject) {
            return true;
        }
        if (anObject instanceof String) {
            String aString = (String)anObject;
            if (!COMPACT_STRINGS || this.coder == aString.coder) {
                return StringLatin1.equals(value, aString.value);
            }
        }
        return false;
    }
```



```java
String k = "abc";
System.out.println("test".equals(k));
System.out.println(k.equals("test"));
```

为什么可以在“”后面加 . ?

因为“ ”中的内容是一个字符串对象，只要是对象都能调用方法。建议使用第一种，因为可以避免k为null，出现异常。

### 4、toString()

输出一个引用的时候，会自动调用toString()方法，默认Object的话，会自动输出对象的内存地址

String重写了toString方法，输出字符串值。

### 5、charAt()

范围0 - length() - 1

### 6、compareTo()

字符串之间不能直接比较大小，必须使用compareTo()

返回int，等于对应0，前小后大对应-1， 前大后小对应1 

**equals()只能看相不相等，底层使用byte数组逐个比较值。**

**compareTo()除了判断相等，还可以判断大小，底层使用 - 运算。**

compareTo(String)方法中其实是从头开始，一个一个字符地比对原字符串和参数字符串中的字符，如果相同就下一个，直到字符出现不同(包括某一个字符串结束了)，就计算这两个不同字符的ASCII码的差作为返回值。有两种比较方式：

- 不同的字符在较短字符串长度之内时

  返回值=原字符串与参数字符串中第一个不同字符相差的ASCII码值，为原减参。

  ```java
  String str1="javasdrip";
  String str2="javdscript";
  str1.compareTo(str2);
  ```

  此时返回值为-3，是a的ASCII码（97）减去了d的ASCII码值（100）得到。

  注意：只比较第一个不同的字符，后面的d和c也不一样但不会进行比较了。

- 不同的字符在较短字符串长度之外时

  返回值=原字符串与参数字符串相差的字符个数，原字符串长度大时为正，反之为负。

  ```java
  String str1="java";
  String str2="javascript";
  str1.compareTo(str2);
  ```

  此时返回值为-6，是str1相比str2少去的字符个数。

  注意：此时只比较位数，而无关ASCII码值，并非是0的ASCII码值减去s的ASCII码值，在参数字符串前面字符和原字符串一样时，返回值就是两者相差的字符个数，即使改变后面的字符也不会影响到返回的值，比如String str2="java123$%^"，此时结果仍是-6。

### 7、contains()

判断前面的字符串是否包含后面的子字符串

```java
System.out.println("test".contains("est"));//true
```

### 8、endsWith()

判断当前字符串是否是以某个字符串结尾

```java
System.out.println("test".endsWith("est"));//true
```

### 9、equalsIgnoreCase()

忽略大小写比较字符串

```java
System.out.println("test".equalsIgnoreCase("Test"));/true
```

### 10、getBytes()

将字符串对象转换成字节数组

```java
byte[] bytes = "absda".getBytes();
```

### 11、indexOf(String str)

判断某个字符串在当前字符串第一次出现的索引

```java
System.out.println("test".indexOf("est"));//1
```

### 12、isEmpty()

判断某个字符串是否为空字符串，底层调用value.length(), value是byte数组

**面试题：**判断数组长度和字符串长度不一样，数组长度是属性，字符串长度是方法。

### 13、lastIndexOf(String str)

判断某个字符串在当前字符串最后一次出现的索引

### 14、substring()

截取字符串，左闭右开

### 15、toCharArray()

将字符串转换为char数组

### 15、toLowerCase() 、toLowerCase()

将字符串转换为小写或大写

### 16、trim()

去除字符串前后空白

### 17、valueOf(Object)

String中只有这个方法是静态方法

把非字符串转换为字符串，调Object的toString()方法

**同理，用println()输出时，会调用toString方法**

### 18、split(String regex)

这里需要注意的是，如果正则表达式是**正则表达式元字符**（.$|()[{^?*+\），需要进行转义。

比如需要以.作为分隔符，那么regex需要写成 `\\.` 因为 \ 在字符串和正则表达式中都有同样的功能：

在字符串中\的功能：

- 代表特殊字符：\t代表制表符，\n代表换行....等。
- 代表转义，在字符串中，“\”后面的字符会当做普通字符处理

在正则表达式中\的功能：

- 代表特殊功能的字符：如\d代表数组
- 代表转义，和上面一样，当出现字符歧义时，加上\表示普通字符。

所以split传入字符串时，第一个\代表后面的\是普通字符，然后第二个 \ 代表正则表达式中 . 是普通字符

## 三、StringBuffer和StringBuilder

StringBuffer构造方法会创建一个byte数组，大小为16字节。往StringBuffer放字符串，实际是放在byte数组当中了。

拼接字符串使用append()方法，如果byte数组满了，会自动扩容。

**String和StringBuffer，有什么区别？**

> String创建的字符串不可变，StringBuffer创建的字符串可变。
>
> 我看过源代码，String中有byte[]，是被final修饰的，不可变。因为数组一旦创建，长度不可变，并且被final修饰的引用一旦指向某个对象之后，不可再指向其他对象，所以String不可变。
>
> StringBuilder和StringBuffer内部实际上是byte[]数组，这个byte[]数组没有final修饰。初始化容量我记得应该是16，当存满之后会进行扩容，底层会调用数组拷贝的方法，System.arraycopy()，所以这两者适合使用字符串的频繁拼接操作。

**字符串不可变是什么意思?**

> 是字符串对象本身不可变，会存在串池当中，字符串对象的引用可以指向其他对象。

**如何优化StringBuffer的性能？**

> 在创建StringBuffer的时候尽可能给定一个初始化容量，最好减少底层数组的扩容次数，预估记一下，给一个大一些的初始化容量。

**StringBuffer和StringBuilder区别？**

> StringBuffer中方法有synchronized关键字修饰，线程安全。StringBuilder没有，线程不安全。

## 四、8种包装类

8中数据类型有8中包装类，属于引用数据类型，父类是Object

8中包装类中6个都是数字对应的包装类，父类为Number，剩下两个是Object

举例Integer

构造方法有两个一个是**Integer(int)**一个是**Integer(String)**

```java
Integer a = 128;
Integer b = 128;
Integer c = 127;
Integer d = 127;

a == b//false
c == d//true
```

**原因：**java中为了提到程序执行效率，将-128到127之间所有的包装对象提前创建好放在了常量池当中。不需要再new了，直接从常量池取。

**常见方法：**

Integer.parseInt(String str)

静态方法，传参String，返回int

**String int Integer转换：**

1、String -> int : Integer.parseInt("123");

2、int -> String: 数字+ ""  || String.valueOf(int);

3、int -> Integer 自动装箱||Intger.valueOf(100);

4、Integer -> int 自动拆箱||intValue();

5、String -> Integer : Integer.valueOf("123");

6、Integer -> String: String.valueOf(Integer对象)

## 五、enum枚举

枚举编译后也是生成class文件，枚举也是一种引用数据类型，枚举中每一个值都可以看做是常量。

枚举定义：

```java
enum 枚举类型名{
	枚举值1, 枚举值2
}
```

返回值两种情况建议使用布尔类型，多种情况且可以列举使用枚举类型。

## 异常

## 一、异常

异常在Java中以类的形式存在，每一个异常类都可以创建异常对象。

**继承关系：**

Object下面有可抛出类，可抛出类Throwable有两个子类：Error和Exception。不管是错误还是异常，都是可抛出的。所有的错误只要发生,Java程序只有一个结果，就是终止程序的执行，错误是不能处理的。

Exception又分为运行时异常和编译时异常

RuntimeException：运行时异常（可以预先处理也可以不管）

Exception的直接子类：编译时异常（必须预先处理，否则编译器报错）

**编译时异常和运行时异常都发生在运行阶段，因为所有异常都需要new对象出来。**

### 1、异常处理方式

第一种方式：在**方法声明的位置上**，使用**throws**关键字。谁调用此方法，就抛给谁。**throw**是在代码中**手动抛出异常**

第二种方式：使用try...catch语句进行异常的捕捉。自己处理，外部不知道。

Java中如果异常发生后一直向上抛，抛给main方法，最终会抛给JVM，JVM会终止程序执行。

只要异常没有捕捉，采用上报的方式，此方法的后续代码不会执行。try语句中如果有一行出现异常，该行后面的代码不会执行。try...catch捕捉异常后，后续代码可以执行。

> - catch后面的小括号中的类型可以使具体的异常类型，也可以是该异常类型的父类型。
> - catch可以写多个。建议catch的时候，精确地一个一个处理。这样有利于程序调试。
> - catch写多个的时候，必须遵循从上到下，从小到大。

### 2、异常对象的常用方法

**exception.getMessage()**：获取异常简单的描述信息

**exception.printStackTrace()**：打印异常追踪的堆栈信息。**实际开发中，建议使用这个，养成好习惯**

怎么查看这个信息？

**异常追踪信息，从上往下一行一行看，但SUN写的代码就不要看了，主要的问题是出现在自己编写的代码上。**

### 3、finally相关

1. 在finally子句中的代码是最后执行的，并且是一定会执行的，即使try语句块中的代码出现了异常(除非退出Java虚拟机System.exit(0);)
2. finally子句必须和try一起出现，不能单独编写
3. finally语句通常使用在**释放资源**的情况下

执行顺序：try.../catch... -> finally  -> return

**面试题：**

```java
public static int m(){
    int i = 0;
    try{
        return i;
    }finally{
        i++;
    }
}

//返回结果为0;
反编译后从字节码可以解释。
    首先i=0，将栈帧中的局部变量槽中的一个变量设置为0，当进入try块，加载到操作数栈上，但这是需要先执行finally语句块，所以操作数栈又将当前的i，即0，存入另一个局部变量槽，然后再对之前的i变量槽自增。完成后，把刚才暂存的返回结果0 load到操作数栈上，进行返回。所以这种情况下虚拟机会先保存返回结果，再执行finally语句块的内容。
```

**注意：**finally中不要写return，这样子会吞掉异常，不会进行处理。

**面试题**：final finally finalize区别

final是一个关键字，表示最终不变的。类不能被继承，方法不能重写，变量不能修改。

finally也是一个关键字，和try联合使用，使用在异常处理中，finally中的代码块一定会执行。

finalize是Object一个方法，作为方法名出现。用在垃圾回收中，有一种引用方式是终结器引用，当这个对象不被其他对象引用时，会先将终结器引用放入引用队列中，根据该引用找到引用对象，之后调用finalize的方法，调用以后垃圾就被回收了。

### 4、自己定义异常

第一步：编写一个类继承Exception或者RuntimeException

第二步：提供两个构造方法，一个无参数，一个带有String参数。

### 5、常见异常

空指针异常：NullPointerException

类型转换异常：ClassCastException

数组下标越界异常：IndexOutOfBoundsException

数字格式化异常：NumberFormatException

### 6、throw和throws

**throws**

- 调用位置：方法名之后
- 作用：通知开发人员当前方法有可能抛出的异常
- 携带数据：throws后面携带异常类型，一个throws后面可以携带多个异常类型
- 调用：当一个方法被throws修饰时，调用方法时必须考虑异常捕捉问题

**throw**

- 声明位置：方法执行体
- 作用：throw是一个命令，执行时抛出一个指定异常对象
- 携带数据：throw后面携带异常对象，一个throw只能携带一个异常对象
- 调用：当一个方法内部存在throw命令时，在调用时可以不考虑异常捕捉问题

# 集合

## 一、集合概述

集合实际上是一个容器，可以来容纳其他类型的数据，本身是一个对象。

集合不能直接存储基本数据类型，也不能存储对象。存储的是引用数据类型。

**Java中集合分为两大类:**

一个是单个方式存储元素，超级父接口是java.util.Collection

一个是以键值对方式存储元素，超级父接口是java.util.Map

**所有常用实现类**：

- ArrayList
- LinkedList
- Vector
- HashSet
- TreeSet
- HashMap
- HashTable
- Properties
- TreeMap

**泛型： 泛型这种语法机制，只在程序编译阶段起作用，给编译器参考的，JVM运行阶段会去泛型化。**

**<这里面是标识符，随便写>**，一般用E或T

使用泛型的好处：

1. 使用泛型之后，集合中的数据更加统一了，不用泛型就是Object
2. 取出的元素不需要进行大量的“向下转型”

使用泛型的缺点：

1. 集合中的元素缺乏多样性。

## 二、Collection

List集合元素特点：有序可重复，存储的元素有下标。

Set集合元素特点：无序不可重复，存储的元素没有下标。

把集合变成线程安全的：Collections.synchronizedList(collectrion);

### 1、List

List常用实现类：ArrayList、LinkedList、Vector

#### ArrayList

ArrayList集合底层采用了数组的数据结构，并且是非线程安全的。

**ArrayList的toArray方法：**

> 有两种方法，第一种toArray不带参数，返回的是一个Object[]数组
>
> 第二种是带参数toArray(new String[0])，返回一个String类型的数组，**这里不能把ArrayList<Integer>转成int[]，不要用这个方法，可能是因为int和Integer不兼容的问题。**

**初始化：**ArrayList集合初始化容量是10，底层是Object类型的数组Object[]，**如果以无参构造方法构造ArrayList，会先默认构造一个空数组，当第一个元素添加进去的时候会扩容成10。**

**扩容机制：**通过数组扩容的方式去实现的。重新定义一个长度为10+10/2的数组也就是新增一个长度为15的数组（原容量1.5倍）。然后把原数组的数据，原封不动的复制到新数组中，这个时候再把指向原数的地址换到新数组。**优化**：数组扩容效率比较低，尽可能少的扩容，建议给定初始化容量。

数组优点：检索效率高

数组缺点：增删效率低，但向数组末尾添加元素效率还是很高的。数组无法存储大数据量，很难找大很大一块的连续内存空间。

**面试题：**

> 这么多集合中你用哪个最多？

ArrayList，因为往数组末尾添加元素不受影响，另外检索/查找某个元素操作比较多。

> 为什么检索效率高?

每个元素占用空间大小相同，内存地址连续，知道首元素内存地址就可以快速找到下标元素地址。

集合构造方法：可以把一个集合作为参数传入构造函数。

#### LinkedList

LinkedList集合底层采用了双向链表的数据结构，并且是非线程安全的。

链表优点：随机增删元素效率高，因为不涉及大量元素位移

数组缺点：查询效率低，每次查找，某个元素都需要从头结点开始往下遍历。

#### Vector

Vector集合底层采用了数组的数据结构，并且是线程安全的。

**初始化：**Vector集合初始化容量是10，底层是Object类型的数组Object[]。

**扩容机制：**通过数组扩容的方式去实现的。原容量2倍。



#### List接口特有常用方法

1. void add(int index, E element) 在列表指定位置添加元素，用的不多，因为对于ArrayList来说效率低
2. E get(int index) 根据下标获取元素
3. int indexOf(Object o) 获取指定对象第一次出现处的索引
4. int lastIndexOf(Object o) 获取指定对象最后一次出现处的索引
5. E remove(int index) 删除指定下标元素
6. E set(int index, E element) 修改指定位置的元素

### 2、Set

Set常用实现类：HashSet、SortedSet接口下的实现类TreeSet（SortedSet无序不可重复，但是可以自动排序）

#### HashSet

HashSet底层实际上new了一个HashMap集合，向HashSet集合中存储元素，实际上是存储到HashMap集合中了。HashMap集合是一个哈希表数据结构。

**默认容量：**默认16（初始化容量必须是2的倍数），加载因子0.75（当HashMap底层数组容量达到75%时，开始扩容，每次容量翻倍乘2）。hash运算的过程其实就是对目标元素的Key进行hashcode，再对Map的容量进行取模，而JDK 的工程师为了提升取模的效率，**使用位运算代替了取模运算**，这就要求Map的容量一定得是2的幂。



#### SortedSet（接口）

##### TreeSet

TreeSet底层实际上new了一个TreeMap集合，向TreeSet集合中存储元素，实际上是存储到TreeMap集合中了。TreeMap集合是一个二叉树数据结构。

**比较方式：**

放到TreeSet或者TreeMap集合key部分的元素想要做到排序，包括两种方式：

1. 放在集合中的元素实现Comparable接口
2. 在构造TreeSet或者TreeMap集合的时候给它传一个比较器对象。

**怎么选择？**

当比较规则不发生改变，只有一个，建议使用Comparable接口。

如果比较规则有多个，并且需要多个比较规则之间频繁切换，建议使用Comparator接口。Comparator接口的设计符合OCP原则。



### 3、Collection常用方法

1. boolean add(Object e) 向集合中添加元素

2. int size() 获取集合中元素个数

3. void clear() 清空集合

4. boolean contains(Object o) 判断当前集合中是否包含元素o

   contains方法底层比较的时候调用的是equals，所以不同String对象，如果和集合中String内容相同会返回true

   **结论：存放在集合中的类型一定要重写equals方法**

5. boolean remove(Object o) 删除集合中等某个元素

   remove方法会调用equals方法

6. boolean isEmpty()，判断集合是否为空

7. Object[] toArray() 把集合转化成数组

### 4、集合迭代器（重点）

两个方法：

```java
Iterator it = c.iterator()

boolean hasNext();
E next();

while(it.hasNext()){
	Object obj = it.next();
    System.out.println(obj);
}
```

迭代器对象it最初并没有指向第一个元素，hasNext()返回true表示还有元素可以迭代，false表示没有元素可以迭代，next()让迭代器前进一位并返回下一个元素。

**注意：**集合结构只要发生了改变，迭代器就必须重新获获取。比如在进行迭代的过程中，通过原集合remove了某个元素，之后没有重新获取迭代器，会报错。只能使用迭代器的remove方法，这样子不重新获取迭代器也不会报错。

**集合迭代方法**：foreach、迭代器，List类有下标还可以for循环

## 三、Map

- Map集合和Collection没关系。

- Map集合以key-value键值对的方式存储元素

- key-value都是存储Java对象的内存地址
- 所有Map集合的key的特点：无序不可重复。key存储特点和Set集合存储元素特点相同

#### 1、HashMap

HashMap底层是哈希表数据结构，是非线程安全的。**允许key和value为null**

**默认容量：**默认16（初始化容量必须是2的倍数），加载因子0.75（当HashMap底层数组容量达到75%时，开始扩容，每次容量翻倍乘2）。hash运算的过程其实就是对目标元素的Key进行hashcode，再对Map的容量进行取模，而JDK 的工程师为了提升取模的效率，**使用位运算代替了取模运算**，这就要求Map的容量一定得是2的幂。

1. HashMap集合底层是哈希表/散列表的数据结构

2. 哈希表是一个数组和单向链表的结合体.一维数组，数组中每个元素是一个单向链表。

3. 底层源代码

   ```java
   Node<K,V>[] table
   
   static class Node<K,V> implements Map.Entry<K,V> {
           final int hash; //哈希值，是key调用hashCode()方法的执行结果再通过哈希函数/算法进行了处理，方法是hashcode高十六位和第十六位进行异或运算，增加散列度，加一次扰动，让高位参加寻址运算。
       	//因为一般table.length-1得到的二进制数，实际有效位有限，主要集中在低16位，让高位参与
           final K key;
           V value;
           Node<K,V> next;
   }
   ```

4. **put工作原理**

   1. 先将k,v封装到Node对象当中
   2. 底层调用k的hashCode()方法得出hash值
   3. 通过哈希函数/算法转化为数组下标
   4. 下标位置上如果没有任何元素，就把Node添加到这个位置上。
   5. 如果下标对应下标有元素，产生冲突，把k和链表中的key进行equals判断，如果所有equals方法返回都是false，那么这个新节点将会被添加到链表末尾。如果其中有一个equals()返回true，那么这个节点的value将会被覆盖。

5. **get工作原理**

   1. 先调用k的hashCode()方法得出哈希值，通过哈希算法转换成数组下标，通过数组下标快速定位到某个位置上
   2. 如果这个位置上什么也没有，返回null。
   3. 如果有单向链表，会拿着k和单向链表上的每个节点中的key进行equals。如果所有equals方法返回false，那么get方法返回null，只要其中有一个节点的k和参数k，equals返回true，这个节点的value就是要找的value，get返回这个value。

6. HashMap集合的Key需要重写hashCode()和equals()方法

   如果一个类的equals方法重写，hashCode也必须重写

7. JDK8之后，如果哈希表单向链表中**元素个数超过8个并且数组长度超过64**，单向链表会变成红黑树的数据结构。当红黑树上的节点小于6时，会重新把红黑树变成单向链表。是为了提高检索效率。

#### 2、HashTable

HashMap底层是哈希表数据结构，是线程安全的。其中所有的方法都带有synchronized关键字，现在使用较少，因为控制线程安全有其他更好的方案。**不允许key和value为null**

**默认容量：**默认11，加载因子0.75。扩容是**原容量*2+1**



##### Properties

Properties被称为属性类。

Properties继承了HashTable，是线程安全的。另外Properties存储元素的时候也采用key-value的形式存储，并且key和value**只支持String类型**，不支持其他类型。

方法：**setProperty** **getProperty**



### SortedMap（接口）

SortedMap集合是无序不可重复，放在其中的元素会自动按照

#### 3、TreeMap

TreeMap底层的数据结构是二叉树

### Map常用方法

1. put(key, value);
2. get(Obejct key); 获取key对应value
3. void clear() 清空Map集合
4. boolean containsKey(Object key) 判断Map中是否包含某个key
5. boolean containsValue(Object value) 判断Map中是否包含某个value
6. boolean isEmpty()  判断Map集合中元素个数是否为0
7. Set<K> keySet() 获取Map集合所有的key（所有的键是一个集合）
8. V remove(Object key) 通过key删除键值对
9. int size() 获取Map集合中键值对的个数
10. Collection<V> values() 获取Map集合中所有的value，返回一个Collection
11. Set<Map.Entry<K, V>> entrySet()，将Map集合转换成Set集合。（Map集合通过entrySet这个方法转化成Set集合，Set集合中元素类型是Map.Entry<K, V>，Map.Entry<K, V>是一个静态内部类）

**Map的遍历：**通过keySet()获取Set集合，遍历key，通过key获取value，通过迭代器或者foreach循环遍历。



# IO流

I：Input

O：Output

通过IO可以完成对硬盘的读和写

## 一、分类方式

1. 按照流的方向：往内存中去，叫做**输入/读**。从内存中出来，叫做**输出/写**。

2. 按照读取数据方式：

   按照字节方式读取数据，一次读取1个byte，这种流是万能的，什么类型的文件都可以读取，称为**字节流**。

   按照字符方式读取数据，一次读取1个字符，这种流只能读取纯文本，不能读取图片、声音、视频等文件，称为**字符流**。能用记事本编辑的是普通文本文件。

Java中的IO流都已经写好了，程序员不需要关心。需要掌握Java中提供了哪些流，每个流的特点和常用方法有哪些。

Java中所有的流都在Java.io.*；下

Java中主要研究：**怎么new对象、调用流的那个方法是读，哪个方法是写**。



## 二、流的四大家族

java.io.InPutStream  字节输入流

java.io.OutPutStream 字节输出流

java.io.Reader 字符输入流

java.io.Writer 字符输出流

- 在java中只要类名以“Stream”结尾的都是字节流，以“Reader/Writer”结尾的都是字符流。

- 四大家族的首领都是抽象类。

- 这四个抽象类都实现了java.io.Closeable接口，都是可关闭的，都有close()方法。流毕竟是一个管道，是内存和硬盘之间的通道，用完之后一定要关闭，否则会耗费很多资源。
- 所有输出流都实现了java.io.Flushable接口，都是可刷新的，都有flush()方法。养成一个好习惯，输出流在最终输出后，一定要记得flush()，表示将管道当中剩余未输出的数据强行输出，清空管道。**如果没有flush()，可能会导致丢失数据**

java.io包下需要掌握的流有16个：

**文件专属**：

1. java.io.FileInputStream
2. java.io.FileOutputStream
3. java.io.FileReader
4. java.io.FileWriter

**转换流（将字节流转换为字符流）：**

1. java.io.InputStreamReader
2. java.io.InputStreamWriter

**缓冲流专属：**

1. java.io.BufferedReader
2. java.io.BufferedWriter
3. java.io.BufferedInputStream
4. java.io.BufferedOutputStream

**数据专属流：**

1. java.io.DataInputStream
2. java.io.DataOutputStream

**对象专属流：**

1. java.io.ObjectInputStream
2. java.io.ObjectOutputStream

**标准输出流：**

1. java.io.PrintWriter
2. java.io.PrintStream



### 1、FileInputStream(掌握)

- 文件字节输入流，万能的，任何类型的文件都可以采用这个流来读。

- 字节的方式，完成输入的操作，完成读的操作（硬盘--->内存）
- finally语句中确保流关闭，前提是流不是空

最终版代码:

```java
package test.io;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.IOException;

//int read(byte[] b)
//一次最多读取b.length个字节
public class FileInputStreamTest02 {
    public static void main(String[] args) {
        FileInputStream fis2 = null;
        try {
            //相对路径是从当前所在的位置为起点开始找
            //IDEA默认的当前路径是project的默认当前路径
            fis2 = new FileInputStream("tempfile");

            //开始读
            byte[] bytes = new byte[4];
            int readCount = 0; 
            while ((readCount = fis2.read(bytes)) != -1){//fis.read(byte[])返回的不是字节本身，是读到的字节数,如果read()无参，返回的是读到的字节本身。
                System.out.println(new String(bytes, 0, readCount));
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            if(fis2 != null){
                try {
                    fis2.close();
                } catch (IOException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}
```

常用方法：

1. read()
2. read(byte[])
3. available()返回剩下没有读到的字节数量，可以一次读这么多字节数量。
4. skip()跳过几个字节不读取

### 2、FileOutputStream(掌握)

```java
FileOutputStream fos = null;
fos = new FileOutputStream("file", true);//true表示追加，在文件末尾继续写，不加的话会删除原文件内容再写
byte[] bytes = {32,31,32,5};
//将byte数组全部写出
fos.write(bytes);
//将byte数组一部分写出
fos.write(bytes,0, 2);
fos.flush();
fos.close();
```

**文件拷贝：**

用FileInputStream读，用FileOutputStream写。注意对FileOutputStream用flush清空，最后finally关闭流的时候分开try，因为有可能一个出现异常导致另一个无法正常关闭。

用FileReader读，FileWriter写，只能拷贝普通文件

### 3、BufferedReader

带有缓冲区的字符输入流。使用这个流的时候不需要自定义char数组或byte数组。

BufferedReader构造方法需要传入一个流(Reader)，当一个流的构造方法中需要一个流的时候，这个被传进来的流叫做**节点流**。

外部负责包装这个的流，叫做**包装流**，还有一个名字：**处理流**。

FileReader就是一个节点流，BufferedReader就是包装流/处理流

对于包装流来说，只需要关闭最外层流就行，里面的节点流会自动关闭。(源代码)

```java
FileReader reader = new FileReader("file");
BufferedReader br = new BufferedReader(reader);
br.close();
```

或：

```java
FileInputStream in = new FileInputStream("file");
//将字节流转换为字符流
//in是节点流，reader是包装流
InputStreamReader reader = new InputStreamReader(in);//FileInputStream实现了InputStream这个接口
//reader是节点流，br是包装流
BufferedReader br = new BufferedReader(reader);//InputStreamReader实现了Reader这个接口
br.close();
```

合并：

```java
BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream("file")));
```



**读一行：**

```java
String firstLine = br.readLine();//读取内容不带换行符
System.out.println(firstLine);
```

**循环读：**

```java
String s = null;
while((s = br.readLine()) != null){
	System.out.println(s);
}
```



### 4、BufferedWriter

带有缓冲的字符输出流。

```java
FileWiter reader = new FileWriter("file");
BufferedWriter out = new BufferedWriter(reader);
out.flush();
out.close();
```

或：

```java
BufferedWriter out = new BufferedWriter(new OutputStreamWriter(new FileOutputStream("file")));
```

### 5、DataOutputStream

数据专属的流，这个流可以将数据连同数据类型一并写入这个文件。**注意：**这个文件不是普通文本文档，用记事本打不开

```java
DataOutputStream dos = new DataOutputStream(new FileOutputStream("data"))
dos.flush();
dos.close();
dos.WriteByte(i);
dos.WriteInt(i);
```

### 6、DataInputStream

读的顺序需要和写的顺序一致才可以读出来

### 7、ObjectOutputStream（序列化）

序列化：Java对象放在硬盘文件中

反序列化：硬盘文件恢复到内存中

```java
ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("students"))//构造方法传入OutputStream
oos.writeObject(s);
oos.flush();
oos.close();
```

参与序列化和反序列化的对象，必须事先serializable这个接口。通过源代码发现，这个接口只是一个标志接口。什么代码都没有。**起到标识的作用**，JVM看到这个类会自动生成一个序列化版本号。

**序列化版本号**：java语言中采用什么方法区分类？

> 第一：首先通过类名进行比对，如果类名不一样，肯定不是同一个类
>
> 第二：如果类名一样，再靠序列化版本号进行区分。即使类名相同，JVM也能区分。

**自动生成序列化版本号缺陷:**

> 一旦代码确定之后不能进行修改。因为一旦修改就要重新编译，生成全新的版本号，JVM认为这是一个全新的类。

**结论：**凡是一个类实现了serializable接口，建议给该类提供一个固定不变的序列化版本号。这样，以后这个类，即使代码修改了，但是版本号不变。建议将序列号手动写出来，不建议自动生成。**transient**：表示这个变量不参加序列化操作



### 8、ObjectInputStream（反序列化）

```java
ObjectInputStream ois = new ObjectInputStream(new FileInputStream("students"))//构造方法传入InputStream
Object obj = ois.readObject();
System.out.println(obj);//会调用obj的toString方法
ois.close();
```





### 9、PrintStream

标准的字节输出流，默认输出控制台。标准输出流不需要手动close()

改变标准输出流的输出方向：

```java
PrintStream printStream = new PrintStream(new FileOutputStream("log"));
//将输出方向修改到“log”文件
System.setOut(printStream);
System.out.println("dsada")//会输出到log文件中
```

## 三、java.io.File类

File类和四大家族没有关系，所以File不能完成文件的读和写

File对象代表文件和目录路径的抽象表示形式,可能对应目录或文件：

- C:\Drivers
- C:\Drivers\test.txt

### File类中常用方法

1. 构造方法

   ```java
   File f1 = new File("D:\\file")
   ```

2. exists()判断是否存在

3. createNewFile()，以文件形式新建

4. mkdir()，以目录形式新建，mkdirs()创建多重目录

5. getParent()，获取父路径

6. getParentFile()，获取父File

   getParentFile().getAbsolutePath()获取绝对路径

7. getName()获取文件名

8. isDirectory()是不是目录

9. isFile()是不是文件

10. length()获取文件大小

11. ListFiles()获取当前目录下所有子文件，返回一个File数组

## 四、IO和Properties联合使用

如果一个文件中数据是“key=value”这种形式，可以使用Properties对象方法load(Reader reader)加载文件到Properties集合中，这样再使用getProperty("key")就可以获取value了。数据库会用到。

**设计理念：**以后经常改变的数据，可以单独写到一个文件当中，使用程序动态获取。将来只需要修改文件的内容，java代码不需要改动，不需要重新编译，服务器也不需要重启，就可以动态的获取信息。

类似于以上机制的这种文件被称为**配置文件**，并且当配置文件中的格式是key=value的时候（最好不要有空格），这种配置文件是属性配置文件。Java规范中要求，属性配置文件建议以**.properties**结尾，但不是必须的。其中Properties对象专门存放属性配置文件的一个类。

属性配置文件中“#”注释，key如果重复，value自动覆盖。

# NIO (New IO)

## 一、NIO和IO区别

![image-20200909214406266.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/image-20200909214406266.png?raw=true)

NIO核心：**通道和缓冲区**

- **通道：**表示打开到IO设备（文件、套接字），负责传输

- **缓冲区：**一个用于特定基本数据类型的容器，由java.nio包定义，所有缓冲区都是Buffer抽象类的子类，负责存储。根据数据类型不同（boolean除外），提供了相应的缓冲区。

  ByteBuffer、CharBuffer、ShortBuffer、IntBuffer、LongBuffer、FloatBuffer、DoubleBuffer

  上述缓冲区的管理方式几乎一致，通过allocate()获取缓冲区

- 缓冲区存取数据的**两个核心方法**：

  put() : 存入数据到缓冲区

  get()：获取缓冲区中的数据

- 缓冲区中的**四个核心属性：**（0 <= mark <= position<= limit<=capacity）(mark初始值默认-1)

  - capacity：容量，表示缓冲区中最大存储数据的容量，一旦声明不能改变
  - limit：界限，表示缓冲区中可以操作数据的大小。（limit后面的数据不能进行读写）
  - position：位置，表示缓冲区中正在操作数据的位置
  - mark：标记，表示记录当前position，可以通过reset()恢复到mark位置

- flip()切换数据读取模式

## 二、直接缓冲区和非直接缓冲区

### 1、非直接缓冲区

通过allocate()方法分配缓冲区，将缓冲区建立在JVM内存中

### 2、直接缓冲区

通过allocateDirect()方法分配直接缓冲区，将缓冲区建立在物理内存中。可以提高效率



## 三、通道

通道：由java.nio.channels包定义，Channel表示IO源与目标打开的连接，Channel类似于传统的“流”。只不过Channel本身不能直接存储和访问数据，Channel只能与Buffer进行交互。

![image-20200910085403536.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/image-20200910085403536.png?raw=true)

通道的主要实现类

FileChannel、SocketChannel、ServerSocketChannel、DatagramChannel

获取通道：

Java针对支持通道的类提供了getChannel()方法

- 本地IO：

  FileInputStream/FileOutputStream

  RandomAccessFile

- 网络IO

  Socket、ServerSocket

  DatagramChannel

JDK1.7中的NIO2 针对各个通道提供了静态方法open()获取通道

JDK1.7中的NIO2的Files类提供了工具类的newByteChannel()获取通道 

### 1、分散（Scatter）与聚集（Gather）

分散读取：将通道中的数据按顺序分散到多个缓冲区

聚集写入：将多个缓冲区数据按顺序写入通道

### 2、字符集：CharSet

编码：字符串->字节数组

解码：字节数组->字符串

## 四、阻塞与非阻塞

![image-20200910114959351.png](https://github.com/Ellery-Lee/JavaNotes/blob/master/pictures/image-20200910114959351.png?raw=true)



# 反射机制

- 通过Java中的反射机制可以操作字节码文件。有点类似于黑客技术（可以读和修改字节码文件）

- java.lang.reflect.*包下

- 相关重要的类有哪些？

  > java.lang.Class：代表字节码文件，代表一个类型
  >
  > java.lang.reflect.Method：代表字节码中的字节码方法
  >
  > java.lang.reflect.Constructor：代表字节码中的构造方法字节码
  >
  > java.lang.reflect.Field：代表字节码中的属性字节码

  **反射机制，让代码具有通用性，可变化的内容都是写到配置文件当中，将来修改配置文件之后，创建的对象不一样了，调用的方法也不同了，但是java代码不需要做任何改动。这就是反射机制的魅力**

## 一、反射类Class

**获取Class的三种方式**

### 第一种

```java
Class c1 = Class.forName("java.lang.String")
```

- Class.forName()是一个静态方法

- 方法的参数是一个字符串
- 字符串需要的是一个完整类名
- 完整类名必须带有包名，java.lang也不能省略

### 第二种

```java
String S = "ABC"
s.getClass();
```

- Java中每个对象都有一个getClass方法

### 第三种

```java
Class z = String.class;
```

- Java中任何一种类型，包括基本数据类型，它都有.class属性

## 二、通过反射机制实例化对象

```java
Class c1 = Class.forName("java.lang.String")
Object obj = c.newInstance();
```

**newInstance()会调用User这个类的无参构造方法，完成对象的创建，必须保证无参构造方法存在**

**有什么用？**

Java代码写一遍，在不改变源代码的基础上，可以做到不同对象的实例化，非常灵活。符合**OCP开闭原则（对扩展开放，对修改关闭）**

后期学习高级框架，工作的时候也都是高级框架。包括：SSH、SSM、Spring



### 1、Class.forName()

静态代码块在类加载时执行，并且只执行一次。所以用静态代码块可以判断类是否加载了。

**经过测试Class.forName()完成了类加载。**如果只希望这个静态代码块执行，其他代码不执行，可以采用这种方法。

**JDBC技术还需要**

### 2、获取类文件下的绝对路径

相对路径可能改变，可移植性差。

使用以下通用方式获得文件绝对路径的前提是：文件必须在类路径下

**凡是在src下的类都是类路径下，src是类的根路径**

```java
String path = Thread.currentThread().getContextClassLoader().getResource("").getPath();
```

- Thread.currentThread() 线程对象
- .getContextClassLoader() 线程对象方法，可以获取到当前线程的类加载器对象
- .getResource("") 类加载器对象的方法，当前线程的类加载器默认从类的根路径下加载资源 

IO/Properties联合使用

```java
package test.io;

import java.io.InputStream;
import java.util.Properties;

public class IOPropertiesTest {
    public static void main(String[] args) throws Exception{
//        String path = Thread.currentThread().getContextClassLoader().getResource("classinfo.properties").getPath();
//        FileReader reader = new FileReader(path);
//        Properties pro = new Properties();
//        pro.load(reader);
//        reader.close();

        //或者以流的形式返回
        InputStream reader = Thread.currentThread().getContextClassLoader().getResourceAsStream("classinfo.properties");
        Properties pro = new Properties();
        pro.load(reader);

        //通过key获取value
        String classname = pro.getProperty("classname");
        System.out.println(classname);
    }
}

```

### 3、资源绑定器

```java
ResourceBundle bundle = ResourceBundle.getBundle("classinfo");
String classname = bundle.getString("classname");
System.out.println(classname);
```

- java.util包下提供了资源绑定器，便于获取属性配置文件中的内容，只能绑定.properties文件
- 使用这种方式的时候，属性配置文件xxx.properties必须放在类路径下。文件扩展名也必须是properties，并且在写路径的时候，路径后面的扩展名不能写。

## 三、类加载器

JDK中自带了3个类加载器

- Bootstrap ClassLoader（启动类加载器）
- Extension ClassLoader(拓展类加载器)
- Application ClassLoader(应用程序类加载器)

### 1、启动类加载器

代码开始执行前，会将所需要的类全部加载到JVM中。

**加载的类：**JAVA_HOME/jre/lib     是JDK最核心的类库

### 2、拓展类加载器

如果启动类加载器加载不到，回启动拓展类加载器加载。

**加载的类：**JAVA_HOME/jre/lib/ext   

### 3、应用程序类加载器

如果拓展类加载器加载不到，会启动应用程序类加载器。

**加载的类：**classpath中的jar包

### 4、双亲委派机制

Java中为了保证类加载安全，使用了双亲委派机制。

**优先从启动类加载器加载，这个称为“父”，“父”无法加载到，再从扩展类加载器中加载，这个称为“双亲委派”，如果都加载不到，才会考虑从应用程序类加载器加载，知道加载到为止。**



## 四、反射属性Field

```java
Class c1 = Class.forName("java.lang.String")
Filed[] fields = c.getFields();//获取类中所有public修饰的Field数组
Field f = fields[0];//取出Field

String fieldName = f.getName();//取出这个Field的名字

Class fieldType = f.getType();//获取Filed属性类型
String fName = fieldType.getName();//f.getType()返回Class，在调用fieldType.getName()获取类名字

int i = f.getModifiers();//获取Field属性修饰符列表，返回的是一个数字，每个数字代表修饰符代号。
String modifierString = Modifier.toString(i); //Modifier类静态方法可以把修饰符代号转化为修饰符字符串
```

- Filed[] fields = c.getFields(); **获取类中所有public修饰的Field数组**

- Filed[] fields = c.getDeclaredFields();**获取类中所有Field数组**

### 通过访问机制访问对象属性（*）

```java
Class c1 = Class.forName("***/student")
Object obj = studentClass.newInstance();

Filed numField = studentClass.getDeclaredField("num");//根据属性名称获取Field
numField.set(obj, 2222);//给obj对象（student对象）的num赋值
System.out.println(numField.get(obj));//获取obj对象的num属性的值
```

**虽然复杂，但灵活**

**访问私有属性打破封装，给不法分子留下机会**

```java
numField.setAccessible(true);//打破封装，这样设置完可以访问私有属性
numField.set(obj,"jackson");//可以给私有属性赋值
```

## 五、反射方法Method

- **可变长参数：语法：**类型... args(一定是三个点)

- 可变长度参数必须在参数列表中最后一个位置上，而且可变长度参数只能有一个
- 可变长度参数可以当做一个数组来看待

```java
public static void m(int... args){
	
}
```

```java
Class c1 = Class.forName("***/student")
Method[] methods = c1.getDeclaredMethods();//获取所有方法的方法数组

for(Method method : methods){
    method.getName();//获取方法名
    method.getReturnType().getSimpleName();//获取返回类型
    method.getModifiers();//获取修饰符列表
    Class[] args = method.getParameterTypes();//获取参数列表
}
    
Object obj = studentClass.newInstance();
```

### 通过反射机制调用对象方法（*）

Java中区分方法靠**方法名**和**参数**

调用方法要素分析：

- 对象
- 方法名
- 实参列表
- 返回值

```java
Class c1 = Class.forName("***/student")
Object obj = c1.newInstance();

Method loginMethod = c1.getDeclaredMethod("login", String.class, String.class);
Object retValue = loginMethod.invoke(obj, "admin", "123");
```

**Object retValue = loginMethod.invoke(obj, "admin", "123");  反射机制中最最最重要的方法，必须记住**

### 通过反射机制创建对象

```JAVA
Class c1 = Class.forName("***/student")
Object obj = c1.newInstance();

Object obj = c1.newInstance();//调用无参构造方法创建对象
//或者
Constructor con = c.getDeclaredConstructor();
Object ob = con.newInstance();

//调用有参构造？
Constructor con = c.getDeclaredConstructor(int.class, String.class, boolean.class);
Object ob = con.newInstance(110, "dsa", true);
```

### 获取父类或父接口

```java
Class c1 = Class.forName("***/student")
Class superclass = c1.getSuperClass();//获取父类
Class[] interfaces = c1.getInterfaces();//获取所有接口
```

# 注解

注解Annotation是一种引用数据类型，编译之后也是生成xxx.class文件

**怎么自定义注解？**

```
[修饰列表] @interface 注解类型名{


}
```

**注解使用：**

```
@注解类型名
```

注解可以出现在类上，属性上，方法上，变量上等

注解还可以出现在注解类型上

### JDK内置注解

java.lang包下的注解类型：

1. **Deprecated**：不鼓励程序员使用这样的元素，通常因为它很危险或存在更好的选择

2. **Override**：表示一个方法声明打算重写超类中的另一个方法的声明

   只能注解方法

   给编译器参考，和运行阶段没有关系

   凡是Java中的方法带有这个注解的，编译器都会检查，如果这个方法不是重写父类方法，编译器报错。

3. SupperessWarnings



**元注解**：用来标注注解的注解

常见的元注解：

**Target**：标注被标注的注解可以出现在哪些位置

**Retention**：标注被标注的注解最终保存在哪里



- 通常在注解中定义属性，看着像一个方法，实际上是属性name。

- 如果一个注解中有属性，必须给属性赋值，除非该属性使用**“default”**设定默认值
- 如果属性名是**value**，并且**只有一个属性**，可以省略属性名，直接写值
- 属性是数组，数组是大括号，如果数组中只有一个元素，可以省略



# Java跟C++区别？

-  Java是解释型语言，所谓的解释型语言，就是源码会先经过一次编译，成为中间码，中间码再被解释器解释成机器码。对于Java而言，中间码就是字节码(.class)，而解释器在JVM中内置了。

  C++是编译型语言，所谓编译型语言，就是源码一次编译，直接在编译的过程中链接了，形成了机器码。

- C++比Java执行速度快，但是Java可以利用JVM跨平台。

- Java是纯面向对象的语言，所有代码（包括函数、变量）都必须在类中定义。而C++中还有面向过程的东西，比如是全局变量和全局函数。

- C++中有指针，Java中没有，但是有引用。

- C++支持多继承，Java中类都是单继承的。但是继承都有传递性，同时Java中的接口是多继承，类对接口的实现也是多实现。

- C++中，开发需要自己去管理内存，但是Java中JVM有自己的GC机制，虽然有自己的GC机制，但是也会出现OOM和内存泄漏的问题。C++中有析构函数，Java中Object的finalize方法。