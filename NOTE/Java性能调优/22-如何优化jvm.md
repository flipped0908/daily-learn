
可达性分析。
弱引用 只被弱引用关联的对象，只要发生垃圾收集事件，就会被回收。

软引用 软引用 只有系统将要发生内存溢出时，才回去回收软引用引用的对象

 -XX:+UseConcMarkSweepGC\
 cms 回收器
 -XX:CMSInitiatingOccupancyFraction=70 : CMS垃圾收集器，当老年代达到70%时，触发CMS垃圾回收。  
-XX:+CMSParallelRemarkEnabled 降低标记停顿
-XX:SoftRefLRUPolicyMSPerMB=0\ 

XX:SoftRefLRUPolicyMSPerMB
-XX:SoftRefLRUPolicyMSPerMB=N 这个参数比较有用的，官方解释是：Soft reference在虚拟机中比在客户集中存活的更长一些。其清除频率可以用命令行参数 -XX:SoftRefLRUPolicyMSPerMB=<N>来控制，这可以指定每兆堆空闲空间的 soft reference 保持存活（一旦它不强可达了）的毫秒数，这意味着每兆堆中的空闲空间中的 soft reference 会（在最后一个强引用被回收之后）存活1秒钟。注意，这是一个近似的值，因为 soft reference 只会在垃圾回收时才会被清除，而垃圾回收并不总在发生。系统默认为一秒，我觉得没必要等1秒，客户集中不用就立刻清除，改为 -XX:SoftRefLRUPolicyMSPerMB=0；

-XX:+CMSClassUnloadingEnabled：
年老代启用CMS，但默认是不会回收永久代(Perm)的。此处对Perm区启用类回收，防止Perm区内存满。（需要与+CMSPermGenSweepingEnabled同时启用）。
-XX:+CMSPermGenSweepingEnabled：
同上，为了避免Perm区满引起的Full GC，开启并发收集器回收Perm区选项。

https://blog.chriscs.com/2017/06/20/g1-vs-cms/