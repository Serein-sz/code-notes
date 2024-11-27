# 垃圾回收算法

## 垃圾判定

### 引用计数法 （Reference Counting）

引用计数算法是一种高效直观的垃圾标识技术，它通过给每个对象分配一个计数器来标识对象是否为垃圾，当有其他对象引用该对象时它的计数器就会加1，当引用失效计数器就会减1，当计数器为0时该对象就没有被其他对象引用了，因此就可以回收该对象占用的空间了。这种算法实现起来简单高效，但它存在一个致命的问题，循环依赖时会导致这个环上的所有对象都无法被回收掉，如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/bb48c3769d33427bb3d5125e58063e79~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=1LFcyFcItBJs8hHkgJJwXi%2BUDDo%3D)

> 对象A引用对象B，此时B的计数器为1，
>  对象B引用对象C，此时C的计数器为1，
>  对象C引用对象A，此时A的计数器为1.

此时所有对象的引用计数都不为0，永远无法被回收，如果环上的对象有引用其他对象，那么其他对象也永远无法被回收，就会导致内存泄漏。所以java并没有采用这种垃圾判定算法。

python如何解决引用计数的？

### 可达性分析（Reachability Analysis）

可达性分析，顾名思义就是看对象是否可达；通过一系列`GC Roots`对象作为起点向下搜索，搜索所走过的路径称为引用链（Reference Chain）。当`GC Roots`到一个对象没有任何引用链相连时，说明此对象是不可达的，代表该对象可进行回收。 在Java中，可作为GC Roots的常见对象包括：

> - 虚拟机栈中引用的对象；
> - 本地方法栈中JNI引用的对象；
> - 方法区中类静态属性引用的对象；
> - 方法区中常量引用的对象。

如图： ![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/0cf98eeaf68d47e0bdef2eb6c3341229~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=ims3a7QW9E9dW2fr5ZZ0yeY%2BKdY%3D) 黑色和灰色的对象是可达的，白色对象是不可达的会被当作垃圾回收掉。

## 垃圾清除算法

### 标记-清除（Mark-Sweep）

标记-清除算法分为两个阶段：

1. 标记：从`GC Roots`开始，递归遍历所有可达的对象，并将它们标记是存活的。
2. 清除：遍历堆内存中所有对象，对于没有被标记为存活对象，释放它占用的内存空间。

如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/bb94c658f1e24fab9b029fcd5d52b39f~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=m8iLjEmezPQiMvL%2ByBduv1CIZ90%3D)

通过上述描述可以知道它的算法比较简单，但需要扫描两遍效率较低，并且容易产生内存碎片；如果存活对象比较多，清除动作就会少一些，存活对象较多的情况下适合使用这种垃圾回收算法，例如老年代。

### 复制（Copying）

复制算法会将堆内存分为两半：一半用于分配内存，另一半处于空闲状态。在垃圾收的时将所有活动对象从当前内存复制到另一半内存中，然后清除原有内存区域中的所有对象。如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/51b091bad59c40e6835473c08fb400e8~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=dsWgmqa5jEls%2BPp9WpSE4s9tE0I%3D)

通过上述描述可以知道它可以解决空间碎片的问题，但是比较浪费空间，需要空留出来一定空间来存放存活的对象，需要复制对象并调整对象的引用；在对象存活较少的场景下适合使用这种垃圾回收算法，例如年轻代。

### 标记-压缩（Mark-Compact）

标记-压缩是标记-清除的升级版，在标记存活对象之后将所有存活的对象移动到内存的一端，然后清除端边界外的内容。如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/20c5deb4ff6a467db3f9244bee4f2d0c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=N%2F%2F5JWZl1Us72I18gUa%2F%2FAdeAYc%3D)

可以看到标记-压缩算法，既解决了空间碎片问题也不需要额外空间，但它的步骤也更复杂了。

### 总结

可以看到各种垃圾回收算法的适用场景是不同的。在jvm中ZGC（Epsilon）之前的垃圾回收器都是把内存在逻辑/物理上拆分成不同的区域，对不同的区域采用不同的垃圾回收算法。

