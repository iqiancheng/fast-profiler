# fast-profiler
快速查看占用CPU前五的java线程栈

## Usage

```bash
wget https://raw.githubusercontent.com/iqiancheng/fast-profiler/master/show-busy-java-threads.sh
sudo chmod u+x show-busy-java-threads.sh
sudo ./show-busy-java-threads.sh
```
demo

```java
[root@localhost ~]# ./show-busy-java-threads.sh
[1] Busy(0.3%) thread(2903/0xb57) stack of java process(2886) under user(root):
"NIOServerCxn.Factory:0.0.0.0/0.0.0.0:2182" daemon prio=10 tid=0x00007f64b023b000 nid=0xb57 runnable [0x00007f64a4844000]
   java.lang.Thread.State: RUNNABLE
       	at sun.nio.ch.EPollArrayWrapper.epollWait(Native Method)
       	at sun.nio.ch.EPollArrayWrapper.poll(EPollArrayWrapper.java:269)
       	at sun.nio.ch.EPollSelectorImpl.doSelect(EPollSelectorImpl.java:79)
       	at sun.nio.ch.SelectorImpl.lockAndDoSelect(SelectorImpl.java:87)
       	- locked <0x00000000c4216a40> (a sun.nio.ch.Util$2)
       	- locked <0x00000000c4216a30> (a java.util.Collections$UnmodifiableSet)
       	- locked <0x00000000c42168a8> (a sun.nio.ch.EPollSelectorImpl)
       	at sun.nio.ch.SelectorImpl.select(SelectorImpl.java:98)
       	at org.apache.zookeeper.server.NIOServerCnxnFactory.run(NIOServerCnxnFactory.java:178)
       	at java.lang.Thread.run(Thread.java:745)

[2] Busy(0.2%) thread(2946/0xb82) stack of java process(2886) under user(root):
"CommitProcessor:2" prio=10 tid=0x00007f645801c800 nid=0xb82 in Object.wait() [0x00007f649fffe000]
   java.lang.Thread.State: WAITING (on object monitor)
       	at java.lang.Object.wait(Native Method)
       	at java.lang.Object.wait(Object.java:503)
       	at org.apache.zookeeper.server.quorum.CommitProcessor.run(CommitProcessor.java:80)
       	- locked <0x00000000c53d84e8> (a org.apache.zookeeper.server.quorum.CommitProcessor)

...

```
是不是很好用呢！

## 更多用法
```bash
./show-busy-java-threads.sh -c 3
```
带`-c` 的参数表示显示的线程数

```bash
./show-busy-java-threads.sh -c 3 -p 2886
```
表示指定某个pid的进程，并显示最多3个线程的详情。

```bash
./show-busy-java-threads.sh -p 2886
```
表示指定某个pid的进程。

## What
用于快速排查Java的CPU性能问题(`top usage`值过高)，自动查出运行的Java进程中消耗CPU多的线程，并打印出其线程栈，从而确定导致性能问题的方法调用。

PS，如何操作可以参见@bluedavy的《分布式Java应用》的【5.1.1 cpu消耗分析】一节，说得很详细：

top命令找出有问题Java进程及线程id：

- 开启线程显示模式
- 按CPU使用率排序
- 记下Java进程id及其CPU高的线程id
- 用进程id作为参数，jstack有问题的Java进程
- 手动转换线程id成十六进制（可以用printf %x 1234）
- 查找十六进制的线程id（可以用grep）
- 查看对应的线程栈

查问题时，会要多次这样操作以确定问题，上面过程太繁琐太慢了。

## Reference
<https://github.com/oldratlee/useful-scripts>
