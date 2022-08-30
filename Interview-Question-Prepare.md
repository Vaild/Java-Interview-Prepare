[TOC]



## Java 

### Basic

####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  Lambda表达式的优缺点

- 优点：
  - 代码更加简洁，减少内部类的使用
- 缺点：
  - 可读性差
  - 不易于维护



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  抽象类和接口他们之间的区别

**共同点：**

- 不能实例化
- 可以包含抽象方法
- 都可以进行默认实现

**不同点：**

- 抽象类只能是单继承，但是接口可以多实现类似于多继承的效果
- 接口只是对类的行为进行约束，但是抽象类更像是一种代码复用，强调的是所属关系
- 接口中的成员变量只能是用public static final进行修饰的，必须有初始值不能修改，抽象类的成员变量默认是default修饰，可以在子类中定义



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  深拷贝与浅拷贝

![](https://s2.loli.net/2022/08/27/bOfK2smMd9T51DC.png)

- 浅拷贝： 类似于重新在堆上创建一个对象，但是对象当中的变量如果是基本数据类型的话复制一份，如果是引用类型的话，直接复制这个对象的地址
- 深拷贝：在堆上创建一个对象，对象中所有的变量都是重新分配新的地址，像是一种递归进去的浅拷贝。
- 引用拷贝：不同的引用指向同一个对象 

####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  Java中有哪些容器

- 主要有List、Map、Set
- List： ArrayList、LinkedList、Vector
- Map：HashMap、HashTable、LinkedHashMap、TreeMap
- Set：HashSet、TreeSet

####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  List和Map的底层是如何实现的？

- List
  - ArrayList：Object[]
  - Vector： Object[]
  - LinkedList：双向链表实现
- Map：
  - HashMap：JDK8之前是**数组+链表**的实现，因为要通过拉链法解决Hash冲突；JDK8之后采用了**数组+链表+红黑树**的实现，主要是解决当Hash冲突很多的时候，当一个链表的长度大于8的时候如果当前这个数组的长度小于64，首先进行扩容，当大于64的时候会将链表转为红黑树，使我们在进行查询的时候效率更高。
  - LinkedHashMap：继承自HashMap，底层依然是**数组+链表+红黑树**实现，除此之外增加了一个双向链表，能够保证键值对的插入顺序，可以实现类似于顺序读取
  - Hashtable：是数组+链表，主要是数组，链表还是为了解决Hash冲突而存在的

线程安全的一些容器：Vector、Hashtable、StringBuffer



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  实现多线程的方法有哪几种

- 继承Thread类，重写run()方法
- 实现Runnable接口，实现run()方法，不能返回值
- 实现Callable接口，实现call()方法，可以返回值，通过FutureTask包装，可以futureTask.get()进行获取



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  创建线程池的方式有几种

- 通过Executors工厂方法创建
  - 包含五种工厂
- 通过new ThreadPoolExecutor创建



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  线程池的优点

- 线程池是管理线程的一个容器
- 提升线程的的使用率，因为减少了创建和销毁的过程
- 线程池的伸缩性对于性能的影响是比较大的，如果我们创建太多的线程，将会浪费很多的资源，有些线程池没有被用到，如果我们销毁的线程过多的话，一旦业务增多的话，就需要重新创建线程将导致长时间的登台，性能变差。

####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />基本数据类型和包装类有什么不同？

- 基本数据类型如果不被static修饰放在虚拟机栈中，但是包装类是放在堆空间中
- 基本数据类型不能用于泛型，包装类可以用于泛型
- 包装类会有初始值null，但是基本数据类型的默认值不是null



####   <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  包装类型的缓存机制了解么？

在Java中的包装类一般使用缓存机制来提高性能，那么什么是缓存机制呢？Byte、Short、Long、Integer，直接创建数组[-128, 127]缓存数据，Boolean直接返回True、False

```
Integer i1 = 40; Integer i2 = new Integer(40); System.out.println(i1==i2);
```

> `Integer i1=40` 这一行代码会发生装箱，也就是说这行代码等价于 `Integer i1=Integer.valueOf(40)` 。因此，`i1` 直接使用的是缓存中的对象。而`Integer i2 = new Integer(40)` 会直接创建新的对象。

所以所有整型包装类的对象使用equals进行比较。



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  BigDecimal可以防止浮点精度不足。

- 小数点的位置
- 整体的精度，也就是位数
- 如果是去掉小数点在int类型的范围，可以用int返回
- 否则用BigInteger



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  ArrayList扩容机制

- 如果创建一个ArrayList，不会直接分配空间
- 当添加第一个add的时候，才会默认分配一个大小为10的数组
- 等再添加11个元素的时候，再次对数组进行扩容，扩容为原来的1.5倍，
- 是通过将创建一个新的1.5倍的数组，然后将原来的数组复制过去。



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  AIO、NIO、BIO的区别

提到上面的IO模型之前，首先是我们在用Java中的IO工具的时候完成IO操作的时候，都不是Java本身完成的，因为我们执行的时候分为用户态和内核态，当像是内存管理、文件管理、磁盘IO这种操作是内核态来完成的，所以Java只是负责给系统通知，也就是进行系统调用。

在系统层面对磁盘IO也会分为三个阶段：准备数据、数据就绪、数据拷贝阶段

> fd：file description 文件描述符

- BIO(Blocking I/O) 同步阻塞IO：一个应用发起一个磁盘IO开始，系统进入阻塞，指导数据拷贝结束，一直处于阻塞

- NIO(Non Blocking I/O)同步非阻塞：一个应用从发起磁盘IO开始，不进入阻塞而是返回error，但是他会轮询发起调用，直到他知道数据准备就绪之后，也就是在数据拷贝阶段依然会进入阻塞。

- IO多路复用：对于一个IO通道，可以同时有多个系统调用，形成一个FD的数组，发起select调用，服务器端对fd数组进行遍历，然后阻塞，一旦发现当前的数据准备好了，通知客户端发起读取数据的系统调用。通过单线程遍历的方式，完成多路复用。
- AIO(Asynchronous I/O)异步IO：就是通过回调的方式解决阻塞的问题，当用户发起IO的系统调用之后，立即给与回应，当数据准备就绪之后自动将数据拷贝出去。



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  API和SPI的区别



他们两个之间主要还是在话语权之间的区别，API是厂商实现一个统一的接口，然后调用方要根据厂商的API来写程序，SPI是厂商根据调用方的要求来实现接口，话语权在调用方。

SPI的经典案例是JDBC



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  排序算法及其时间复杂度

- 冒泡排序：$ o(n^2) $
- 插入排序
- 选择排序
- 归并排序
- 快速排序
- 堆排序



####  <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  Java中实现动态代理的两种方式，他们的原理和区别

Java实现动态代理的两种方式：JDK动态代理，CGLib动态代理

原理：

- JDK动态代理，是实现目标类的接口
- CGLib动态代理，是将目标类字节码文件加载进来之后，在字节码的基础上创建目标类的子类。

区别：

- JDK只能代理实现接口的目标类，CGLib 可以直接代理类
- 好像现在JDK动态代理的性能更好一些





#### <img src="https://img.shields.io/badge/Basic-brightgreen" style="zoom:80%;" />  谈谈对布隆过滤器的理解

---

### JVM

####  <img src="https://img.shields.io/badge/JVM-brightgreen" style="zoom:80%;" />  JVM的GC机制

&emsp;讲到GC肯定就要讲到JVM的内存结构，包含了堆空间、方法区以及线程隔离的虚拟机栈、本地方法栈、程序计数器，而GC发生最频繁的地方是在堆空间中。我们对于GC的了解无非就是他在什么时候、在什么地方、对谁、做了什么事情。

简短的：

- 在什么时候：发现Eden区满了进行MinorGC；老年代不足以容纳晋升到老年代的对象的时候进行Full GC；强制Full GC、GC与非GC的时间之比超过了某个时间引发了OOM，
- 在什么地方：主要是在堆空间中
- 对什么东西：对GC root出发搜索不到的的对象，且他们经过第一次标记清理之后都没有复活的对象。
- 做了什么事情：新生代做了复制清理，老年代的标记清理，通过具体的垃圾回收器进行说明。



##### 堆空间的结构

&emsp;在垃圾回收的过程中，因为现在的垃圾收集器都是用的分代垃圾回收算法，所以我们需要对堆空间进行划分

- 新生代
  - Eden
  - Survive0
  - Survive1
- 老年代
- 永久代/元空间

![](https://javaguide.cn/assets/hotspot-heap-structure.41533631.png)



&emsp;垃圾回收主要做的就是内存回收，那么他什么时候会回收呢，应该是分配的时候出问题了，才会想到回收。所以这里先讲一下分配。

- 对象优先在Eden区进行分配
- 如果发现Eden区中的空间不足以分配给现在的对象，那么虚拟机将做一个Minor GC对Eden区域进行一个垃圾回收，将Eden区域的对象放到survivor中，如果GC期间发现Survivor中也没有足够的空间，则使用**空间分配担保**直接把Eden中的对象直接复制到老年代中，如果老年代还可以放下的话，就不会在进行Full GC。经过Minor GC之后，Eden可以放下新对象后，直接在Eden区域分配内存空间。
- 对于年龄的增加，如果在Eden中经过MinorGC之后，没有被回收，年龄增加1，从Eden中转移到Survivor，在survivor中每经过一次Minor GC年龄就会增加1.
- 如果一个新请求分配的对象很大，在Eden区放不下的话，可以直接放到老年代，大对象为了防止复制带来的效率损失，也直接放到老年代
- 长期存活的将进入到老年代，那么如何来定义这个长期存活呢，我们在里面使用的是通过每经历一次MinorGC 就让他们的年龄 + 1，当JVM遍历所有的对象，当发现某个年龄大小的综合超过了survivor的50%的时候，就取这个值作为门槛，将他们从新生代晋升到老年代。

> 空间分配担保：确保在Minor GC之前，老年具有能容纳所有的新生代对象的空间。



##### 主要进行垃圾的区域

- Minor GC：
  - Young GC： Eden，只有Eden满的时候触发，
  - Old GC：Old gen
  - mix
- Full GC/ Major GC：收集整个堆，当准备出发 young GC之前，发现之前平均晋升的大小比老年代的剩余空间还大，就需要进行Full GC



##### 对谁进行垃圾回收？

&emsp;最早有计数引用的方法，分析当前的对象是不是还有人引用他，如果没有的话就对其进行回收，但是他不能解决循环引用的问题。

&emsp;可达性分析算法：基本思想通过一个GC Root对象作为起点，分析他的引用链，只要是在引用链上的说明目前正在被使用，如果一个对象没有没有在任何一个引用链上的话说明这个对象现在不可用，可以被回收。

**GC Root：**

- 虚拟机栈中的引用对象
- 本地方法栈中的对象
- 方法区中的静态引用对象
- 方法去的常量
- 获取同步锁的对象



&emsp;如果一个对象可以被回收，他也不一定被回收，因为要宣告一个对象的死亡，至少要经过要经历两次标记；在经过可达性分析之后，不可达对象被第一次标记并且进行筛选，没有必要死亡对象，将会放到一个队列当中进行第二次标记，如果第二次标记依然没有出现在任何一个引用链上，就直接回收。



##### GC算法

- 标记-清除：这个是最早的一种垃圾回收算法，他要求将不需要回收的空间标记出来，然后将需要回收的垃圾全部回收掉
  - 对于有很多需要回收的时候，效率很低
  - 会产生大量的空间碎片
- 标记-复制：对标记清除的改进，每次只使用一般的空间，对不需要清除的空间进行标记，将标记的空间按顺序复制到另外一半的空间，直接将刚才的一半空间全部回收
- 标记-整理：一般用在老年代，对不回收的空间进行标记，将标记的空间顺序的从开始整理好，将剩余的空间全部回收掉。
- 分代收集算法：没有新的垃圾回收算法，是针对不同的结构具有不同的特点选择合适的垃圾回收算法
  - 在新生代中，对象的存活率不高，所以每次回收都有很多需要回收，如果使用标记清除的话，每次清除很浪费时间，所以使用标记复制的算法更好，因为存活率低所以标记复制的也比较少。
  - 在老年代中，对象的存活率较高，每次回收的不多，所以使用标记清除或者是标记整理更好一些。

##### 垃圾收集器

- Serial收集器：阻塞单线程垃圾收集器，新生代使用标记复制，老年代使用标记整理算法

![](https://javaguide.cn/assets/46873026.3a9311ec.png)

- ParNew收集器：阻塞多线程并行垃圾收集器，新生代使用标记复制，老年代使用标记整理算法

![](https://javaguide.cn/assets/22018368.df835851.png)

- Parallel scavenge垃圾收集器：关注于CPU的吞吐量，关心能不能高效的利用CPU，不关心用户体验

- Serial Old垃圾收集器：是Secial的老年代版本，单线程，使用标记整理算法

- Parallel Old： Parallel Scavenge的老年代版本，标记整理

- CMS(Concurrent Mark Sweep)：并行标记整理收集器，我们前面的垃圾回收器都会有很长时间的停顿时间，CMS是追求尽可能短的停顿时间, 主要优点是：并发收集，低停顿

  - 初始标记：初始单线程阻塞标记，很快
  - 并发标记：非阻塞标记
  - 重新标记：阻塞多线程标记，因为程序在运行的过程中，他的可达性关系会发生变化。
  - 并发清除：非阻塞清除

  ![](https://javaguide.cn/assets/CMS%E6%94%B6%E9%9B%86%E5%99%A8.8a4d0487.png)





####  <img src="https://img.shields.io/badge/JVM-brightgreen" style="zoom:80%;" />  如何判断一个无用的类？

- 该类的所有的实例已经被回收了
- ClassLoader已经被回收了
- Class对象没有在任何地方进行引用

&emsp;则这个无用类可以被回收，但不一定被回收。



####  <img src="https://img.shields.io/badge/JVM-brightgreen" style="zoom:80%;" />  如何判断一个常量是不是已经废弃？

&emsp;如果没有任何的对象引用的话，则说明这个常量已经被废弃了，可以直接被回收。





####   <img src="https://img.shields.io/badge/JVM-brightgreen" style="zoom:80%;" />  双亲委派机制

##### 什么是双亲委派机制

我们知道JVM在加载类的时候，需要通过类加载器来进行加载，那么使用哪个类加载器来加载，就需要用到双亲委派机制，首先了解有哪些类加载器：

- BootStrap ClassLoader：主要用来加载 Java home目录下lib中核心包的类
- Extension ClassLoader：主要用来加载Java home目录下lib中ext中jar包中的类
- Application ClassLoader：主要用来加载classPath下面的类
- User ClassLoader：主要用来加载自定义的类加载器的类

**双亲委派机制就是当我们加载一个类的时候首先交给他的父加载器中取加载，只有当父加载器加载不了当前类的时候才会使用当前的加载器来进行加载。**



##### 为什么需要双亲委派机制，如果没有会怎么样？

- 双亲委派可以避免重复加载，就是如果一个类已经被父加载器加载那么当前的加载器就不会再次进行加载
- 可以防止第三方包篡改JDK本身的包，保证安全性，如果没有双亲委派机制的话，那么我们可以通过自定义核心类去加载达到村该核心类的目的，但是现在只能有BootStrap加载器进行加载。

##### 双亲委派机制如何实现的

- 判断当前的类是不是已经被加载了，如果被加载直接退出
- 如果父加载器不为空的话，则向上由父加载器进行类加载；如果为空则说明当前已经是BootStrap类加载器，尝试由他进行类加载，如果加载失败抛出ClassLoaderNotFound
- 上一步类加载完之后，Class类对象依然为空，说明没有加载，则有当前类加载器的findClass进行加载

##### loadClass、find Class、defineClass的区别

- loadClass是类加载，实现双亲委派的主要代码
- findClass 是通过名称和位置来进行类加载，如果想定义一个加载器，但是不想改变双亲委派的话，可以继承classLoad然后重写findClass
- defineClass将字节码转为Class

##### 如何破坏双亲委派？

修改loadClass，因为主要的双亲委派的逻辑都在这个里面。JDBC和Tomcat就是破坏了双亲委派机制

##### JDBC为什么破坏双亲委派机制？

- JDBC使用的是一种SPI的方式，他在获取链接的时候会通过DriverManager，会加载这个类

  ````java
  ServiceLoader<Driver> loadedDrivers = ServiceLoader.load(Driver.class);
  ````

- 然后会尝试加载所有classpath下面，实现Driver接口的的实现类

- DriverManager通过根加载器加载，但是实现类需要Application进行加载不能通过根加载器进行加载，所以需要破坏

- 通过ThreadContextClassLoader线程上下文类加载器，用于加载classpath下面的实现类

##### Tomcat为什么破坏？

- 和双亲委派机制正好相反的机制进行加载
- tomcat是一个web容器，他很有可能运行很多个web应用，但是这些web应用的版本可能不一样但是全路径名是一样的，所以就不能重复加载
- tomcat需要破坏双亲委派机制，提供隔离机制，为每个web应用提供一个单独的WebAppClassLoader
- 每个类加载器，优先记载本身目录下的class文件，加载不到的时候再向上加载

>  [3]. [你确定你真的理解"双亲委派"了吗？！ ](https://www.cnblogs.com/hollischuang/p/14260801.html)





## Computer Basic

### Network

####  <img src="https://img.shields.io/badge/Network-brightgreen" style="zoom:80%;" />  TCP和UDP之间的异同

- 是否面向连接：UDP在传输数据之前是不需要建立连接的；TCP是面向连接的，在数据传输之前需要先建立连接，等数据传输完成之后再关闭连接
- 是否是可靠传输：UDP是不可靠传输的，不管收到与否都不给出任何的确认，不保证数据不丢失，不保证能顺利到达；TCP是可靠传输的，再数据传递之前先建立三次握手建立连接，通过滑动窗口，确认、重传、拥塞控制机制，来保证数据可以无差错、不丢失、不重复、按序送到。
- 是否有状态：UDP是没有状态的，只管发送，不管发送之后的事儿；TCP是有状态的，他知道自己发送的东西是不是被接受了，是不是送到了
- 传输效率：因为它不需要保证数据可靠送达，所以他的效率要比TCP高很多。主要用于一些实时性要求比较高的场景，比如直播。
- 能否提供广播：UDP支持广播，比如直播，但是TCP是不支持广播的。

####  <img src="https://img.shields.io/badge/Network-brightgreen" style="zoom:80%;" />  TCP的三次握手和四次挥手

##### 三次握手

- 建立TCP连接需要三次握手
- 客户端向服务器发送带有SYN(x)标志的数据包，客户端进入syn_send，等待服务器的确认
- 服务器向客户端发送带有SYN-ACK(x+1, y)标志的数据包，服务器进入到SYN_recv，
- 客户端向服务器发送带有ACK(y+1)的数据包，客户端和服务器进入到established的状态，完成握手

##### 为什么三次握手？

- 首先要了解进行三次握手的目的：
  - 第一次是client只是单独的请求，server收到之后，可以确定client的发送正，server接收正常
  - 第二次是client知道client发送正常、接收正常，server发送正常、接收正常；server确定了client发送正常，server接收正常
  - 第三次client确认client发送正常、接收正常、server发送正常、接收正常；server确定了client发送正常、接收正常、server发送正常、接收正常
- 如果没有第三次握手的话，server能确定client的接收是否正常、自己的发送是否正常
- 另外，如果没有第三次握手的话，再第一次发起TCP连接的数据，可能发生了延迟，但是现在已经不需要建立连接了，但是原本发起的数据传到了sever，server就会给响应，如果是两次的话，连接已经建立没有什么要发的东西，那么就会导致资源的浪费。

##### 四次挥手

- 断开TCP的连接需要四次挥手
- 第一次挥手，客户端给服务器发送FIN(x)的数据包，说明客户端没有东西向服务器端传送了，关闭了从客户端向服务器数据传输，客户端进入FIN_wait
- 第二次，服务器给客户端发送ACK(x+1)数据包，表明server知道了客户端不发东西了，服务器进入close_wait，客户端进入到FIN_wait2
- 第三次，服务器给客户端发送FIN(y)数据包，服务器没有什么给客户端的东西了，关闭了服务器到客户端的连接，进入到Last ack状态
- 第四次，客户端给服务器发送ACK(y+1)，客户端进入TIME_wait 状态，服务器收到ACK之后会进入到close状态。如果客户端再两个最长报文段寿命时间内没有收到回复，进入到close，否则重传。





##### 为什么要进行四次挥手？

- 通俗解释一个四次挥手的目的
  - 第一次，我没啥东西了，这边连接关了吧
  - 第二次，我知道了这边链接关了
  - 第三次，服务器也没啥东西了，关了吧
  - 第四次，我知道了。
- 因为中间服务器可能还要有东西给客户端，连接依然存在，所以不能将FIN和ACK合并



##### 为什么要等待2MSL时间进入close状态？

- 第四次挥手的时候，ACK可能丢失，这个时候服务器会重新发送FIN，ACK会再次从客户端发送
- 如果直接关闭的话，那这个连接可能就永远都关不了了
- 另外也可以防止，三次握手时候，早就出现的连接请求的问题，导致意外建立连接。





#### <img src="https://img.shields.io/badge/Network-brightgreen" style="zoom:80%;" />  在建立TCP连接之后，客户端突然挂掉的流程

会出现两种情况：

- 客户端回来：
  - 客户端向服务器发送SYN报文，会得到不是期望序列的ACK
  - 客户端返回RST，服务器收到之后，释放连接
- 客户端不回来：
  - 客户端的端口变化了，发送SYN重新建立新的连接
  - 如果彻底没回来的话，根据linux本身的策略，当前的连接不会释放
  - 我们可以自己实现释放策略，比如通过Keep alive，如果多久没有收到，自动释放连接



## Framework

### Spring

#### <img src="https://img.shields.io/badge/Spring-brightgreen" style="zoom:80%;" />  SpringBoot自动装配流程

下面有讲

####  <img src="https://img.shields.io/badge/Spring-brightgreen" style="zoom:80%;" />  SpringBoot启动流程

SpringBoot启动分为了两个部分，对应于main方法中的两个部分：

- 实例化一个SpringApplication对象：主要是对一些监视器，环境上下文进行初始化

  - servlet类型环境
  - 通过getSpringFactoryInstances初始化META/spring.factories配置中的Initializer
  - 加载所有的Listener

- 通过SpringApplication执行run()方法：扫描配置，加载IoC容器，启动监听模块，创建上下文环境、自动装配以及自动注入

  run方法分为好几个模块，下面分开介绍：

  - ~~获取监视器、启动监视器~~

  - 获取配置文件，初始化environment环境

  - 初始化应用上下文，初始化IoC容器

  - prepareContext方法：初始化读取bean的读取器，然后将目前有的Initializer等放进beanfactory   IoC容器中

  - IoC容器的初始化，refresh()

    postProcessBeanFactory()方法向上下文中添加了一系列的Bean的后置处理器。后置处理器工作的时机是在所有的beanDenifition加载完成之后，bean实例化之前执行。

    **这其中还给上下文添加了环境变量、系统配置、系统环境信息**

    其中最重要的就是， 调用**invokeBeanFactoryPostProcessors(beanFactory);**，也就是IoC初始化的步骤

    - BeanDefinition的Resource定位
    - BeanDefinition的载入: 定位完成之后，会将他们拼接起来形成路径，然后根据路径加载进来，之后判断是不是带有 `@Component`注解，如果有的话就是要载入的BeanDefinition
    - 向IoC容器注册BeanDefinition：加入到一个ConcurrentHashMap中
    - 在出口处有一个自动装配的入口：
      - 主要对SpringBootApplication注解中的@EnableConfiguration中的@Import  AutoConfigurationImportSelector类
      - getCandidateConfigurations他会获取META-INFO/spring.factories中所有的key为org.springframework.boot.autoconfigure.EnableAutoConfiguration的类
      - 排除掉一些不是oncondition的之后，通过获取resources, 载入beandefinition，最后将其注册到IoC容器中



对象实例化代码：

````java
public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {
    /**
    * 这里还有一部分代码没有什么用，就删除了
    */
	// 推断Application的类型，这里一般是Servlet
    this.webApplicationType = WebApplicationType.deduceFromClasspath();
    // 加载根初始化器
    this.bootstrapRegistryInitializers = new ArrayList(this.getSpringFactoriesInstances(BootstrapRegistryInitializer.class));
    // 在META-INFO/spring.factories配置中找到Initializer并生成一个对象 初始化
    this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));
    // 同上，是Listener
    this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
    this.mainApplicationClass = this.deduceMainApplicationClass();
}
````

run的源代码如下：

````java
public ConfigurableApplicationContext run(String... args) {
    long startTime = System.nanoTime();
    // 
    DefaultBootstrapContext bootstrapContext = this.createBootstrapContext();
    // spring上下文
    ConfigurableApplicationContext context = null;
    this.configureHeadlessProperty();
    // 获取监听器
    SpringApplicationRunListeners listeners = this.getRunListeners(args);
    // 启动监视器
    listeners.starting(bootstrapContext, this.mainApplicationClass);

    try {
        // 获取命令行的参数
        ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
        // 这个是环境，根据用户配置加载环境，这个非常重要，在这里面加载了yaml文件，进行配置
        ConfigurableEnvironment environment = this.prepareEnvironment(listeners, bootstrapContext, applicationArguments);
        this.configureIgnoreBeanInfo(environment);
        Banner printedBanner = this.printBanner(environment);
        // 初始化应用上下文，同时在这里面创建了IoC容器，DefalutListableBeanFactory
        context = this.createApplicationContext();
        context.setApplicationStartup(this.applicationStartup);
        // 准备上下文，执行容器中目前所有的Initializer
        // 创建了读取bean的读取器
        // 将命令行参数封装成单例Bean装进容器
        // 对现在容器中的事件进行发布
        this.prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
        // 刷新上下文，最重要的IoC初始化，refresh
        // 进行了IoC初始化，以及自动装配
        this.refreshContext(context);
        this.afterRefresh(context, applicationArguments);
        Duration timeTakenToStartup = Duration.ofNanos(System.nanoTime() - startTime);
        if (this.logStartupInfo) {
            (new StartupInfoLogger(this.mainApplicationClass)).logStarted(this.getApplicationLog(), timeTakenToStartup);
        }

        listeners.started(context, timeTakenToStartup);
        this.callRunners(context, applicationArguments);
    } catch (Throwable var12) {
        this.handleRunFailure(context, var12, listeners);
        throw new IllegalStateException(var12);
    }

    try {
        Duration timeTakenToReady = Duration.ofNanos(System.nanoTime() - startTime);
        listeners.ready(context, timeTakenToReady);
        return context;
    } catch (Throwable var11) {
        this.handleRunFailure(context, var11, (SpringApplicationRunListeners)null);
        throw new IllegalStateException(var11);
    }
}
````

> [4]. [SpringBoot启动流程分析 1 - 5](https://www.cnblogs.com/hello-shf/p/10976646.html)



#### <img src="https://img.shields.io/badge/Spring-brightgreen" style="zoom:80%;" />  为什么service层开启事务之后，不生效？

在使用Transactional注解之后，Spring就会通过动态代理的方式增强方法的功能，但是他在代理的过程中会发现不能对private的方法进行代理

- allowPublicMethodsOnly()
- AopUtils

> [5]. [为什么private方法加了@Transactional，事务也没有生效？](https://juejin.cn/post/7034042574146895902)

####  <img src="https://img.shields.io/badge/Spring-brightgreen" style="zoom:80%;" />  Spring中的声明式事务是如何实现的

声明式事务，也就是通过`@Transactional`注解对service层的方法声明事务，是使用AOP的思路实现的，对原有方法的增强，在原有方法开始之前实现开启事务，在方法完成之后进行事务的提交或回滚。



####  <img src="https://img.shields.io/badge/Spring-brightgreen" style="zoom:80%;" />  Spring请求处理

- 请求进入DispatcherServlet类，doService执行doDispatch
- 通过reques对象获得handler对象，遍历handlerMapping找到可以处理当前request的handler
- 通过handler找到合适的适配器，handlerAdapter
- 执行handler，就是进去执行controller方法的以及生成ModelAndView的过程

### MyBatis

####  <img src="https://img.shields.io/badge/MyBatis-brightgreen" style="zoom:80%;" />  MyBatis中的$和#有什么区别？

- 首先这两个都是MyBatis中进行拼接数据的符号

- #使用了`PreparedStatement`进行了预编译，使用? 代替，按序给 sql 的? 号占位符设置参数值，可以有效的防止sql注入。
- 但是$符号是直接使用字符串拼接，可能会导致sql注入的问题

####  <img src="https://img.shields.io/badge/MyBatis-brightgreen" style="zoom:80%;" />  MyBatis缓存机制

- MyBatis中包含了查询缓存机制，用于提高查询效率， 包括了一级缓存和二级缓存
- 默认情况下只有一级缓存，也就是sqlSession级别的缓存开启
  - 本地缓存，作用在sqlSession当session被关闭之后，缓存被清空
  - 不能关闭
  - 保存在一个Map中
  - 失效的情况：
    - 不同的SqlSession
    - 查询条件不同
    - 两次查询中间对表增加或者删除、更新操作
- 二级缓存，全局作用域缓存，需要手动开启
- 缓存回收机制：
  - LRU
  - FIFO



## DB

### MySQL

####  <img src="https://img.shields.io/badge/MySQL-brightgreen" style="zoom:80%;" />  如何提高MySQL的查询效率

- 避免全表查询，对where、order by涉及的列建立索引
- 避免where进行null判断，会导致引擎放弃索引进行全表扫描
- 避免在where子句中使用or连接，会导致引擎放弃索引





## Item

####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" />  介绍一下你的项目

&emsp; 我的项目是一个在线音乐服务平台，只不过是在内网内部服务，这个项目包括音乐、用户认证、webSocket聊天模块，在音乐相关的模块中包含了歌曲上传、下载、歌单创建、收藏、点赞、评论等功能。



####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" />  讲一下你项目的技术栈

&emsp;我的项目是通过SpringBoot进行构建的，在数据端使用MySQL进行数据存取，另外在用户认证方面使用的JWToken的认证方式，采用这种方式有一个缺点是一旦Token生成他的过期时间就是一个确定的值，这会给我们的退出造成一定的困扰，所以使用Redis在服务器端保存Token及对应的用户信息，退出的时候直接从redis中删除。



####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" /> 讲一下你项目的登录模块吧

- 首先，登录的功能是通过JWT实现的，也就是Json Web Token，它本质上是一个Token，我们把前端的Username、password给到后端之后，服务器通过这个获取到用户信息、权限信息，生成token返回到前端，存储在local storage当中，之后每次访问都会携带。
- 登录使用的拦截器的形式，定义一个HandlerInterceptor、通过webconfig配置到springboot中，同时我们还会验证当前请求的uri需要什么权限和当前用户权限本身作比较，如果符合就放行，不符合就不通过。

####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" />  简单介绍一下webSocket协议

- 首先，websocket也是一种通信协议
- 在了解websocket之前，我们先了解一下在websocket使用之前，我们是怎么从服务器端获取消息的
  - Ajax轮询的方式，通过定时发送大量的HTTP包去询问有没有新的消息
  - 采用Long Polling的方式，采用阻塞的方式，在没有消息来之前不响应
  - 以上两种方式，都比较占用资源，所以有了websocket
- websocket本身也是需要使用HTTP报文进行连接
- 在建立连接之后，客户端和服务器之间实现了全双工通信，之后的通信不需要在发送任何的请求
- 在调试的过程中，可以看到建立通信之后网络中不会再出现其他的包，而可以继续通信，个人认为websocket的通信就像是建立了一个隧道，当服务器收到一个消息之后，就会使用回调函数给所有的客户端广播刚才收到的信息



####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" />  简单介绍一下ThreadLocal的应用

&emsp;在我这个项目中，主要使用ThreadLocal做线程隔离，同时也能保证在本线程内的局部变量不再需要需要每次都要进行解析。

- 因为我们的token发送到后端之后，需要进行解析出来用户的信息，但是因为token很多的请求都会携带，这样的话我们每次在进行其他操作的时候，他都会去进行解析
- 使用ThreadLocal，将解析的用户的信息set进ThreadLocalMap中，这样的话，在当前线程中就不需要再对其信息进行解析，只需要将从Map中 直接get出来就可以了
- 这样既起到线程隔离的作用，同时也减轻了服务器的压力

**ThreadLocal如果我们在用完之后不主动进行remove的话，很容易导致内存泄露，为什么会这样呢？**

- 每个线程都有一个ThreadLocalMap的实例，这个map中的key：this，也就是ThreadLocal对象本身的引用，value：set进去的值。那为什么不使用线程本身作为map的key呢？主要是一个线程本身可以有多个局部变量，那么如果是使用线程引用就会被覆盖，所以使用ThreadLocal对象的引用作为key。

  ````java
  public void set(T value) {
      Thread t = Thread.currentThread();
      ThreadLocalMap map = getMap(t);
      if (map != null)
          map.set(this, value);
      else
          createMap(t, value);
  }
  ````

- map中key对ThreadLocal的引用是弱引用，value的引用是强引用，map对entry也是强引用。

  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/47ef8e97d7aa457cb8e57920ef2fbe9f~tplv-k3u1fbpfcp-zoom-in-crop-mark:3024:0:0:0.awebp)

- 关于内存泄漏

  - 比较流行的：因为map的key是弱引用，所以ThreadLocal很容易被回收，所以一旦被回收之后，key指向null，无法找到value之后就无法删除当前的entry，如果不手动remove的话，就会产生内存溢出的问题。

    > 实际上，开发者也想到了这个问题，所以对于这种情况做了预防，在get、set、remove方法中，会清除map中key为null的value。

    ````java
    if (k == null) {
        replaceStaleEntry(key, value, i);
        return;
    }
    ````

  - 实际上：ThreadLocal的对象有threadLocal的变量引用着，所以根本不会被回收，所以产生内存泄露的主要的问题还是我们一直往里面放东西，但是从来都没有删除过。即使我们将value手动设置为null也不能改变这个事实。

  - 我们使用弱引用也是为ThreadLocal更有效的防止内存泄漏

> [2]. [面试必备：ThreadLocal详解](https://juejin.cn/post/7126708538440679460)



####  <img src="https://img.shields.io/badge/Item-brightgreen" style="zoom:80%;" />  权限认证是如何实现的

- 主要通过RBAC的模型实现的权限认证，大概是这样
  - 我给每个用户都分配一个或多个角色
  - 每个角色都有一个或多个资源访问的权限
- 首先，通过token认证取得用户认证的信息，同时获取到用户的权限列表，同时获取当前资源可以允许访问的role列表，进行比对
- 如果发现当前role在资源的允许访问的列表中，就直接return
- 如果不行，向前端返回，权限不足。





### 未完成

- [ ] mysql数据库去重
- [ ] mysql[数据](https://www.nowcoder.com/jump/super-jump/word?word=数据)库从第10个[数据](https://www.nowcoder.com/jump/super-jump/word?word=数据)往后查10个

````sql
select * from table where id > 10 limit 10;
````

- [ ] mysql事务的隔离级别

- [ ] Spring中的设计模式

- [ ] JDBC的存储的流程

- [ ] MyBatis如何连接数据库
  - 存在一个sqlSession的工厂类，用来获取session



**2022/08/29 待完成**

- [x] SpringBoot自动装配
- [x] SpringBoot启动流程
- [x] Java中实现动态代理的两种方式，他们的原理和区别
- [x] spring的底层实现原理，在service层的一个方法上加spring注解后，controller是怎么调用这个方法的
- [x] 在service层的一个private方法上加上事务的注解，然后用controller去调用service层的方法，事务会开启吗
- [x] 两道leetcode
- [x] 排序算法，以及事件复杂度





### 参考文章:

[1]. [JavaGuide](javaguide.cn)

[2]. [面试必备：ThreadLocal详解](https://juejin.cn/post/7126708538440679460)

[3]. [你确定你真的理解"双亲委派"了吗？！ ](https://www.cnblogs.com/hollischuang/p/14260801.html)

[4]. [SpringBoot启动流程分析 1 - 5](https://www.cnblogs.com/hello-shf/p/10976646.html)

[5]. [为什么private方法加了@Transactional，事务也没有生效？](https://juejin.cn/post/7034042574146895902)