# 对象分布

java会把堆分成年轻代和老年代（默认比例1:2），年轻代会分为Eden区和2个survivor区（默认比例8:1:1），如图： ![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/8376b17402aa404bb946e7b542dc092d~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=vX2KGULlk1%2BbMJV%2FfuF%2FnVlQR7g%3D)

创建一个对象首先会尝试在栈上分配，分配不下才会进入eden区域，但在多线程同时创建对象时，会存在同时竞争空间的问题，这里java提供TLAB（线程本地分配）机制减少了多线程竞争eden资源的情况。当经历过young Gc时，eden区中还存活的对象就会进入survivor区（s1），与其一同进入s1区的还有s0区中存活下来的对象，这三个区都属于年轻代，当年轻代中对象存活年龄超过一定阈值就会进入到老年代，除此之外还有一些其他情况也会进入老年代。

> 栈上分配条件：
>  1、线程私有的小对象；
>  2、无逃逸（只在某段代码中使用）；
>  3、标量替换（一个对象，可以把对象拆散成属性）。

> 线程本地分配：
>  1、默认占用eden区的百分之1（很多文章都有提这句话，实际是错误的，实际上jvm会根据线程数量动态分配，在jvm启动时也可以根据`-XX:TLABSize=2048`进行配置，最小2048b）；
>  2、多线程不用竞争申请eden空间，提高创建对象速度；
>  3、只能分配一些小对象。

> 进入老年代的年龄，通过-XX:MaxTenuringThreshold设置：
>  1、po + ps：15；
>  2、cms：6；
>  3、G1：15
>  最大就为15，在之前介绍Synchronized文章中有贴过对象头中mark的含义，它是用4位来标识年龄，4位能标识的最大值就是15（1111）。

> 进入老年的条件：
>  1、动态年龄：当大于等于某个年龄的所有对象大小大于survivor的一定阈值时这些对象也会进入到年代（默认百分之50）；
>  2、大对象：创建的对象特别大（通过-XX:PretenureSizeThreshold设置）；
>  3、分配担保：young Gc时，survivor区的空间不足时直接进入老年代。

# 传统垃圾回收器

目前场景的垃圾回收器全是分代垃圾回收器，如：CMS、G1等，下面会对每个垃圾回收器进行介绍。

## Serial收集器

Serial垃圾回收器是一个采用复制算法的年轻代垃圾回收器，通常配合Serial Old使用（JDK9之前可以搭配CMS），在JDK1.3.1之前它唯一的选择。它在工作时只使用一个单线程，同时必须停止其他所有用户线程，直至它回收完成，这也是垃圾回收中`Stop The World`的由来。当我们的内存比较大，只用一个线程进行回收并且停止用户线程直至垃圾回收完成，这对用户来讲肯定是无法忍受的，所以后续也推出很多优秀的垃圾回收器来减少用户线程停顿的时间，目前除了早期的java应用使用Serial垃圾回收器，在内存资源少，cpu资源少的客户端应用也在使用Serial垃圾回收器。垃圾器回收过程如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/bb3cd0f88f154d4f833f4dad4ff9c7d4~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=Z4t5QSEQFwy4kuszcw8iKYP1EC0%3D)

## Parallel Scavenge收集器（jdk 1.4）

Parallel Scavenge也是使用复制算法的年轻代垃圾回收器。它的本质就是Serial收集器多线程版本，除了多线程以外其他的行为和Serial几乎一样；它更加关注吞储量（处理器用于运行用户代码的时间与处理器总消耗时间的比值）；目前是jdk8的默认年轻代垃圾回收器，更适合后台应用程序，通常搭配`Parallel Old`使用。垃圾回收过程如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/fdb798628b5848fca635068c19939fb1~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=kbBhny%2FxUin8s1N8XVK3EddgMi0%3D)

以及几个参数可以关注一下：

