# Java核心技术基础

## 反射

​	能够分析类能力的程序被称为反射（reflective），反射可以用来：

- 在运行中分析类的能力
- 在运行中查看对象
- 实现通用的数组操作代码
- 利用Method对象，这个对象很像c++的函数指针

### 反射实战

#### 获取Class对象的四种方式

​	想要动态的获取类的信息，需要依靠Class对象。Class对象将一个类的方法、变量等信息告诉运行中的程序。Java提供了四种方式来获取Class对象：

1. 知道具体类的情况下使用

```java
Class clazz = TargetClass.class;
```

2. 通过Class.forname()传入类的全路径获取：

```java
Class clazz = Class.forname("com.alibaba.fastjson.json");
```

3. 通过对象实例instance.getClass()获取：

```java
Person person = new Person();
Class clazz = person.getClass();
```

4. 通过类加载器xxxClassLoader.loadClass()传入类路径获取：

```java
//此方式获取类对象不会进行初始化
Class clazz = ClassLoader.loadClass("com.alibaba.fastjson.json");
```









### Class类

​	在程序运行期间，Java运行时系统始终为所有的对象维护一个被称为运行时的类型标识。这个信息跟踪着每个对象所属的类。虚拟机利用运行时类型信息选择相应的方法执行。

​	然而，可以通过专门的类来保存这些信息，即Class类。Object::getClass()方法返回一个Class类型的实例。

​	一个Class对象表示一个特定类的属性，例如类的名称，可以通过Class::getName()方法来获取类的名称，或类在包中，包名也会作为类名的一部分，例如：java.lang.String，还可以调用静态方法Class::forName(类名或接口名)来加载类的实例

```java
Class class = Class.forName("java.lang.String");
```

#### 创建类的实例的方式：

1. new出一个对象；
2. 通过Class::forName(String className)来加载一个指定名称的类；
3. 通过Class::newInstance()方法来创建对象

```java
String s = "java.util.Date";
Object o = Class.forName(s).newInstance();
```

> 上述2、3点都是通过反射来创建对象，第三点使用默认的无参构造器来构造对象，若没有无参构造器则会报错

#### 反射分析类的结构

​	获取Class对象之后，可以通过该对象来返回类所提供的全部域、方法和构造器数组，包括私有的和受保护的成员，但不包括超类的成员

####  反射分析运行时对象

​	



## 代理

利用代理可以在运行时创建一个实现了一组给定接口的新类，称为代理类，这个类实现了指定的接口，并且有以下方法：

- 指定接口所需要的全部方法
- Object类的全部方法

但是不能再运行时定义这些新方法的代码。而是要提供一个调用处理器(invocation handler)。调用处理器是实现了InvocationHandler接口的具体类，该接口只有一个方法：

```java
Object invoke(Object proxy,Method method,Object[] args)
```

> 无论何时调用代理对象的方法，invocationhandler的invoke方法都会被调用，并向其传递Method对象和原始的调用参数。调用处理器必须给出处理调用的方式。







## 集合

### 集合框架

​	框架(framework)是一个类的集，它奠定了创建高级功能的基础。框架包含很多的超类，这些超类包含了很多非常有用的功能、策略和机制。框架使用者可以扩展这些超类的功能而不必重新创建这些基本的机制。

​	Java集合类库构成了集合类的框架。它为集合的实现者定义了大量的接口和抽象类，并且对其中某些机制进行描述，例如：迭代协议。

​	集合有两个基本的接口：Collection和Map，可以使用add方法向集合中插入元素，Map保存的是键值对，因此要可以使用put(K,V)方法来进行插入。

​	从集合中读取元素，可以使用迭代器或者get方法。

​	List是一个有序集合。元素可以添加到容器中特定的位置，将对象放置在某个位置上可以采用两种方式：使用整数索引或者列表迭代器。List接口定义了几个用于随机访问的方法：

> - void add(int index,E element)
>
> - E get(int index)
>
> - void remove(int index)
>
>   仅仅是定义，而不管实现是否高效，具体实现要看实现类的方式。

​	LIstIterator接口定义了以下数个方法：

> - void add(E element)：将一个元素添加到迭代器所处位置的前面。
> - E next()：获取下一个元素
> - E remove()：移除并返回next或previous方法返回的最后一个元素，在remove之前，必须先用next或previous方法获取一次元素

​	Set和Collection接口是一样的，只不过Set接口对方法的行为有更加严谨的定义。集的add方法拒绝重复的元素、equals方法定义两个集相同的条件是包含相同的元素而顺序不必相同、hashCode方法应该保证相同的元素经过hash运算之后得到相同的散列码。

​	SortedSet和SortedMap会提供用于排序的比较器对象，这两个接口定义了可以获得子视图的方法，后续讨论。

#### 集合的接口和实现分离

