# 第一章 课程介绍 #
## 本课程会帮你迈过哪些坎？ ##
- 生产环境发生了内存溢出该如何处理？
- 生成环境应该给服务器分配多少内存合适？
- 如何对垃圾收集器的性能进行调优？
- 生产环境CPU负载飙高该如何处理？
- 生产环境应该给应用分配多少线程合适？
- 不加log如何确定请求是否执行了某一行代码？
- 不加log如何实时查看某个方法的入参与返回值？
- JVM的字节码是什么东西？
- 循环体中做字符串+拼接为什么效率低？
- 字符串+拼接一定就是StringBuilder.append吗？
- String常量池是咋回事？
- i++与++i到底哪种写法效率更高？

## 本课程能让你收获什么？ ##
- 熟练使用各种监控和调试工具
- 从容应对生产环境中遇到的各种调试和性能问题
- 熟悉JVM的字节码指令
- 深入理解JVM的自动内存回收机制，学会GC调优
- 从容对面试中关于性能调优和调试的问题
- 独当一面走向高级工程师很重要的一步

## 本课程章节如何安排？ ##
- Tomcat性能监控与调优
- Nginx性能监控与调优
- JVM层GC调优
- Java代码层优化

## 基于JDK命令行工具的监控 ##
- JVM参数类型
- 查看运行时JVM参数
- 查看JVM统计信息
- jmap+MAT实战内存溢出
- jstack实战死循环与死锁

## 基于JVisualVM的可视化监控 ##
- 监控本地JAVA进程
- 监控远程JAVA进程

## 基于Btrace的监控调试 ##
- Btrace安装使用入门
- Btrace使用详解

## Tomcat性能监控与调优 ##
- Tomcat远程debug
- Tomcat-manager监控Tomcat
- psi-probe监控Tomcat
- Tomcat调优

## Nginx性能监控与调优 ##
- ngx_http_stub_status监控连接信息
- mgxtop监控请求信息
- nginx-rrd图形化监控
- nginx调优

## JVM层GC调优 ##
- JVM内存结构
- 垃圾回收算法
- 垃圾收集器
- GC日志格式与可视化日志分析工具
- Tomcat的GC调优实战

## Java代码层调优 ##
- JVM字节码指令与javap
- i++与++i,字符串+拼接原理
- 常用代码优化方法
- 不止这些......

# 第2章  基于JDK命令行工具的监控 #
## 主要内容 ##
- JVM的参数类型
- 运行时JVM参数查看
- jstat查看虚拟机统计信息
- jmap+MAT实战内存溢出
- jstack实战死循环与死锁

## JVM参数类型 ##
标准参数
X参数
XX参数

### 标准参数 ###
- -help
- -server -client
- -version -showversion
- -cp -classpath

### X参数 ###
非标准化参数
-Xint:解释执行
-Xcomp:第一次使用就编译本地代码
-Xmixed:混合模式，JVM自己来决定是否编译成本地代码

### XX参数 ###
- 非标转化参数
- 相对不稳定
- 主要用于JVM调优和Debug

### XX参数分类 ###
**Boolean类型**

	格式：-XX:[+-]<name>表示启动或者禁用name属性
	比如:-XX:+UseConcMarkSweepGC
	     -XX:+UseG1GC

**非Boolean类型**

	格式:-XX:<name>=<value>表示name属性的值是value
	比如:-XX:MaxGCPauseMillis=500
	      XX:GCTimeRatio=19

### -Xmx -Xms ###
	不是X参数，而是XX参数
	-Xms等价于-XX:InitialHeapSize
	-Xmx等价于-XX:MaxHeapSize
	
	[root]#jinfo -flag MaxHeapSize 23789(进程号)
	-XX:MaxHeapSize=268435456
	
	[root]#jinfo -flag THreadStackSize 23789(进程号)
	-XX:THreadStackSize=1024

## 查看JVM运行时参数 ##
	-XX:+PrintFlagsInitial
	-XX:+PrintFlagsFinal
	-XX:+UnlockExperimentalVMOptions解锁实验参数
	-XX:+PrintCommandLineFlags打印命令行参数

### PrintFlagsFinal ###
    java --XX:+PrintFlagsFinal -version 显示如下：

	bool UseG1GC = false
	bool UseGCLogFileRotation = false
	bool UseGCverheadLimit = true
	
	uintx InitialHeapSize :=130023424
	uintx MaxHeapSize := 2053111808
	uintx MaxNewSize := 684195840

	=表示默认值
	:=被用户或者JVM修改后的值

### jps ###
查看Java相关进程
jps -help  
	[root]#jps -l
	17640 sum.tools.jsp.Jps

### jinfo ###
	jinfo 举例
	查看最大内存
	jinfo -flag MaxHeapSize 3176(pid)
	-XX:MaxHeapSize=2147483648
	查看垃圾回收器
	jinfo -flag UseConcMarkSweepGC 3176
	-XX:-UseConcMarkSweepGC
	
	jinfo -flag UseGlGC 3176
	-XX:-UseGlGC
	
	jinfo -flag UseParallelGC 3176
	-XX:-UseParallelGC






