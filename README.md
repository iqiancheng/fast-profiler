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

[3] Busy(0.2%) thread(2949/0xb85) stack of java process(2815) under user(root):
"CommitProcessor:1" prio=10 tid=0x00007ff0c400e800 nid=0xb85 in Object.wait() [0x00007ff11db7a000]
   java.lang.Thread.State: WAITING (on object monitor)
       	at java.lang.Object.wait(Native Method)
       	at java.lang.Object.wait(Object.java:503)
       	at org.apache.zookeeper.server.quorum.CommitProcessor.run(CommitProcessor.java:80)
       	- locked <0x00000000c5637d68> (a org.apache.zookeeper.server.quorum.CommitProcessor)

[4] Busy(0.1%) thread(2948/0xb84) stack of java process(2886) under user(root):
"ProcessThread(sid:2 cport:-1):" prio=10 tid=0x00007f6458021800 nid=0xb84 waiting on condition [0x00007f649faf9000]
   java.lang.Thread.State: WAITING (parking)
       	at sun.misc.Unsafe.park(Native Method)
       	- parking to wait for  <0x00000000c53514b8> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
       	at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
       	at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2043)
       	at java.util.concurrent.LinkedBlockingQueue.take(LinkedBlockingQueue.java:442)
       	at org.apache.zookeeper.server.PrepRequestProcessor.run(PrepRequestProcessor.java:120)

[5] Busy(0.1%) thread(2950/0xb86) stack of java process(2815) under user(root):
"FollowerRequestProcessor:1" prio=10 tid=0x00007ff0c4013000 nid=0xb86 waiting on condition [0x00007ff11dd7c000]
   java.lang.Thread.State: WAITING (parking)
       	at sun.misc.Unsafe.park(Native Method)
       	- parking to wait for  <0x00000000c5637d20> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
       	at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
       	at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2043)
       	at java.util.concurrent.LinkedBlockingQueue.take(LinkedBlockingQueue.java:442)
       	at org.apache.zookeeper.server.quorum.FollowerRequestProcessor.run(FollowerRequestProcessor.java:58)

[root@localhost ~]#
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

## Reference
<https://github.com/oldratlee/useful-scripts>