​	Java中集合类库将接口和实现进行分离，因此，在程序中使用集合时就无需知道究竟使用了那种实现（多态）而直接使用接口类型存放集合的引用，只有在构造集合对象时，使用具体的类才有意义。

```java
  Queue<Thread> threads = new LinkedList<>();
  threads.add(new Thread());
```

​	利用这种方式，一旦需求变更要使用另外一种实现类，则只需要更改构造器即可。	

​	接口本身不能说明那种实现的好坏，如队列，循环数组比链表更加高效，但元素数量若不确定或无上限，则链表要优于循环数组。

​	在API文档中，有这样一些类，以Abstract开头，例如：AbstractQueue。这些类是为了类库实现者设计的，若想自己实现队列类，扩展这些抽象类命名的类要容易很多。

#### Java类库中的集合接口和迭代器接口

​	Java类库中，集合类的基本接口是Collection接口。该接口有几个基本方法：

```java
boolean add(E e);
Iterator<E> iterator();
```

​	add()用于向集合中添加元素。若添加元素确实改变了集合就返回true，集合没有发生变化就返回false。若向集合中添加一个已经存在的元素，则返回false，因为集合不允许有重复的元素存在。

iterator()方法返回了一个实现了Iterator接口的对象，使用这个迭代器对象可以一次访问集合中的元素。迭代器接口主要有以下四个方法：

```java
public interface Iterator<E>{
  boolean hasNext();
  E next();
  void remove();
  void forEachRemaining(Consumer<? super E> action)//1.8新增方法
}
```

1、遍历元素

​	通过反复调用next方法，可以逐个访问集合中的元素，但是，到达集合的末尾，next方法将会抛出一个NoSuchElementException。因此，在调用next之前先调用hasNext方法判断一下。

```java
        Collection<String> strings = new ArrayList<>();
        Iterator<String> iterator = strings.iterator();
        while (iterator.hasNext()) {
            String next = iterator.next();
            System.out.println(next);
        }
```

在Javase 5.0之后这个循环有了更优雅的写法 forEach：

```java
       for (String string : strings) {
            System.out.println(string);
        }
```

forEach循环可以与任何实现了Iterable接口的对象一起工作。Collocation接口扩展了Iterable接口，因此对于标准集合类库中的任何集合都可以使用。

> forEach写法对于基本数组来说是由编译器翻译成标准for循环来进行遍历，对于集合来说是使用了迭代器进行遍历，更加安全也更加高效，如：遍历LinkedList，传统for循环遍历到i再list.get(i)这样的时间复杂度就是O(N^2)了

​	java类库的迭代器查找操作和位置变更是紧密联系的，查找一个元素的唯一方法就是next，返回该元素的引用的同时迭代器的位置向前移动。

> 找一次就变更一次位置，即一个迭代器遍历到尾部之后，再使用这个迭代器取下一个元素就会抛出异常了。
>
> 可以将Iterator.next与InputStream.read看做等效的，都是移动一次位置读取一个元素

2、删除元素

​	Iterator接口的remove方法将会**删除上次调用next方法时返回的元素**。因此，remove方法前必须先用next方法越过一个元素才能将该元素删除，否则会抛出IllegalStateException异常。

> 即remove方法和next方法的调用具有互相依赖性

3、泛型实用方法

Collection和Iterator都是泛型接口，具有一些集合的实用方法，以下是一些例子：

- int size()
- boolean isEmpty()
- boolean contains(Object obj)
- boolean containsAll(Collection<? extends E> from)
- boolean addAll(Collection<? extends E> from)
- boolean remove(Object obj)
- boolean removeAll(Collection<? >c) //从这个集合中删除与other集合中相同的元素，即去重。如果改变了该集合则返回true
- void clear()
- boolean retainAll(Collection<? >c) //从这个集合中删除与other集合中不同的元素，即求并集。如果改变了该集合则返回true
- Object[] toArray()//返回这个集合的对象数组
- <T> T[] toArray(T[] arrayToFill)//返回值这个集合的对象数组。如果arrayToFill足够大，就将集合中的元素填入这个数组中，剩余空间补null；否则，分配一个新数组，其成员类型与arrayToFill的成员类型相同，其大小等于集合的大小，并填入集合元素。

### 具体的集合

​	下表java集合具体实现类中，除了Map结尾的类，其余都继承了Collection接口。

