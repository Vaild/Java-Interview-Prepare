# 

#### **介绍一下你的项目**

&emsp 我的项目是一个在线音乐服务平台，只不过是在内网内部服务，这个项目包括音乐、用户认证、webSocket聊天模块，在音乐相关的模块中包含了歌曲上传、下载、歌单创建、收藏、点赞、评论等功能。



#### **讲一下你项目的技术栈**

​	我的项目是通过SpringBoot进行构建的，在数据端使用MySQL进行数据存取，另外在用户认证方面使用的JWToken的认证方式，采用这种方式有一个缺点是一旦Token生成他的过期时间就是一个确定的值，这会给我们的退出造成一定的困扰，所以使用Redis在服务器端保存Token及对应的用户信息，退出的时候直接从redis中删除。





#### **简单介绍一下webSocket协议**

- 首先，websocket也是一种通信协议
- 在了解websocket之前，我们先了解一下在websocket使用之前，我们是怎么从服务器端获取消息的
  - Ajax轮询的方式，通过定时发送大量的HTTP包去询问有没有新的消息
  - 采用Long Polling的方式，采用阻塞的方式，在没有消息来之前不响应
  - 以上两种方式，都比较占用资源，所以有了websocket
- websocket本身也是需要使用HTTP报文进行连接
- 在建立连接之后，客户端和服务器之间实现了全双工通信，之后的通信不需要在发送任何的请求
- 在调试的过程中，可以看到建立通信之后网络中不会再出现其他的包，而可以继续通信，个人认为websocket的通信就像是建立了一个隧道，当服务器收到一个消息之后，就会使用回调函数给所有的客户端广播刚才收到的信息



#### 简单介绍一下ThreadLocal的应用

​	在我这个项目中，主要使用ThreadLocal做线程隔离，同时也能保证在本线程内的局部变量不再需要需要每次都要进行解析。





#### 权限认证是如何实现的

- 主要通过RBAC的模型实现的权限认证，大概是这样
  - 我给每个用户都分配一个或多个角色
  - 每个角色都有一个或多个资源访问的权限
- 首先，通过token认证取得用户认证的信息，同时获取到用户的权限列表，同时获取当前资源可以允许访问的role列表，进行比对
- 如果发现当前role在资源的允许访问的列表中，就直接return
- 如果不行，向前端返回，权限不足。





#### ![](https://img.shields.io/badge/Java-Network-brightgreen)TCP和UDP之间的异同

- 是否面向连接：UDP在传输数据之前是不需要建立连接的；TCP是面向连接的，在数据传输之前需要先建立连接，等数据传输完成之后再关闭连接
- 是否是可靠传输：UDP是不可靠传输的，不管收到与否都不给出任何的确认，不保证数据不丢失，不保证能顺利到达；TCP是可靠传输的，再数据传递之前先建立三次握手建立连接，通过滑动窗口，确认、重传、拥塞控制机制，来保证数据可以无差错、不丢失、不重复、按序送到。
- 是否有状态：UDP是没有状态的，只管发送，不管发送之后的事儿；TCP是有状态的，他知道自己发送的东西是不是被接受了，是不是送到了
- 传输效率：因为它不需要保证数据可靠送达，所以他的效率要比TCP高很多。主要用于一些实时性要求比较高的场景，比如直播。
- 能否提供广播：UDP支持广播，比如直播，但是TCP是不支持广播的。

#### ![](https://img.shields.io/badge/Java-Network-brightgreen)TCP的三次握手和四次挥手

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







#### ![](https://img.shields.io/badge/Java-MySQL-brightgreen)如何提高MySQL的查询效率

- 避免全表查询，对where、order by涉及的列建立索引

- 避免where进行null判断，会导致引擎放弃索引进行全表扫描

- 避免在where子句中使用or连接，会导致引擎放弃索引

  

#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)Lambda表达式的优缺点

- 优点：
  - 代码更加简洁，减少内部类的使用
- 缺点：
  - 可读性差
  - 不易于维护



#### ![](https://img.shields.io/badge/Java-MyBatis-brightgreen)MyBatis中的$和#有什么区别？

- 首先这两个都是MyBatis中进行拼接数据的符号

- #使用了`PreparedStatement`进行了预编译，使用? 代替，按序给 sql 的? 号占位符设置参数值，可以有效的防止sql注入。
- 但是$符号是直接使用字符串拼接，可能会导致sql注入的问题

#### ![](https://img.shields.io/badge/Java-MyBatis-brightgreen)MyBatis缓存机制

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



#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)抽象类和接口他们之间的区别

**共同点：**

- 不能实例化
- 可以包含抽象方法
- 都可以进行默认实现

**不同点：**

- 抽象类只能是单继承，但是接口可以多实现类似于多继承的效果
- 接口只是对类的行为进行约束，但是抽象类更像是一种代码复用，强调的是所属关系
- 接口中的成员变量只能是用public static final进行修饰的，必须有初始值不能修改，抽象类的成员变量默认是default修饰，可以在子类中定义



