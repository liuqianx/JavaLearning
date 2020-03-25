# Java重点

## 1. 基础

1. **静态变量/方法**：主要考虑的是使用时不需要再实例化一个类，方便在没有创建对象的情况下调用。

2. **static/final关键字**：static强调程序运行期间变量始终存在于内存；final强调变量只能被赋值一次。

3. **基本数据类型/包装数据类型**：有8个，每个包装数据类型都封装了相应的基本数据类型，并提供了一些有用的方法。例如int和Integer，char和Character。（拆箱装箱）

4. **引用数据类型**：除去以上的8种基本数据类型，其他的都是引用类型，也就是Object。分为5种：类，接口，数组，枚举，注解。

5. **对象引用**：reference，类似指针，用来指向一个实例对象（内存空间）的标识符，从而操纵对象。

6. **向上转型**：父类引用指向子类对象，子类引用不能指向父类对象，例：Father f1 = new Son()。

   - 意义：失去子类独有的方法，但是可以调用重写后的父类方法，把这个子类看作父类进行使用，只能做出父类的行为。

7. **向下转型**： Son s1 = (Son) f1—— 子类引用s1指向一个子类对象，父类引用f1还是指向子类对象。

8. **接口/抽象类**：抽象类是对根源的抽象，而接口是对动作的抽象。抽象类主要是用来抽象类别，接口主要是用来抽象方法功能。当你关注事物的本质的时候，用抽象类；当你关注一种操作的时候，用接口。

   接口里面只能对方法进行声明，抽象类既可以对方法进行声明也可以对方法进行实现；抽象类除了无法被实例化，和普通的类几乎一样；一个类只能继承一个抽象类，但能实现多个接口。

9. **重载(Overload)/重写(Override)**：Overload指的是一个类中可以存在多个同名不同参数的方法；Override指的是在子类对父类同名方法进行覆盖，参数必须相同。

10. **多态**：多态指一个接口可以有多种实现的方式/一个类可以实现多个接口，以及override/overload。

11. **泛型**：把明确类型的工作推迟到创建对象或调用方法的时候（反射机制）。编译器编译完成后，生成的.class文件中不含泛型（泛型擦除）。可以应用在抽象类中减少重复代码。通过extends/super限定通配符上下限。

12. **String/StringBuilder/StringBuffer**：

    - String对象不可变，StringBuffer/StringBuilder对象可变。
    - String不可变所以线程安全，StringBuffer对方法加了同步锁所以线程安全，StringBuilder非线程安全。

13. **super关键字**：每当创建子类的实例时，父类的实例被隐式创建，由 super 关键字引用变量引用。super 可以用来引用直接父类的实例变量，可以用来调用父类方法。

    调用子类构造方法之前会先调用父类的无参构造方法，目的是帮助子类做初始化工作。

14. **this关键字**：当前类对象的引用；可以在一个构造方法中通过this(params…)来调用当前类的其他构造方法，类似用super(params...)来调用父类构造方法。

15. **异常处理**：Error、Exception，都继承自Throwable。

    - RuntimeException：编译能够通过的异常，一般由程序逻辑错误引起。
    - File/IOException：必须处理的异常。必须使用try-catch捕获它，否则程序不能编译通过。

16. **字节流和字符流IO**：Blocking IO

    - 面向字节（Byte）的InputStream和OutputStream
    - 面向字符（Character）的Reader和Writer（其实也是stream）

17. **NIO(Non-blocking IO)**：

    <img src="https://lh4.googleusercontent.com/proxy/TJcB6BsxgccvnilEnwC9fp1WXrVYt21vU7o0tkzx7_8cCHJLf5ogmyArW35reXwfEMaLiA-mDICEq6D2FQ" alt="“buffer selector channel”的图片搜索结果" style="zoom:50%;" />

    - IO是面向流的处理，NIO是面向块（缓冲区）的处理。

    - 采用多路复用模型。

    - 三个核心部分组成：buffer（缓冲区）、channel（管道）、selector（选择器）。
      
      - Buffer就是一块内核内存区域，存有socket/file的原始数据，主要跟channel交互。
      - Channel是对socket/file的一层封装，方便selector对socket的管理。
      
      - Selector这个类是select/poll/epoll的外包装类。在不同的平台上，底层的实现有所不同。

    >进程之间的Socket通信相当于一种IO。当交互很多，数据很少时，我们也能使用多线程的方式处理IO，但创建和切换线程需要的资源太多，所以多路复用机制是更好的选择。
    >
    >linux中提供了select/poll/epoll来为我们实现多路复用机制。select/poll/epoll可以帮助服务端把所有的客户端socket连接管理起来，同时观察许多socket的IO事件，由此我们就可以逐个处理socket上准备好的IO事件，我们把这个轮询的过程都交给select/poll/epoll来实现。