| 集合类型        | 描述                                                 |
| --------------- | ---------------------------------------------------- |
| ArrayList       | 一种可以动态增长和缩减的索引序列                     |
| LinkedList      | 一种可以在任何位置进行高效的插入和删除操作的有序序列 |
| ArrayDeque      | 一种用循环数组实现的双端队列                         |
| HshSet          | 一种没有重复元素的无序集合                           |
| TreeSet         | 一种有序集合                                         |
| EnumSet         | 一种包含枚举类型的集合                               |
| LinkedHashSet   | 一种可以记住元素插入顺序的集合                       |
| PriorityQueue   | 一种允许高效删除最小元素的集合                       |
| HashMap         | 一种存储键/值关联的数据结构                          |
| TreeMap         | 一种键值有序排列的映射表                             |
| EnumMap         | 一种键值属于枚举类的映射表                           |
| LinkedHashMap   | 一种可以记住键值对添加顺序的映射表                   |
| WeakHashMap     | 一种其值无用武之地后可以被垃圾回收器回收的映射表     |
| IdentityHashMap | 一种用==而不是equals比较键值对的映射表               |

#### 链表

​	链表将每个对象放在独立的节点中。每个节点还存放着下一个节点的位置。在java中所有链表的设计都是双向链表，即节点也保存着前驱结点的位置。

​	链表还有一个有序集合，每个对象的位置十分重要。LinkedList.add方法将对象添加到链表的尾部，但是，常常需要将元素添加到链表的中间。由于迭代器是描述集合位置的，所以这种依赖于位置的add方法将由迭代器负责。只有对自然有序的集合使用迭代器添加元素才有实际意义。

​	集合类库中还提供了迭代器子接口ListIterator，其中包含add方法

```java
interface ListIterator <E> extends Iterator<E>{
	void add(E element);	
}
```

​	与Collection.add不同，这个方法不返回boolean值，因为它默认所有添加操作都会成功。另外，ListIterator接口有两个方法可以用来反向遍历链表。

```java
E previous();
boolean hashPrevious();
```

​	LinkedList类的listIterator()方法返回一个实现了ListIterator接口的迭代器对象。

​	迭代器的Add方法在迭代器位置之前添加一个新对象，多次调用add方法将按照提供的顺序将元素添加到迭代器当前位置之前。

​	当用一个刚刚由Iterator方法返回，并且指向链表表头的迭代器调用add操作时，新添加的元素会变成链表的新表头。当迭代器越过最后一个元素时（即hasNext方法返回false），新添加的元素将会成为新的表尾。如果链表有n个元素，那么就有n+1个位置可以插入元素，这些位置与迭代器的n+1个可能的位置对应。

​	set方法会取代上一个被next()或previous()方法返回的元素。

```java
ListIterator<String> iter = list.listIterator();
String old = iter.next();
iter.set(newString);
```

​	java标准集合类库中的集合均为线程不安全的。

​	若一个迭代器在遍历时，另一个迭代器对集合**结构**进行了修改，或者用集合自身的方法进行修改了集合的**结构**，都会抛出concurrentModificationException异常（并发修改异常），注意：只有修改并发的修改集合的结构才会抛出上述异常，set这种取代元素而不改变集合结构的不会抛出异常。

> 为了避免发生并发修改异常，可以遵循以下简单原则：
>
> 1. 可以根据需要给容器附加许多的迭代器，但是这些迭代器只能读取列表。再另外增加一个能读能写的迭代器。

​	列表迭代器还有一个方法，可以返回当前迭代器所在位置的索引，实际上，因为迭代器处于两个元素中间，因此可以返回两个前后两个索引，nextIndex方法可以返回下一次调用next返回元素所处位置的整数索引，反之previouesIndex，返回上previous方法返回元素的整数索引。这两个方法效率很高，因为迭代器直接保存着当前位置的索引数值。

​	也可以直接指定list中的位置返回一个迭代器：list.ListIterator(n)将返回在指定位置的迭代器，只不过这个迭代器的获取效率低一点。

> ​	在只有少量元素时，可以完全不用为数组插入删除的性能开销担心，可以放心的使用ArrayList，使用LinkedList的唯一理由是尽可能的减少在列表中间插入删除元素的性能开销。
>
> ​	在使用链表时，尽量不使用以整数索引表示链表中位置的所有方法，每访问一次都要从头开始遍历，要想随机访问使用ArrayList。

#### 散列集

​	数组和链表可以按照意愿来安排顺序，但是若是忘记或者不知道某个特定元素的索引位置，想要找到它就必须要进行遍历，效率就很低下。若不在意元素的顺序，就可以使用散列集来存储数据，散列集按照有利于操作目的的原则存放对象。           

​	散列表为每个元素计算一个整数，称为散列码（Hash Code）。散列码是由对象的实例域产生的一个整数，不同实例域的对象将产生不同的散列码。

​	若自定义类，就要负责实现这个类的hashCode方法，注意，自己实现的hashCode方法应该与equals方法兼容，因为若a.equals(b),则a和b必须具有相同的散列码。

​	在java中，散列表用链表数组实现（jdk1.8之后加入了链表和红黑树的转化）。每个列表称之为桶（bucket，列表的头结点索引保存在数组中）。要查找表中元素的位置，就要先计算元素的散列码，然后与桶的总数取余，余数就是元素在桶的索引。

