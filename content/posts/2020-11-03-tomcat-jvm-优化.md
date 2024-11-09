---
title: Tomcat JVM 优化
author: Kubehan
type: post
date: 2020-11-03T09:19:09+08:00
url: /3075.html
post_style:
  - sidebar
cao_vip_rate:
  - 1
views:
  - 912
categories:
  - Linux性能优化
  - Tomcat

---
在Linux环境下设置Tomcat JVM，在/opt/tomcat/bin/catalina.sh文件中找到"# \----- Execute The Requested Command"位置，设置JVM如下：

<pre><code class="language-bash"># ----- Execute The Requested Command ------------
JAVA_OPTS="$JAVA_OPTS -server -Xms3072m -Xmx3072m -XX:PermSize=1024M -XX:MaxPermSize=1024M"</code></pre>

参数说明：

<pre><code class="language-bash">-Xms：设置JVM最小内存。此值可以设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。
-Xmx：设置JVM最大可用内存。
-XX:NewSize：设置年轻代大小
-XX:PermSize：设置永久代大小
-XX:MaxPermSize：设置最大永久代大小</code></pre>

JVM内存模型

#### 1.1、Java栈

Java栈是与每一个线程关联的，JVM在创建每一个线程的时候，会分配一定的栈空间给线程。它主要用来存储线程执行过程中的局部变量，方法的返回值，以及方法调用上下文。栈空间随着线程的终止而释放。

StackOverflowError：如果在线程执行的过程中，栈空间不够用，那么JVM就会抛出此异常，这种情况一般是死递归造成的。

#### 1.2、堆

Java中堆是由所有的线程共享的一块内存区域，堆用来保存各种JAVA对象，比如数组，线程对象等。

#### 1.3、Java 的内存模型

##### a、Young，年轻代（易被 GC）

Young 区被划分为三部分，Eden 区和两个大小严格相同的 Survivor 区，其中 Survivor 区间中，某一时刻只有其中一个是被使用的，另外一个留做垃圾收集时复制对象用，在 Young 区间变满的时候，minor GC 就会将存活的对象移到空闲的Survivor 区间中，根据 JVM 的策略，在经过几次垃圾收集后，任然存活于 Survivor 的对象将被移动到 Tenured 区间。

##### b、Tenured，终身代

Tenured 区主要保存生命周期长的对象，一般是一些老的对象，当一些对象在 Young 复制转移一定的次数以后，对象就会被转移到 Tenured 区，一般如果系统中用了 application 级别的缓存，缓存中的对象往往会被转移到这一区间。

##### c、Perm，永久代

主要保存 class,method,filed 对象，这部门的空间一般不会溢出，除非一次性加载了很多的类，不过在涉及到热部署的应用服务器的时候，有时候会遇到 java.lang.OutOfMemoryError : PermGen space 的错误，造成这个错误的很大原因就有可能是每次都重新部署，但是重新部署后，类的 class 没有被卸载掉，这样就造成了大量的 class 对象保存在了 perm 中，这种情况下，一般重新启动应用服务器可以解决问题。

如果服务器只运行一个 Tomcat：  
机子内存如果是 8G，一般 PermSize 配置是主要保证系统能稳定起来就行：

<pre><code class="language-bash">JAVA_OPTS="-Dfile.encoding=UTF-8 -server -Xms6144m -Xmx6144m -XX:NewSize=1024m -XX:MaxNewSize=2048m -XX:MaxTenuringThreshold=10 -XX:NewRatio=2 -XX:+DisableExplicitGC"</code></pre>

机子内存如果是 16G，一般 PermSize 配置是主要保证系统能稳定起来就行：

<pre><code class="language-bash">JAVA_OPTS="-Dfile.encoding=UTF-8 -server -Xms13312m -Xmx13312m -XX:NewSize=3072m -XX:MaxNewSize=4096m -XX:MaxTenuringThreshold=10 -XX:NewRatio=2 -XX:+DisableExplicitGC"</code></pre>

机子内存如果是 32G，一般 PermSize 配置是主要保证系统能稳定起来就行：

<pre><code class="language-bash">JAVA_OPTS="-Dfile.encoding=UTF-8 -server -Xms29696m -Xmx29696m -XX:NewSize=6144m -XX:MaxNewSize=9216m -XX:MaxTenuringThreshold=10 -XX:NewRatio=2 -XX:+DisableExplicitGC"</code></pre>

如果是开发机：

<pre><code class="language-bash">-Xms550m -Xmx1250m -XX:PermSize=550m -XX:MaxPermSize=1250m</code></pre>

<pre><code class="language-bash">参数说明：
-Dfile.encoding：默认文件编码
-server：表示这是应用于服务器的配置，JVM 内部会有特殊处理的
-Xmx1024m：设置JVM最大可用内存为1024MB
-Xms1024m：设置JVM最小内存为1024m。此值可以设置与-Xmx相同，以避免每次垃圾回收完成后JVM重新分配内存。
-XX:NewSize：设置年轻代大小
-XX:MaxNewSize：设置最大的年轻代大小
-XX:PermSize：设置永久代大小
-XX:MaxPermSize：设置最大永久代大小
-XX:NewRatio=4：设置年轻代（包括 Eden 和两个 Survivor 区）与终身代的比值（除去永久代）。设置为 4，则年轻代与终身代所占比值为 1：4，年轻代占整个堆栈的 1/5
-XX:MaxTenuringThreshold=10：设置垃圾最大年龄，默认为：15。如果设置为 0 的话，则年轻代对象不经过 Survivor 区，直接进入年老代。对于年老代比较多的应用，可以提高效率。如果将此值设置为一个较大值，则年轻代对象会在 Survivor 区进行多次复制，这样可以增加对象再年轻代的存活时间，增加在年轻代即被回收的概论。
-XX:+DisableExplicitGC：这个将会忽略手动调用 GC 的代码使得 System.gc() 的调用就会变成一个空调用，完全不会触发任何 GC</code></pre>