1. `-XX:MaxGCPauseMillis`可以设置最大停顿时间（单位毫秒），来减少对用户体验的影响，但这只能尽量保证在这个时间内完成，时间设置的太短会影响整体的吞吐量；
2. `-XX:GCTimeRatio`可以设置吞吐量大小（默认99）。
3. `-XX:UseAdaptiveSizePolicy`可以让我们不需要关注新年代各区域的比例、晋升老年代对象年龄等参数，jvm会自适应调整各个参数。

## ParNew收集器（jdk 1.5）

Jdk1.5版本由于`Parallel Scavenge`无法和CMS搭配使用，此时推出了`ParNew`；它的本质和`Parallel Scavenge`非常相似，相比ps更关注用户停顿时间，同时它也复用了大量的`Serial收集器`的代码，除了多线程以外和Serial收集器完全一致，它通常搭配CMS+Serial Old使用。

如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4ba8e29bb16045f28c0ad926c87dfced~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=Ar%2FjOHqSNPPTw1GGE%2FE7ihFduYM%3D)

## Serial Old收集器

它是一个单线程老年代垃圾回收器，采用标记-整理算法，通常搭配`Serial`、`Parallel Scavenge`，或者当作cms发生`concurrent Mode Failure`时的备用垃圾回收器。执行过程如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/bb3cd0f88f154d4f833f4dad4ff9c7d4~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=Z4t5QSEQFwy4kuszcw8iKYP1EC0%3D)

## Parallel Old收集器（jdk 1.6）

它是`Parallel Scavenge`收集器的老年代版本，也只是支持多线程并行收集的，它采用标记-整理算法，它俩也是jdk8版本中的默认组合。执行过程如图： ![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/4ba8e29bb16045f28c0ad926c87dfced~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=Ar%2FjOHqSNPPTw1GGE%2FE7ihFduYM%3D)

## CMS收集器

CMS相比上面的垃圾回收器就要复杂很多了。它是一款采用标记-整理算法的老年代垃圾回收器，它的目的是让垃圾回收停顿时间更短，对于c端应用如果能让服务的响应速度更快会给用户带来更好的体验。它的垃圾收集过程分为以下五步：初始标记、并发标记、重新标记、并发清除、并发重置。

> **初始标记**需要stop the world，在初始标记阶段仅是标记一下Gc Roots能直接关联的对象，cms有采用oopMap对这块对象进行了优化，它的速度很快。
>  **并发标记**是从Gc Roots直接关联的对象开始遍历整个对象图的过程，在整个过程中采用三色标记法进行标记，这个过程很长但可以和用户线程共同执行。
>  **重新标记**阶段是为了修正在`并发标记`阶段因为用户线程运行导致标记产生变动的那一部分对象的标记记录；这个阶段通常比`初始标记`停顿时间要长一点。
>  **并发清除**清除掉已经被标记为死亡的对象，因为直接清除就可以了所以可以和用户线程并发执行。
>  **并发重置**为下一次gc做准备。

执行过程如下图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/77e75d9542bc419a9b5946b380410b4c~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=eQ8w4GAOxoEweNR18TV%2FM1wy7gg%3D) 通过上面描述可以了解到CMS可以和用户线程并行运行，这也是JVM的第一次尝试，但在后面CMS一直都没有当作过JDK的默认垃圾回收器并且在后续版本中已经从JDK中移除就可以说明这次尝试还存在很多问题。

首先在回收垃圾时和用户共同运行必然会占用系统资源，它默认的回收线程数是（处理器核心数+3）/4，当处理器核心数比较多时占用的资源还好，但系统资源本身就不多，必然会导致程序运行变慢的。

其次CMS不能处理在并发标记、清除阶段产生的`浮动垃圾`，这样就有可能发生`Concurrent Mode Failure`（几乎每一次压测都会出现）失败，进而导致一次完全`Stop The World`的Full Gc，同时因为垃圾回收的时候用户线程还在运行必然会产生垃圾，这样就必须预留出来一些空间提供给用户线程使用，这个值在JDK5时为68%，到JDK6时就变为百分之92%了，但剩下的这百分之8如果无法满足用户线程运行时需要的内存空间就会使用`serial old` 进行一次垃圾回收，而`serial old`是一个单线程垃圾回收器，这样就会使得服务长时间无法提供服务，可以根据服务运行情况通过`XX:CMSSInitiatingOccupancyFranction`设置其阈值。