​	计算元素散列码之后找到存放该元素的桶，若此时桶中没有元素，则直接插入即可，若该桶中已经存在元素（这种现象被称之为哈希（散列）冲突），需要用新对象与桶中每个元素逐一比较，查看这个对象是否已经存在。

​	若想更好的控制散列表的运行性能，就要指定一个初始的桶数，使元素能够尽可能均匀的分布在桶中。比如，预先知道最终大致会有多少个元素要插入到散列表中，就可以设置桶数。通常将桶数设置为预计元素个数的75%~150%。标准类库使用的桶数是2的幂，默认值为16（任何提供的初始值都会被自动转化为2的下一个幂）。

​	当然，并不是所有时候都可以估算要存储的元素个数，也有可能初始估计过少。若散列表太满，就需要再散列（rehashed）。再散列需要一个桶数更大的表，并将所有新元素插入到新表中，然后丢弃原来的表。装填因子（load factor）决定何时对散列表进行再散列。例如，标准类库中默认的装填因子为0.75，当表中超过75%的位置装满了元素时，这个表就会用双倍的桶数进行再散列，对于大部分情况来说，装填因子为0.75是比较合理的。

​	散列表可以用于实现几个重要的数据结构，其中最简单的是set类型。set是没有重复元素的元素集合，set的add方法首先在集合中查找是否已经存在要添加的对象，若不存在则添加进set。

​	java集合类库提供了一个HashSet类，它实现了基于散列表的集，可以使用add方法添加元素，contains方法已经被重新定义，可以用来快速查看是否某个元素已经存在集中。它只在某个桶中查找元素，而不必查看集合中所有元素。

​	散列集迭代器一次访问所有的桶。由于散列将各个元素分散在表的各个位置上，所以访问它们的顺序是随机的。

#### 树集

​	TreeSet与散列集非常相似，但树集有序。可以以任意顺序将元素插入到集合中，对树集进行遍历时，将按照插入顺序遍历呈现。

​	如类名所示，排序是用树结构完成的（当前实现是红黑树）每次将一个元素添加到树中时，都被放在正确的排序位置上，因此，迭代器总是以排好序的顺序访问每个元素。

​	将一个元素添加到树集中比添加到散列集中慢，但相比添加到数组和链表的正确位置上还是快很多，红黑树查找一个元素的平均需要log2n次的比较。

##### 对象的比较

​	TreeSet如何知道怎样去排序元素呢？在默认情况，树集假定插入的元素实现了Comparable接口，该接口定义了一个方法：

```java
public interface Comparable<T>{
  int compareTo(T other);
}

{
  a.compareTo(b);//若a>b 则返回正数，相等返回0，a<b返回负数
}
```

​	有些Java平台类实现了Comparable接口，例如String类，String类的Compare方法依据字典序列对字符串进行比较。

​	然而，使用Comparable接口定义排序序列并不方便，一个类只能实现compareTo方法一次。若需要TreeSet按照不同的方式进行排序，或者该类的创建者没有实现Comparable接口，这种情况下，可以 通过将Comparator对象传递给TreeSet构造器来告诉TreeSet使用不同的比较方法。Comparable接口声明了带有两个参数的compare方法：

```java
public interface Comparable<T>{
  int compare(T a,T b);//与上述compareTo方法类似
}
```

   若要实现传入自定义的比较器，先定义一个实现Comparable接口的类，重写compare方法：

```java
class ItemComparator implements Comparable<Item>{
  public int compare(Item a, Item b){
    return a.getDescription().compareTo(b.getDescription());
  }
}
```

然后创建这个类的对象，在构造TreeSet的时候传入该对象即可创建一个带比较器的树集。

> 若有排序需求，可以使用树集，否则不需要使用树集付出排序的开销。
>
> Java6之后TreeSet还实现了Navigable接口，详情查看API文档，后续补充。

#### 队列和双端队列

​	队列可以有效的将一个元素添加到尾部，在头部删除一个元素。有两个端头的队列即双端队列，可以高效的在头部和尾部添加或删除元素。不支持在中间添加或删除元素。在JDK6中加入了Deque接口，并由ArrayDeque和linkedList类实现。这两个类提供了双端队列，而且在必要时刻可以增加队列的长队。

[API]: java.util.Queue<E>	"JDK5.0"

> - boolean add(E element)
>
> - boolean offer(E element)
>
>   如果队列没有满，将给定的元素添加到这个队列的尾部并返回true，如果队列已满，第一个方法将抛出illegalException，第二个方法返回false
>
> - E remove()
>
> - E poll()
>
>   假如队列不为空，删除并返回该队列的头部元素。如果队列是空的，第一个方法抛出NoSuchElementException，第二个方法返回null。
>
> - E element()
>
> - E peek()
>
>   如果队列不空，返回这个队列的头部元素，但不删除，如果队列空，第一个方法将抛出NoSuchElementException，第二个方法返回null。