#### 深拷贝与浅拷贝

![](https://guide-blog-images.oss-cn-shenzhen.aliyuncs.com/github/javaguide/java/basis/shallow&deep-copy.png)

- 浅拷贝： 类似于重新在堆上创建一个对象，但是对象当中的变量如果是基本数据类型的话复制一份，如果是引用类型的话，直接复制这个对象的地址
- 深拷贝：在堆上创建一个对象，对象中所有的变量都是重新分配新的地址，像是一种递归进去的浅拷贝。
- 引用拷贝：不同的引用指向同一个对象 

#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)Java中有哪些容器

- 主要有List、Map、Set
- List： ArrayList、LinkedList、Vector
- Map：HashMap、HashTable、LinkedHashMap、TreeMap
- Set：HashSet、TreeSet

#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)List和Map的底层是如何实现的？

- List
  - ArrayList：Object[]
  - Vector： Object[]
  - LinkedList：双向链表实现
- Map：
  - HashMap：JDK8之前是**数组+链表**的实现，因为要通过拉链法解决Hash冲突；JDK8之后采用了**数组+链表+红黑树**的实现，主要是解决当Hash冲突很多的时候，当一个链表的长度大于8的时候如果当前这个数组的长度小于64，首先进行扩容，当大于64的时候会将链表转为红黑树，使我们在进行查询的时候效率更高。
  - LinkedHashMap：继承自HashMap，底层依然是**数组+链表+红黑树**实现，除此之外增加了一个双向链表，能够保证键值对的插入顺序，可以实现类似于顺序读取
  - Hashtable：是数组+链表，主要是数组，链表还是为了解决Hash冲突而存在的

线程安全的一些容器：Vector、Hashtable、StringBuffer



#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)实现多线程的方法有哪几种

- 继承Thread类，重写run()方法
- 实现Runnable接口，实现run()方法，不能返回值
- 实现Callable接口，实现call()方法，可以返回值，通过FutureTask包装，可以futureTask.get()进行获取



#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)创建线程池的方式有几种

- 通过Executors工厂方法创建
  - 包含五种工厂
- 通过new ThreadPoolExecutor创建



#### ![](https://img.shields.io/badge/Java-Basic-brightgreen)线程池的优点

- 线程池是管理线程的一个容器
- 提升线程的的使用率，因为减少了创建和销毁的过程
- 线程池的伸缩性对于性能的影响是比较大的，如果我们创建太多的线程，将会浪费很多的资源，有些线程池没有被用到，如果我们销毁的线程过多的话，一旦业务增多的话，就需要重新创建线程将导致长时间的登台，性能变差。





#### ![](https://img.shields.io/badge/Java-JVM-brightgreen)JVM的GC机制

​	讲到GC肯定就要讲到JVM的内存结构，包含了堆空间、方法区以及线程隔离的虚拟机栈、本地方法栈、程序计数器，而GC发生最频繁的地方是在堆空间中。我们对于GC的了解无非就是他在什么时候、在什么地方、对谁、做了什么事情。

简短的：

- 在什么时候：发现Eden区满了进行MinorGC；老年代不足以容纳晋升到老年代的对象的时候进行Full GC；强制Full GC、GC与非GC的时间之比超过了某个时间引发了OOM，
- 在什么地方：主要是在堆空间中
- 对什么东西：对GC root出发搜索不到的的对象，且他们经过第一次标记清理之后都没有复活的对象。
- 做了什么事情：新生代做了复制清理，老年代的标记清理，通过具体的垃圾回收器进行说明。



##### 堆空间的结构

在垃圾回收的过程中，因为现在的垃圾收集器都是用的分代垃圾回收算法，所以我们需要对堆空间进行划分

- 新生代
  - Eden
  - Survive0
  - Survive1
- 老年代
- 永久代/元空间

![](https://javaguide.cn/assets/hotspot-heap-structure.41533631.png)



​	垃圾回收主要做的就是内存回收，那么他什么时候会回收呢，应该是分配的时候出问题了，才会想到回收。所以这里先讲一下分配。

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





##### GC算法

- 标记-清除
- 标记-复制
- 标记-整理
- 分代收集算法



#### 包装类型的缓存机制了解么？





#### 双亲委派机制



### 未完成

#### mysql数据库去重

#### mysql[数据](https://www.nowcoder.com/jump/super-jump/word?word=数据)库从第10个[数据](https://www.nowcoder.com/jump/super-jump/word?word=数据)往后查10个

````sql
select * from table where id > 10 limit 10;
````

#### mysql事务的隔离级别

#### Spring中的设计模式

#### JDBC的存储的流程

#### MyBatis如何连接数据库

- 存在一个sqlSession的工厂类，用来获取session





### 参考文章:

[1]. [JavaGuide](javaguide.cn)