

LINUX:


JVM:

第 3 层：也称为 C1 编译，执行所有带 Profiling 的 C1 编译；
第 4 层：可称为 C2 编译，也是将字节码编译为本地代码，但是会启用一些编译耗时较长的优化，甚至会根据性能监控信息进行一些不可靠的激进优化。
在 Java8 中，默认开启分层编译，-client 和 -server 的设置已经是无效的了。如果只想开启 C2，可以关闭分层编译（-XX:-TieredCompilation），如果只想用 C1，可以在打开分层编译的同时，使用参数：-XX:TieredStopAtLevel=1。


热点方法的优化可以有效提高系统性能，一般我们可以通过以下几种方式来提高方法内联：

通过设置 JVM 参数来减小热点阈值或增加方法体阈值，以便更多的方法可以进行内联，但这种方法意味着需要占用更多地内存；
在编程中，避免在一个方法中写大量代码，习惯使用小方法体；
尽量使用 final、private、static 关键字修饰方法，编码方法因为继承，会需要额外的类型检查。

这下知道了，为什么 final private static 这些关键词和 范围到底有什么用
他是有用的。

查询当前 JVM 使用的垃圾收集器类型
jmap -heap {pid}


### netty nio -XX:+DisableExplicitGC
这个jvm参数可能会引起内存泄漏
虚引用，Phantom Refenance 被虚引用关联的对象的唯一作用是能在这个对象被回收器回收的时候收到一个系统通知

https://blog.csdn.net/aitangyong/article/details/39403031
https://www.ezlippi.com/blog/2017/10/why-not-expliclitgc.html