[API]: java.util.Deque<E>	"JDK6.0"

> - void addFirst(E element)
>
> - void addLast(E element)
>
> - boolean offerFirst(E element)
>
> - boolean offerLast(E element)
>
>   将给定的对象添加到双端队列的头部或尾部，如果队列满了，前两个方法将抛出一个IllegalStateException，后两个方法返回false
>
> - E removeFirst()
>
> - E removeLast()
>
> - E pollFIrst()
>
> - E pollLase()
>
>   若队列不为空，删除并返回队列头部或尾部元素，若队列为空，前两个方法将抛出NoSuchElementException，后两个方法返回null。
>
> - E getFirst()
>
> - E getLast()
>
> - E peekFirst()
>
> - E peekLast()
>
>   若队列非空，返回头部或尾部元素，但不删除，若队列为空，前面两个方法将抛出NoSuchElementException，后两个方法返回null。

[API]: java.util.ArrayDeque<E>;	"JDK6.0"

> - ArrayDeque()
>
> - ArrayDeque(int initialCapacity)
>
>   用初始容量16或指定的初始容量构造一个无限双端队列。

#### 优先级队列

​	优先级队列(priority queue)中的元素可以按照任意的顺序插入，却总是按照排序的顺序进行检索。即无论何时调用remove方法，总会获得当前优先级队列中最小的元素。然而，优先级队列并没有对所有元素进行排序。如果用迭代的方式处理这个元素，并不需要对它们进行排序。优先级队列使用了一个优雅且搞笑的数据结构，堆(heap)。堆是一个可以自我调节的二叉树，对树进行添加和删除操作，可以让最小的元素移动到根，而不必花费时间对元素进行排序。

​	与TreeSet一样，一个优先级队列既可以保存实现了Comparable接口的对象，也可以保存在构造器中提供比较器的对象。

​	使用优先级队列的典型事例是任务调度，每一个任务有一个优先级，任务以随机顺序添加到队列中。每当启动一个新任务时，都将优先级最高的任务从队列中删除（一般优先级越高，用于表示优先级的数字就越小，因此会将优先级最高的元素删除）。

#### 映射表

​	映射表(Map)可以根据某些键的信息，查找到与之对应的元素。映射表用来存放键值对。

##### 基本操作

​	Java为映射表提供了两个实现，HashMap和TreeMap。这两个类都实现了Map接口。

​	散列映射表（HashMap）对键进行散列，树映射表用键的整体顺序对元素进行排序，并将其组织成搜索树。散列只能作用于键，值不能进行散列或比较。

> 若有排序需求则选择使用树映射表，否则尽量使用散列映射表。

​	每当使用put方法向映射中添加对象时，必须同时提供一个键。检索对象时必须提供一个键（因此，要记住这个键）。若Map中没有给定键的对应信息则返回null，null返回值可能不方便，有时可以有一个好的默认值，使用getOrDefault(Object key,V defaultValue)方法可以在给定键的值为null时返回默认的值。

​	键必须是唯一的。不能对同一键存放两个值，若对同一个键使用两次put方法，则第二次加入的元素会取代第一次put进去的元素。事实上，put方法将返回这个键上次存储的值。

##### 更新映射项

​	处理映射时一个难点就是更新映射项。正常情况下，先取得一个键关联的原值，更新后再放回去。不过有些特殊情况，比如键第一次出现，先取的话返回null，可能会出现NPE，因此可以使用getOrDefault方法补救，还可以使用putIfAbsent方法

```java
//单次出现次数计数，常规更新映射项，若是第一次更新则会出现NPE
Counts.put(word,Counts.get(word)+1);

//第一种补救措施
Counts.put(word,Counts.getOrDefault(word,0)+1);


```

​	

​	Map并不算是集合，也可以将Map看作是键值对的集合。因此，可以获得Map的几个视图，键集(keySet()) ，值集合（values()可重复）和键值对集合(entrySet())

> - keySet()
> - values()
> - entrySet()
>
> ​	以上三个方法返回改Map的三个不同视图。keySet并不是HashSet或者TreeSet，而是实现了Set接口的某个其他类的对象（定义在实现类中的类，如HashMap中的KeySet），Set接口扩展了Collection接口，因此可以像使用其他集合一样使用keySet。
>
> ​	若调用Map的三个视图的迭代器的remove方法，实际上就是从Map中删除了键/值或者键值对，但是，不能将元素添加到键集中，因为只添加键而不添加值是没有意义的，会抛出UnsupportOperationException异常。键值对集也有同样的限制，但添加新的键值对本应该有意义。

#### 专用集与映射表类