最后由于CMS采用的是`标记-清除`算法，必然会产生空间碎片，如果空间碎片过多会给大对象分配带来比较大的麻烦，可能老年代还有很多空间，但就是没办法找到足够大的连续空间来分配对象，此时就会提前发生一次Full Gc。CMS提供了`XX:+UserCMSCompactAtFullCollection`（默认值开）开关参数来控制在不得不进行Full Gc时进行内存碎片的整合，因为整理的过程需要移动对象，而且还没办法和用户线程并发执行，这样停顿的时间就会变长，此时CMS又提供了一个`XX:CMSFillGCsBeforeCompaction`（默认值：0）参数来根据发生多少次不得不进行Full Gc时才进行内存空间整合。这两个参数在JDK9中已经被废弃。

以下情况会触发full gc:

1. **晋升失败**‌：Eden区满了并且触发Minor GC后，可以晋升到老年的对象，放入老年代时发现空间不足，则触发Full Gc；
2. **无法放下大对象**进行大对象分配时，无法找到足够的连续空间来分配该大对象，也会触发Full Gc；
3. **元空间或永久代**：在JDK 8及更高版本中，类信息存放在元空间中。当系统中要加载的类、反射的类和调用的方法较多时，元空间可能会被占满，导致Full Gc；
4. **分配担保**：young Gc时，survivor区的空间不足时直接进入老年代，会导致Full Gc；
5. **发生（Concurrent Mode Failure）**：在执行CMS GC的过程中，如果有对象需要放入老年代，而此时老年代空间不足，或者在做Minor GC的时候，新生代Survivor空间放不下，需要放入老年代，而老年代也放不下，导致并发模式失败，进而触发Full Gc；
6. **显示System.gc()** 通知虚拟机执行Full Gc。

## Garbage First收集器

G1垃圾回收器在jdk11及其以后的版本中都被当作默认的垃圾回收器，这也标志着并行垃圾回收器取得了里程碑式的成功。它基于Region的内部分布形式开创了局部收集的先河；在G1中逻辑上遵循了分代设计思想，但在物理布局上和之前的垃圾回收器有明显的差异，G1不再固定年轻代、老年代的大小，而是把连续的内存划分为多个大小相等的独立区域（Region），每个Region可以根据需要划归为Eden区、Survivor区、Old区、Humongous区（超过Region大小的百分之50就会存放在这里,如果超过百分之百，则放入连续的多个Humongous区）；通过`XX:G1HeapRegionSize`可以设置每个Region的大小，它的取值范围1-32Mb，必须为2的N次幂。

内存分布如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/5d17d5080a5d47728680d5308c246bdb~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=Uppvr2FF6cV4sObgnOkLxGyuIrk%3D)

由于有了region这种思想，也改变了在这之前的垃圾回收器要么收集整个新生代，要么收集整个老年代的方式；G1可以面向堆内存任何部分来组成回收集`Collection Set`，不再取决于region属于哪个分代，而是看哪块region中存放的垃圾数量最多，回收收益最大，这就是G1特有的`Mixed Gc`。同时G1建立“停顿预测模型”，支持在一个长度为M的时间时间片内，消耗在垃圾收集的时间大概率不超过N，为了实现这个功能，G1跟踪了各个Region的垃圾堆积的价值，价值就是回收所获的的空间大小以及回收需要的时间，然后维护了一个优先级列表，每次根据用户设定的收集停顿时间`XX:MaxGcPauseMillis`（默认200ms）优先处理回收价值大的region，这样就保证了在有限的时间获取最高的收集效率；而如何看region价值，G1会记录每个Region的回收耗时，每个region记忆集里的藏卡数量等各种可测量步骤花费的成本，并分析得出的。

