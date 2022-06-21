# Java集合

概念：对象的容器，实现了对对象常用的操作，类似数组。

与数组的区别：

- 数组长度固定，集合长度不固定
- 数组可以存储基本类型和引用类型，集合只能存储引用类型

![image-20201220234101322](/Users/yangxiansheng/笔记/images/image-20201220234101322.png)

![img](/Users/yangxiansheng/笔记/images/2243690-9cd9c896e0d512ed.png)

> 由上图可知，Java集合狂啊急主要包含两种类型的容器，集合（collection，存储单个元素）和图（map，存储键值对映射）。
>
> Collection接口又有3种子类型，List（链表）、Set（集合）和Queue（队列），再下面是一些抽象类和具体实现类，常用的有ArrayList、LinkedList、HashSet、LinkedHashSet、HashMap、LinkHashMap等

​	集合框架是一个用来代表和操纵集合的同一架构，所有的集合框架都包含以下内容：

- 接口：是代表集合的抽象数据类型。如Collection、List、Set、Map等，之所以定义多个接口，是为了以不同方式操作集合对象。
- 实现（类）：是集合接口的具体实现。它们是可重复使用的数据结构，如ArrayList、LinkedList、HashSet、HashMap。
- 算法：是实现集合接口的对象里的方法执行的一些有用的计算。如搜索和排序，这些算法被称为多态，因为相同的方法可以在相似的接口上有着不同的实现。

除了集合，该框架也集成了几个Map接口和类。尽管Map不是集合（存储键值对），但它们完全整合在集合框架中。

## 集合框架体系

![img](/Users/yangxiansheng/笔记/images/java-coll.png)

​	Java 集合框架提供了一套性能优良，使用方便的接口和类，java集合框架位于java.util包中， 所以当使用集合框架的时候需要进行导包。



## 集合接口

1. Collection接口：最基本的集合接口，一个Collection代表一组Object，即Collection集合内的元素，java不提供直接继承Collection的类，只提供继承于Collection接口的子接口（如List和Set），Collection接口存储一组不唯一，无序的对象
2. List接口：是一个有序的Collection。使用此接口能够精确的控制每个元素插入的位置，能够通过索引（元素在List中的位置，类似数组的下标，从0开始）来访问List中的元素，并且允许有相同的元素，即List接口存储一组不唯一，有序（插入顺序）的对象
3. Set接口：具有和Collection完全一样的接口，唯一一点与Collection不同就是Set不保存重复的元素，即Set 接口存储一组唯一，无序的对象
4. SortedSet接口：继承于Set接口，唯一、有序
5. Map接口：存储一组键值对象，提供Key和Value的映射
6. Map.Entry：描述一个Map 中的一个元素（键值对）。是一个Map的内部类
7. SortedMap：继承于Map，使得Key保持在升序序列

> Set和List的区别
>
> - Set接口实例存储的是无序的，不重复的元素。List接口实例存储的是有序的，可以重复的元素
> - Set检索效率低下，删除和插入效率高，插入和删除不会引起元素位置改变，实现类有**HashSet，TreeSet**

## 集合实现类（集合类）

​	java提供了一套实现了Collection接口的标准集合类。其中一些是具体类，可以直接拿来使用，另外一些是抽象类，提供了接口的部分实现。

1. **AbstractCollection**：实现了大部分的集合接口。

2. **AbstractList**：继承于AbstractCollection并且实现了大部分LIst接口

3. **AbstractSequentialList**：提供了对数据元素的链式访问而不是随机访问

4. **LinkedList**：该类实现了List接口，允许有null（空）元素，主要用于创建链表数据结构，该类没有同步方法，如果多个线程同时访问一个List，则必须自己实现同步，解决方法就是在创建List时构造一个同步的List。如

   ```java
   List list = Collection.synchronizedList(new LinkedLise(...))
   ```

5. **ArrayList**:该类也是实现了List的接口，实现了可变大小的数组，随机访问和遍历元素，提供更好的性能，该类是非同步的。ArrayList每次 扩容1.5倍，插入删除效率低

   1. 默认长度是10

6. **Vector**：同样继承AbstractList，与ArrayList非常相似，但是它是线程同步的，允许设置默认的长度，默认扩容方式扩容为原来的两倍

7. Stack：栈，Vector的一个子类，是一个标准的后进先出的栈

8. **AbstractSet**：继承于AbstractCollection并且实现了大部分Set接口

9. **HashSet**：该类实现了Set接口，不允许出现重复元素，不保证集合中元素的顺序，允许包含值为null的元素，但最多只能有一个，基于哈希表实现支持快速查找。

10. LinkedHashSet：具有HashSet的查找效率，且内部使用双向链表维护元素的插入顺序。

11. **TreeSet**：该类实现了Set接口，基于红黑树实现，支持有序性操作，例如根据一个范围查找元素的操作，但是查找效率不如HashSet，HashSet查找的时间复杂度为O（1），TreeSet则为O（logN）。

12. **AbstractMap**：实现了大部分的Map接口

13. **HashMap**：是一个散列表，它存储的内容是键值对（Key-Value）映射，该类实现了Map接口，根据键的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的键为 null，不支持线程同步