​	集合框架中有几个专用的映射表类。

1. ##### 弱散列映射表(WeakHashMap)

   ​	设计该类是为了解决一个问题：若一个值，对应的键已经不再使用了，即不再有任何途径引用这个值的元素对象了，但也正是由于程序中不再出现这个键，所以这个键值对对象无法从Map中删除，也无法被垃圾回收器回收，因为垃圾回收器跟踪活动的对象。只要映射表是活动的，其中所有的桶都是活动的，它们就不能被回收。因此需要程序负责将映射表中无用的值进行删除，或者使用WeakHashMap来完成上述事情。当键的唯一引用来源于散列表条目(entry)时，这一数据结构将与垃圾回收器协同工作一起删除无用键值对。

> WeakHashMap使用弱引用(weak reference)保存键。WeakReference对象将引用保存到另外一个对象中，在这里，就是将散列表的键。对于被WeakReference对象包裹的对象，垃圾收集器有另外的处理方式。通常，如果垃圾回收器发现某个特定对象已经没有其他引用途径了，就将其回收。然而，当某个对象只有弱引用来引用它的话，垃圾回收器仍然回收它，但要将这个对象的弱引用放入队列中。WeakHashMap将周期性的检查队列，以便找出新添加的弱引用。一个弱引用进入队列意味着这个键不再被其他对象使用，并且已经被垃圾回收器收集起来。于是，WeakHashMap删除对应的条目。

2. ##### 链接散列表和链接散列映射表

   ​	Java SE 1.4新增了两个类：LinkedHashSet和LinkedHashMap，用来记住插入元素的先后顺序，当插入条目时，条目会并入双向链表中。

   ​	链接散列映射表(LinkedHashMap)有两种遍历模式：插入顺序遍历和访问顺序遍历。默认使用插入顺序遍历，创建LinkedHashMap时在构造器中设置accessOrder为true时，将使用访问顺序遍历，每次调用get或put，受到影响的条目将从当前链表位置删除，并放到条目链表的尾部(只有条目在链表中的位置受到影响，在散列表中的桶位置不会受到影响)。

   ```java
   //创建一个使用访问顺序遍历模式的LinkedHashMap
   Map<K,V> map = new LinkedHashMap<K,V>(initialCapacity,loadFactor,true);
   ```

   ​	访问顺序对于实现高速缓存的”最近最少使用（LRU）“原则十分重要。例如，希望访问频率较高的元素放到内存，而访问频率较低的元素从数据库读取。当在表中找不到元素而表已经满时，可以使用迭代器将前面几个元素删除，最前面的元素是近期使用最少的元素。

   ​	甚至可以让这个过程实现自动化：构造一个LinkedHashMap的子类，然后重写下面这个方法

   ```java
   //源码注释详细的介绍了该方法的重写细节，每当使用put或putAll方法添加元素时，都会检查该方法是否返回true，若为true，则删除最前面的元素
   protect boolean removeEldestEntry(Map.Entry<K,V> eldest){return false;}
   
   //重写例子：缓存中存放100个常用元素
   Map<Integer,String> cache = new LinkedHashMap<>(128,0.75f,true){
     @Override
     protected boolean removeEldestEntry(Map.Entry<Integer, String> eldest) {
       return size()>100;
     }
   };
   ```

3. ##### 枚举集与映射表

   ​	EnumSet是一个枚举类型元素集的高效实现。由于枚举类型只有有限个实例，所以EnumSet内部使用位序列实现。如果对应的值在集合中，则对应的位被置为1。

   EnumSet没有公共的构造器，可以使用静态工厂方法构造EnumSet。

   ```java
   enum WeekDay {
       MONDAY,TUESDAY,WEDNESDAY,THURSDAY,FRIDAY,SATURDAY,SUNDAY;
   }
   
   public static void main(String[] args){
       EnumSet<WeekDay> always = EnumSet.allOf(WeekDay.class);
       EnumSet<WeekDay> never = EnumSet.noneOf(WeekDay.class);
       EnumSet<WeekDay> workDays = EnumSet.range(WeekDay.MONDAY,WeekDay.FRIDAY);
   }
   ```

   ​	可以使用Set接口的常用方法来修改EnumSet。

   ​	EnumMap是一个键类型为枚举类型的映射表，它可以直接且高效的用一个值数组实现。

   ```java
   //需要在构造器中指定值类型
   EnumMap<WeekDay,User> personInCharge = new EnumMap<>(WeekDay.class);
   ```

4. ##### 标识散列映射表

   IdentityHashMap，这个类中，hash值不是使用hashCode方法计算的，而是用System.identityHashCode()方法计算。这是Object.hashCode方法根据对象的内存地址来计算散列码所使用的方式。而且，IdentityHashMap比较对象时使用==，而不使用equals。