18. **深拷贝/浅拷贝**：都需要实现 Cloneable 接口，然后重写clone()方法。

    - 浅拷贝：对基本数据类型进行值传递，对引用数据类型进行引用传递般的拷贝。

    - 深拷贝：对基本数据类型进行值传递，对引用数据类型，创建一个新的对象，并复制其内容。

19. **==/equals()**：

    - 基本数据类型：==比较的是他们的值，equals()一般被重写为值比较。
    - 引用数据类型：==和equals()比较的都是内存地址。

    > equals()默认比较内存地址，但很多类重写了equals方法，比如String、Integer把它变成了值比较。

20. **hashCode()/equals()**：我们在比较对象是否相等时，首先比较他们的hashCode()，然后才是用equals()来对他们的内存地址进行比较。所以我们将某个类的equals()重写为值比较时，同时需要重写hashCode()。

21. **Java反射机制**：当每个类被装载时，会在Java堆上自动创建一个对应的**Class类**对象（反射类），用来实例化这个类的所有对象，我们可以通过反射机制得到一个实例对象的Class类对象。可以应用在泛型，另外可以使用Class c=Class.forName("MyObject")来获得Class类对象，然后用Class类的newInstance()来创建实例对象，这种方式被应用于工厂模式。另外JDK使用反射机制实现动态代理。

    

## 2. 容器

1. Collection是集合的上级**接口**，Set和List继承自Collection。（集合只能存储引用类型）
2. Collections是集合的**工具类**，提供了一系列对集合类进行操作的静态方法，如对集合排序、最大最小等操作。
5. ArrayList线程不安全，每次扩容0.5倍；Vector线程安全，每次扩容1倍（淘汰）。
4. HashSet实际上封装了HashMap，键是要存储的元素，值是一个Object对象。LinkedHashSet/Treeset同理。
5. HashMap线程不安全；HashTable线程安全（淘汰）。
6. HashMap每次扩容两倍，为了方便对哈希值进行**&**运算，重哈希时新增的哈希值高位随机置0/1。
7. 哈希表：底层是数组 + 链表/红黑树。当链表达到一定长度时（8）变为红黑树。
8. 迭代器Iterator代替了Enumeration。Iterator线程安全，因为当一个集合被遍历的时候，它会阻止其他线程去修改集合。
9. Hashtable、Vector加锁的粒度大：直接在方法声明处使用synchronized。
10. ConcurrentHashMap、CopyOnWriteArrayList加锁粒度小：他们用各种的方式来实现线程安全。
11. List因为元素有序且允许插入重复值，可以通过随机访问/顺序访问来知道元素的位置；但Set和Map因为元素无序，为了更好的查找元素，产生了哈希表和红黑树两种实现方式。

#### List：元素有序

- ArrayList：底层用数组实现，所以查询速度快，增删速度慢。

- LinkedList：底层用双向链表实现，所以查询速度慢，增删速度快。Stack和Queue可以由LinkedList实现。

- CopyOnWriteArrayList：在写/修改的时候给原数组加锁，并复制出一个新数组，修改的操作在新数组中完成，最后把原数组指针指向新数组，解锁。

  > 只能保证数据的最终一致性，不能保证数据的实时性，因为对于COWArrayList的读-写操作，并没有对数组设读锁，只有在写操作完成后，读操作的值才会发生变化。为什么不用volatile修饰？因为这只是个对象引用，volatile并没有修饰到数组中的元素。

#### Set：元素不重复

- HashSet：基于哈希表，存储自定义类对象时需要重写hashCode()和equals()方法。
  - LinkedHashSet：基于哈希表和链表，散列到哈希表的同时使用一个链表保存次序。

- TreeSet：基于红黑树，等于现有节点的元素不存储。自定义类应实现Comparable接口。
- CopyOnWriteSet：实际上封装了CopyOnWriteArrayList。

#### Map：键值映射

- HashMap：基于哈希表，存储自定义类对象时同样需要重写hashCode()和equals()。
  - LinkedHashMap：基于哈希表和链表，使得存取有序。
- TreeMap：基于红黑树，需要比较所以自定义类需要实现Comparable接口。（有序）
- ConcurrentHashMap：基于哈希表。ConcurrentHashMap通过锁分段技术和volatile实现同步；
  - 一个ConcurrentHashMap实例中包含一个由多个Segment实例组成的数组（一个Segment就相当于一个小哈希表），一个Segment实例又包含多个桶，一个桶包含一条由多个HashEntry 对象连接起来的链表。
  - Segment类继承自ReentrantLock类，所以可以在多个不同Segment上同步进行写操作。
  - ConcurrentHashMap的value域被volatile修饰，可以保证线程的读操作可以读到最新的值，所以读操作不需要上锁。