14. **TreeMap**：继承了AbstractMap，并且使用一棵树。

15. **WeakHashMap**：继承于HashMap，使用弱密钥的哈希表

16. **LInkedHashMap**：继承于HashMap，使用元素的自然顺序对元素进行排序

17. **IdentityHashMap**：继承AbstractMap类，比较文档时使用引用相等



## 集合算法

> 集合框架定义了集中算法，可用于集合和映射，这些算法被定义为集合类的静态方法。

一般情况下遍历一个集合有多种方法，for循环和增强for循环for-each，也可以将链表转换为数组（脱裤子放屁）进行for循环遍历，也可以采用迭代器遍历集合框架，如遍历ArrayList

```java
import java.util.*;
 
public class Test{
 public static void main(String[] args) {
     List<String> list=new ArrayList<String>();
     list.add("Hello");
     list.add("World");
     list.add("HAHAHAHA");
     //第一种遍历方法使用 For-Each 遍历 List
     for (String str : list) {            //也可以改写 for(int i=0;i<list.size();i++) 这种形式
        System.out.println(str);
     }
 
     //第二种遍历，把链表变为数组相关的内容进行遍历（脱裤子放屁）
     String[] strArray=new String[list.size()];
     list.toArray(strArray);
     for(int i=0;i<strArray.length;i++) //这里也可以改写为  for(String str:strArray) 这种形式
     {
        System.out.println(strArray[i]);
     }
     
    //第三种遍历 使用迭代器进行相关遍历
     
     Iterator<String> ite=list.iterator();
     while(ite.hasNext())//判断下一个元素之后有值
     {
         System.out.println(ite.next());
     }
 }
}
```

使用迭代器无需担心遍历过程中超出集合的长度，可以有效避免数组下标越界之类的异常。

### 遍历Map

```java
import java.util.*;
 
public class Test{
     public static void main(String[] args) {
      Map<String, String> map = new HashMap<String, String>();
      map.put("1", "value1");
      map.put("2", "value2");
      map.put("3", "value3");
      
      //第一种：普遍使用，二次取值
      System.out.println("通过Map.keySet遍历key和value：");
      for (String key : map.keySet()) {
       System.out.println("key= "+ key + " and value= " + map.get(key));
      }
      
      //第二种
      System.out.println("通过Map.entrySet使用iterator遍历key和value：");
      Iterator<Map.Entry<String, String>> it = map.entrySet().iterator();
      while (it.hasNext()) {
       Map.Entry<String, String> entry = it.next();
       System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
      }
      
      //第三种：推荐，尤其是容量大时
      System.out.println("通过Map.entrySet遍历key和value");
      for (Map.Entry<String, String> entry : map.entrySet()) {
       System.out.println("key= " + entry.getKey() + " and value= " + entry.getValue());
      }
    
      //第四种
      System.out.println("通过Map.values()遍历所有的value，但不能遍历key");
      for (String v : map.values()) {
       System.out.println("value= " + v);
      }
     }
}
```

### 给集合排序

第一种：将hashset转化为treeset

```java

```

collection接口方法补充：

iterator()：返回该集合的迭代器，该迭代器有三个方法：

- hasNext
- next
- remove

> 使用迭代器遍历集合时，只能使用迭代器的方法来修改集合元素，否则抛出并发修改异常。

List接口方法补充：

- subList(int formIndex,int toIndex):指定下标返回子集，左闭右开。
- listIterator()：返回listIterator列表迭代器，与普通迭代器不同的是它允许沿任意方向进行遍历，可以用列表迭代器来设置和添加元素、判断前后是否有元素以及获取下一个元素的索引值。

### 总结

- Java集合框架为程序员提供了预先包装的数据结构和算法来操纵它们。

- 集合是一个对象，可容纳其他对象的引用。集合接口声明对每一种类型的集合可以执行的操作。

- 任何对象加入集合类之后，自动转变为Object类型，所以在取出的时候需要进行强制类型转换。

  > 使用范型之后不用强制转换

### 补充笔记

#### ArrayList和LinkedList的区别

- ArrayList是List接口的一种实现，是使用数组实现的
- LinkedList也是List接口的一种实现，顾名思义是使用链表来实现的。
- ArrayList遍历和查找元素较快，添加、删除元素较慢
- LinkedList遍历、查找元素较慢，但添加、删除元素较快



### 关于map.entrySet()和keySet()

1. 遍历HashMap时entrySet()是将Key和Value全部取出来，而keySet（）方法进行遍历的时候是根据取出的key值去查询对应的value值，所以如果key值是比较简单的结构（如1，2，3。。。）的话，性能上的消耗比entrySet（）方法低，但随着key值的复杂度提高，entrySet（）的优势就会显露出来
2. 综合比较在只遍历key的时候使用keySet（），只遍历value时使用values（），在遍历key-value时使用entrySet是比较合理的选择
3. 如果遍历TreeMap的key-value时，务必要使用entrySet（），它要远远高于其他两个的性能，同样在只遍历key或value时和遍历HashMap的选择一致