#### 视图与包装器

​	通过视图(views)可以获得其他实现了Collection和Map接口的对象。Map的keySet方法就是这样一个示例，看起来这个方法创建了一个新集，并将Map中所有的键都填进去，然后返回这个集。但实际情况并非如此，keySet返回一个实现Set接口的对象，这个类的方法对原映射进行操作(即对于Map的keySet方法返回的keySet对象的操作能够改变Map中的键，即有关联的集合成才称为视图)







#### 算法

​	泛型集合接口有个很大的缺点，通用算法只需要实现一次，就无需程序员重复的实现，也可以避免重复实现导致错误的出现。

##### 排序与混排

​	Collections类中的sort方法实现了对List接口的集合进行排序。

```java
List<String> staff = new ArrayList<>();
//fill collection
Collections.sort(staff);
```

这个方法假定列表元素实现了Comparable接口。如果箱采用其他方式对列表进行排序，可以使用List接口的sort方法并传入一个Comparator对象。如下可以按工资对员工进行排序。

```java
        Collections.sort(a,person::compareTo);
```



Collections类还有一个算法shuffle，其功能刚好和排序相反，即随机的混排列表中元素的顺序。



##### 二分查找

​	Collections类中binarySearch方法实现了这个算法。但集合必须是有序的，否则将返回错误的结果。要想查找某个元素，必须提供集合（该集合要实现List接口）以及要查找的元素。若该集合没有采用Comparable接口的compareTo方法进行排序，就还需要提供一个比较器对象。

```java
 i = Collections.binarySearch(c,element);
 i = Collections.binarySearch(c,element,comparator);
```

​	如果binarySearch方法返回值大于等于0，则表示匹配对象的索引。若返回负值，则表示没有匹配的元素。但是，可以利用返回值计算应该将element插入到集合的那个位置，以保持集合的有序性。插入的位置是：

```java
insertionPoint = -i - 1；
```

​	这并不是简单的-i，因为0值是不确定的，即以下操作可以将元素插入到正确的位置上：

```java
if(i < 0){
  c.add(-i -1,element);
}
```

​	只有采用随机访问，二分查找才有意义，若必须使用迭代的方式一次次遍历链表的一半来找到中间值，那二分查找就失去了优势，因此，如果为binarySearch方法提供一个链表，他将自动的变为线性查找。

##### 简单算法

​	在Collections类中包含了几个简单且有用的算法，除了前面的查找集合中最大元素之外，还包括：将一个列表中的元素复制到另外一个表中；用一个常量填充容器；逆置一个列表的元素顺序。

​	之所以提供这些简单的算法（使用一个循环即可轻易实现的事情，为何还要提供算法Api？），是因为这些简单算法让程序员阅读程序变得轻松，当阅读别人的循环时，肯定要理解别人的想法，但是看到Collections中的简单算法即可立刻明白这行代码的用途。如：

```java
//手写循环
for(int i = 0; i < words.size();i++){
  if(words.get(i).equals("C++")){
    words.set(i,"Java");
  }
}
//简单算法
Collections.replaceAll("C++","Java");
//对比起来，简单算法Api清晰明了
```

​	Java 8 之后增加了默认方法Collection.removeIf和List.replaceAll，这两个方法稍微复杂一点。要提供一个lambda表达式来测试或者转换元素。如下列代码将删除所有长度小于3的单次，并把其余单次改成小写：

```java
words.removeIf(w -> w.length() <= 3);
words.replaceAll(String::tolowerCase);
```

##### 批操作

​	很多操作会成批复制或者删除元素。以下调用：

```java
//将从coll1众删除coll2中出现的所有元素。
coll1.removeAll(coll2);
//将从coll1中删除coll2中没有出现的元素
coll1.retainAll(coll2);
```

​	找出两个集合(a和b)的交集

```java
Set<String> result = new HashSet<>(a);
result.retainAll(b);
//这样就得到了两个集合的并集result
```

​	建立一个子列表筛选出前10个元素

```java
a.addAll(b.busList(0,10));
```



##### 集合与数组的转换

​	把数组转化为集合，Arrays.asList包装器可以达到这个目的。例如：

```java
String[] values = ...;
HashSet<String> staff = new HashSet(Arrays.asList(values));
```

​	从集合得到一个数组就困难一些，可以使用toArray方法：

```java
//返回的是一个Object数组，但是不能通过强转改变Object数组的类型
Object[] values = staff.toArray();
```

​	想要获得指定对象类型的数组，必须先提供一个所需类型且长度为0的数组，toArray()方法就会创建相同类型的数组类型：

```java
String[] values = staff.toArray(new String[0]);

//若构造了一个指定足够大小的数组，则不会创建新数组
staff.toArray(new String[staff.length()])
```

#### 遗留的集合

##### Hashtable类

