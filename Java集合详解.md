## Java 集合详解

### 类层次关系

Collection

|-List
| |-LinkedList 非同步 允许null元素
| |-ArrayList  非同步 允许null元素
| |-Vector     同步
|   -|Stack
|-Set

Map
|-Hashtable
|-HashMap
|-WeakHashMap

### Colloection接口（而不是类）
Collection是最基本的集合接口，一个Collection代表一组Object，即Collection的元素（Elements）。一些 Collection允许相同的元素而另一些不行。一些能排序（List）而另一些不行(Set)。Java SDK不提供直接继承自Collection的类，Java SDK提供的类都是继承自Collection的“子接口”如List和Set。

如何遍历Collection中的每一个元素？不论Collection的实际类型如何，它都支持一个"iterator()"的方法，该方法返回一个迭代子，使用该迭代子即可逐一访问Collection中每一个元素。典型的用法如下：

　　　　Iterator it = collection.iterator(); // 获得一个迭代子
　　　　while(it.hasNext()) {
　　　　　　Object obj = it.next(); // 得到下一个元素
　　　　}

由Collection接口派生的两个接口是List和Set。

### List接口
List是“有序”的Collection，使用此接口能够精确的控制每个元素插入的位置。用户能够使用“索引”（元素在List中的位置，类似于数组下标，这也决定了List可以通过fori循环来遍历）来访问List中的元素，这类似于Java的数组。
和下面要提到的Set不同，List允许有“相同”的元素。

List还提供一个listIterator()方法，返回一个ListIterator接口，和标准的Iterator接口相比，ListIterator多了一些add()之类的方法，允许添加，删除，设定元素，还能向前或向后遍历。

实现List接口的常用类有LinkedList，ArrayList，Vector和Stack。

#### ArrayList 和 LinkedList
Arraylist是实现了基于动态数组的数据结构，Linked基于链表的数据结构。
##### 1. ArrayList和LinkedList的时间复杂度比较：
(1). 对于随机访问get和set，ArrayList由于LinkedList，因为ArrayList可以随机访问，而LinkedList需要一步一步的移动到节点处。

(2).对于新增和删除操作add和remove，LinedList比较占优势，只需要对指针进行修改即可，而ArrayList要移动数据来填补被删除的对象的空间。

ArrayList和LinkedList是两个集合类，用于存储一系列的对象引用(references)。例如我们可以用ArrayList来存储一系列的String或者Integer。那么ArrayList和LinkedList在性能上有什么差别呢？什么时候应该用ArrayList什么时候又该用LinkedList呢？

##### 2. ArrayList和LinkedList的空间复杂度：

LinkedList 比 ArrayList空间开销更大，因为有指针 指向前一个元素和后一个元素的指针。

在LinkedList中有一个私有的内部类，定义如下：

private static class Entry {
         Object element; 
         Entry next; 
         Entry previous; 
     }

每个Entry对象reference列表中的一个元素，同时还有在LinkedList中它的上一个元素和下一个元素。一个有1000个元素的LinkedList对象将有1000个链接在一起的Entry对象，每个对象都对应于列表中的一个元素。这样的话，在一个LinkedList结构中将有一个很大的空间开销，因为它要存储这1000个Entity对象的相关信息。

ArrayList使用一个内置的数组来存储元素，这个数组的起始容量是10.当数组需要增长时，新的容量按如下公式获得：新容量=(旧容量*3)/2+1，也就是说每一次容量大概会增长50%。这就意味着，如果你有一个包含大量元素的ArrayList对象，那么最终将有很大的空间会被浪费掉，这个浪费是由ArrayList的工作方式本身造成的。如果没有足够的空间来存放新的元素，数组将不得不被重新进行分配以便能够增加新的元素。对数组进行重新分配，将会导致性能急剧下降。如果我们知道一个ArrayList将会有多少个元素，我们可以通过构造方法来指定容量。我们还可以通过trimToSize方法在ArrayList分配完毕之后去掉浪费掉的空间。

##### 3. ArrayList和LinkedList的选择总结
(1). 适用于ArrayList的情况：在一列数据后面添加或删除数据而不是在一列数据的前面和中间，并且需要随机的访问其中的元素；
(2). 适用于LinkeList的情况：在一列数据前面和中间添加或删除元素，并且按照顺序访问其中的元素。

#### Vector类详解 --- 同步 
Vector非常类似ArrayList，但是Vector是同步的。由Vector创建的Iterator，虽然和ArrayList创建的 Iterator是同一接口，但是，因为Vector是同步的，当一个Iterator被创建而且正在被使用，另一个线程改变了Vector的状态（例如，添加或删除了一些元素），这时调用Iterator的方法时将抛出ConcurrentModificationException，因此必须捕获该异常。

##### Stack类
继承自Vector, 实现一个后进先出的堆栈，提供了额外的5个方法：push,pop,peak,empty,search。

### Set接口
Set是一种不允许包含重复(通过equals方法来判断元素是否重复)元素的Collection，即任意的两个元素e1和e2都有e1.equals(e2)=false，Set最多有一个null元素。

### Hash接口
请注意，Map没有继承Collection接口，Map提供key到value的映射。一个Map中不能包含相同的key，每个key只能映射一个 value。Map接口提供3种集合的视图，Map的内容可以被当作1. 一组key集合，2. 一组value集合，或者3. 一组key-value映射。

#### HashTable类 同步的
Hashtable继承Map接口，实现一个key-value映射的哈希表。任何“非空”（non-null）的对象都可作为key或者value。

由于作为key的对象将通过计算其散列函数来确定与之对应的value的位置，因此任何作为key的对象都必须实现hashCode和equals方法。hashCode和equals方法继承自根类Object。

只需要牢记一条：要同时复写equals方法和hashCode方法，而不要只写其中一个。
　　
#### HashMap类
和HashTable类似，但是HashMap是非同步的，允许null， null key 和 null value均可。

#### WeakHashMap类
WeakHashMap是一种改进的HashMap，它对key实行“弱引用”，如果一个key不再被外部所引用，那么该key可以被GC回收。