### Java的快速失败和安全失败

#### 快速失败（fail-fast）

​	在使用迭代器遍历一个集合对象时，如果遍历过程中对集合对象的内容进行了修改（增删改），则会抛出Concurrent Modification Exception。

**原理：**迭代器在遍历时直接访问集合内的内容，并且在遍历过程中使用一个modCount变量，集合在被遍历期间如果内容发生变化，就会改变modCount的值，每当迭代器使用hasNext（）/next（）遍历下一个元素之前，都会检测modCount是否为expectmodCount的值，true则返回遍历，false则抛出异常，终止遍历。

**注意：**这里的异常抛出条件是modCount！=expectmodCount，若集合发生变化时修改了modCount时刚好又设置了expectmodCount的值相等，则异常不会抛出，因此，不能依赖于这个异常是否抛出而进行并发操作编程，这个异常只建议用于检测并发修改的bug。

**场景：**java.util包下的集合类都是快速失败的，不能在多线程下发生并发修改（迭代过程中被修改）。

#### 安全失败（fail-safe）

​	采用安全失败机制的集合容器，在遍历时并不是直接访问集合内的元素，而是将整个集合复制一份，在复制的集合上进行遍历。

**原理：**由于迭代器是对原集合的拷贝集合进行遍历，所以在遍历过程中对原集合所做的修改并不能被迭代器检测到，因此不会触发Concurrent Modification Exception异常。

**缺点：**基于复制的集合遍历优点是避免了上述异常，但同样的，迭代器不能访问到修改后的内容，遍历的是一开始时集合的拷贝。

场景：java.util.concurrent包下的容器都是安全失败，可以在多线程下并发使用，并发修改。

## ArrayList

​	ArrayList是一个可以动态修改的数组，与普通数组的区别就是它没有固定大小的限制，我们可以添加或删除元素。

> ArrayList继承了AbstractList，并实现了LIst接口。

![img](/Users/yangxiansheng/笔记/images/ArrayList-1-768x406-1.png)

ArrayList类位于java.util包中，使用前需引入

```java
import java.util.ArrayList; // 引入 ArrayList 类

ArrayList<E> objectName =new ArrayList<>();　 // 初始化
```

- E：泛型数据类型，用于设置objectName的数据类型，**只能为引用数据类型**

## LinkedList

- 链表，一种常见的基础数据结构，是一种线性表，但是并不会按照现行的顺序存储数据，而是在每个节点里存储下一个节点的地址。

- 链表可以分为单向链表和双向链表。
- 一个单向链表包含两个值，当前节点的值和指向下一个节点地址的值

![img](/Users/yangxiansheng/笔记/images/408px-Singly-linked-list.svg_.png)

- 一个双向链表有三个整数值，当前节点数值，向前的节点链接，向后的节点链接。

![img](/Users/yangxiansheng/笔记/images/610px-Doubly-linked-list.svg_.png)

- LinkedList类似于ArrayList，也是一种常用的数据容器，与ArrayLIst相比，LinkedList的增加和删除操作效率更高，而查找和修改的操作较低

**以下情况使用ArrayList：**

- 频繁访问列表中的某一个元素。
- 只需要在列表进行添加和删除元素操作。

**以下情况使用LinkedList：**

- 需要通过循环迭代来访问列表中的某些元素。
- 需要频繁的在列表开头、中间、末尾等位置进行添加和删除元素操作。

### LinkedList继承的接口和类

![img](/Users/yangxiansheng/笔记/images/20190328164737.png)

> - LinkedList继承了AbstractSequentialList类。
> - LinkedList实现了Queue接口，可作为队列使用。
> - LinkedList实现了List接口，可进行列表的相关操作。
> - LinkedList实现了Deque（双端队列）接口，可作为队列使用。
> - LinkedList实现了Cloneable接口，可实现克隆。
> - LinkedList实现了java.io.Serializable，即可支持序列化，能够通过序列化去传输。

LinkedList 类位于 java.util 包中，使用前需要引入它，语法格式如下：

```java
// 引入 LinkedList 类
import java.util.LinkedList; 

LinkedList<E> list = new LinkedList<E>();//普通创建方法
LinkedList<E> list = new LinkedList(Collection<? extends E> c);//使用集合创建链表

```

正常情况下，使用ArrayList访问列表中的随机元素更加高效，但是以下几种情况LinkedLIst提供了更高效的方法。

- 在列表的头添加元素:addFirst();
- 在列表结尾添加元素:addLast();
- 在列表开头移除元素:removeFirse();
- 在列表结尾移除元素:removeLast();
- 获取列表开头的元素:getFirst();
- 获取列表结尾的元素:getLast();

> 更多方法查看API文档

## HashMap

- 是一个散列表，存储的内容时键值对（key-value）的映射。
- 实现了Map接口，根据键的HashCode值存储数据，具有很快的访问速度，最多允许一条记录的键为null，不支持线程同步。
- 是无序的，即不会记录插入的顺序
- 继承于AbstractMap，实现了 Map、Cloneable、java.io.Serializable 接口。

![img](/Users/yangxiansheng/笔记/images/WV9wXLl.png)