​	HashTable和HashMap类作用一致，与Vector类的方法非常相似，Hashtable方法是同步的，若对同步性与遗留代码的兼容性没有任何要求即可使用HashMap。如需并发访问，可以使用concurrentHashamap。

##### 枚举

​	遗留集合使用Enumeration接口对元素序列进行遍历。Enumeration接口有两个方法，即hasMoreElements和nextElement。

##### 位集

​	BitSet类用于存放一个位序列，若需要高效的存储位序列可以使用位集。由于位集将位包装在字节里，因此要比使用Boolean对象的ArrayList更加高效。

​	BitSet提供了一个便于读取、设置或清除各个位的接口，使用这个接口可以避免和屏蔽其他麻烦的位操作。如：

```java
//对于一个名为bucketOfBits的BitSet
//获取第i位的标记，true或false
bucketOfBits.get(i);
//设置第i位为true状态
bucketOfBits.set(i);
//将第位置为fasle
bucketOfBits.clear(i);
```

```
• BitSet(int initialCapacity) 创建一个位集。
• int length( ) 返回位集的“ 逻辑长度”， 即 1 加上位集的最高设置位的索引。 
• boolean get(int bit) 获得一个位。 
• void set(int bit) 设置一个位。 
• void clear(int bit) 清除一个位。 
• void and(BitSet set ) 这个位集与另一个位集进行逻辑“ AND”。 
• void or(BitSet set ) 这个位集与另一个位集进行逻辑“ OR”。 
• void xor(BitSet set ) 这个位集与另一个位集进行逻辑“ X0R”。 
• void andNot(BitSet set) 清除这个位集中对应另一个位集中设置的所有位。
```

### 并发

​	操作系统中的多任务(multitasking)：在同一时刻运行多个程序的能力。操作系统将CPU的时间片分配给每一个进程，给人并行处理的感觉。

​	多线程程序在较低层次上扩展了多任务的概念：一个程序同时执行多个任务。每一个任务被称为线程(thread)，它是线程控制的简称。可以同时运行一个以上线程的程序称为多线程程序(multithreaded)。

​	多线程和多进程的区别：本质的区别在于每个进程有自己的一套变量体系，而线程则共享数据，所以才会出现线程安全问题，但正是因为共享变量，线程间的通信比进程间通信更加有效和容易，此外，在某些操作系统中，线程比进程更轻量级，创建和撤销的开销比进程小得多。

#### 中断线程

​	当线程run方法执行方法体中最后一条语句后，并经由执行return语句返回时，或者出现了在方法中没有捕获的异常时，线程将终止。

​	没有强制终止线程的方法，但是，interrupt方法可以用来请求终止线程。

​	当对一个线程调用interrupt方法时，线程的中断状态将被置位。这是每一个线程都有的一个boolean标志。每个线程都应该不时的检查这个标志，以判断线程是否被中断。

​	在线程中获取中断状态标记位：首先调用静态的Thread.currentThread方法获取当前线程，然后调用isInterrupted方法：

```java
while(!Thread.currentThread().isInterrupted()&&more work to do ){
  //do more work
}
```

​	但是，如果线程被阻塞，就无法检测中断状态。这是产生interruptedException异常的地方。当在一个被阻塞的线程(调用sleep或wait)上调用interrupt方法时，阻塞调用将会被InterruptedException异常中断。(存在不能被中断的阻塞I/O调用，应该考虑选择可中断的调用)

​	中断一个线程只是引起该线程的注意。被中断的线程可以决定如何响应中断。某些线程比较重要以至于应该处理完成异常之后继续执行而不理会中断。但更加普遍的情况是，线程将中断位作为一个终止的请求。这种线程的run方法具有一下形式：

```java
Runnable r = () -> {
	try{
    ...
    while(!Thread.currentThread().inInterrupted()&& more work to do){
      do more work
    }
  }
  catch(InterruptedException e){
    //thread was interrupted during sleep or wait
  }finally{
    clearup,if required
  }
  exiting the run method terminates the thread
};
```

​	如果在while循环中都调用sleep方法，isInterrupted方法的检测没有必要也没有用处，当中断位被置位时调用sleep方法，它不会休眠，反而清除中断状态并且抛出InterrupedException。因此，如果你的循环调用sleep，不会检测中断状态。

# 卷二 高级特性

## Java8的流库

​	与集合相比，流提供了一种在更高概念级别上指定计算任务的数据视图。

### 从迭代到流的操作

​	在处理集合时，我们一般使用迭代遍历它的元素，然后对元素进行操作，如：对一个单词列表中所以长度超过12的单词计数

```java
int count = 0;
for(String word : words){
  if(word.length() > 12){
    count++;
  }
}
```

​	使用流时，相同的操作如下：

```java
long count = words.stream().filter(word -> w.length() > 12).count();
```