对于G1的Major Gc流程分为：初始标记、并发标记、最终标记、筛选回收。

> **初始标记** 仅仅标记一下Gc Roots能直接关联到的对象，还会借用进行Minor Gc的时候同步完成的，耗时非常短。
>  **并发标记** 从Gc Roots开始对对象进行可达性分析，扫描整个堆里的对象，找出要回收的对象，此时是和用户线程并发执行，在这期间会有对象引用的变更。
>  **最终标记** 暂停用户线程，用户处理并发标记阶段变化的那些对象引用。
>  **筛选回收** 更新region的统计数据，对各个region进行回收价值与成本排序，根据用户期望停顿时间来指定执行计划，选择其中一部分region进行回收，把存活对象移到空region中，再清除原有region，因为涉及到移动对象所以也需要停顿用户线程。

如图：

![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/f772acc0996542229cbcaa8ed18bd872~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=cZlDeFScu5rcI0lE5RjQ%2F1OM9cI%3D)

需要注意的是停顿时间尽可能的设置合理一些，这个数值设置一两百毫秒都是合理的，如果设置太短会导致每次收集只能收集一小部分，如果收集的速度逐渐跟不上分配内存的速度，运行时间一久就会导致占满整个堆而引用Full Gc反而降低性能。
 再举个具体例子： 对于young gc会回收全部的eden和survivor区，如果把停顿时间只设置1ms，那么G1根据自适应策略只能把年轻代设置的很小，这样young gc就会特别的频繁，影响服务的吞吐量。在实际的调优场景中考虑响应时间的同时也要考虑吞吐量。感兴趣的朋友可以通过设置`XX:+PrintAdaptiveSizePolicy`和`XX:+PrintTenuringDistribution` 把各个区域打印出来观察一下。

## CMS和G1对比

G1和CMS都是关注停顿时间的垃圾回收器。在早期通常都会拿来进行对比，但目前在高版本jdk中CMS已经被移除了，同时默认使用G1垃圾回收器。相比CMS，G1可以指定最大停顿时间、Region内存布局、按收益动态回收region、算法上也采用`标记-整理`利于长期运行；但由于要维护记忆集付出的成本要比cms高。

对于CMS和G1在JDK11以前发生full gc都是串行收集这样整个回收时间就会变得非常长，如果频繁发生full gc，那它们的性能还不如ps+po的组合，而在JDK11开始对G1的full gc进行改进，支持了并行收集，乍一看其实就让从单线程执行标记-整理，变成多线程，每个线程分配一部分region进行标记-整理，这其中涉及到了很多细节，例如：每个线程标记-整理完整后，最后一个region是不满的，并且当前没有可用region，就会把每个线程最后一个不满的region再进行一次压缩以便可以释放出完整的region空间。除此之外在jdk11中还优化了很多细节，有兴趣的话可以查一下。

------

以上就是常见的垃圾回收，各种垃圾回收器的搭配组合如图所示： ![image.png](https://p3-xtjj-sign.byteimg.com/tos-cn-i-73owjymdk6/10059a57bd954563ad67eb5caced7853~tplv-73owjymdk6-jj-mark-v1:0:0:0:0:5o6Y6YeR5oqA5pyv56S-5Yy6IEAg5oOz5omT5ri45oiP55qE56iL5bqP54y_:q75.awebp?rk3s=f64ab15b&x-expires=1725960027&x-signature=QVzw7R7tjVk8NsCrZIjvl5FaWZg%3D)

注：上面设置参数前面记得加`-`，因为掘金中写`-`会换行，所以在文中没写。

# 总结

整篇文章介绍了垃圾标识算法以及他们各自适用的场景；对象在内存中是如何分配以及流转的以及传统垃圾回收器介绍。重点是CMS和G1垃圾回收器，这两个也是面试中经常考察的问题点，在工作中很多应用程序也是采用了这两个垃圾回收器，对于理解这两款垃圾回收器对我们线上优化服务有很大帮助。
