# fast-profiler

快速查看占用CPU前五的java线程栈。    
\# 脚本来自 https://github.com/oldratlee/useful-scripts/blob/master/docs/java.md#beer-show-busy-java-threads

## Usage

```bash
wget https://raw.githubusercontent.com/iqiancheng/fast-profiler/master/show-busy-java-threads
sudo chmod u+x show-busy-java-threads
sudo ./show-busy-java-threads
```

### demo

```java
[root@localhost ~]# ./show-busy-java-threads
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
./show-busy-java-threads -c 3
```

带`-c` 的参数表示显示的线程数

```bash
./show-busy-java-threads -c 3 -p 2886
```

表示指定某个pid的进程，并显示最多3个线程的详情。

```bash
./show-busy-java-threads -p 2886
```

表示指定某个pid的进程。

## What

用于快速排查`Java`的`CPU`性能问题(`top us`值过高)，自动查出运行的`Java`进程中消耗`CPU`多的线程，并打印出其线程栈，从而确定导致性能问题的方法调用。  
目前只支持`Linux`。原因是`Mac`、`Windows`的`ps`命令不支持列出进程的线程`id`，更多信息参见[#33](https://github.com/oldratlee/useful-scripts/issues/33)，欢迎提供解法。

PS，如何操作可以参见[@bluedavy](http://weibo.com/bluedavy)的[《分布式Java应用》](https://book.douban.com/subject/4848587/)的【5.1.1 `CPU`消耗分析】一节，说得很详细：

1. `top`命令找出有问题`Java`进程及线程`id`：
    1. 开启线程显示模式（`top -H`，或是打开`top`后按`H`）
    1. 按`CPU`使用率排序（`top`缺省是按`CPU`使用降序，已经合要求；打开`top`后按`P`可以显式指定按`CPU`使用降序）
    1. 记下`Java`进程`id`及其`CPU`高的线程`id`
1. 用进程`id`作为参数，`jstack`有问题的`Java`进程
1. 手动转换线程`id`成十六进制（可以用`printf %x 1234`）
1. 查找十六进制的线程`id`（可以用`vim`的查找功能`/0x1234`，或是`grep 0x1234 -A 20`）
1. 查看对应的线程栈，以分析问题

查问题时，会要多次上面的操作以分析确定问题，这个过程**太繁琐太慢了**。

## Reference

更多使用方式详见用户文档： <https://github.com/oldratlee/useful-scripts/blob/master/docs/java.md#%E7%94%A8%E6%B3%95>
