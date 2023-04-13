-server

-Xmx3G

-Xms3G

-XX:+UseG1GC

-XX:G1HeapRegionSize=32M

-XX:+UseGCOverheadLimit

-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses

// -XX:+ExplicitGCInvokesConcurrent

-XX:+HeapDumpOnOutOfMemoryError

-XX:+ExitOnOutOfMemoryError

-XX:OnOutOfMemoryError=kill -9 %p         — new

-XX:-UseBiasedLocking

-XX:ReservedCodeCacheSize=512M

-XX:PerMethodRecompilationCutoff=10000

-XX:PerBytecodeRecompilationCutoff=10000

-Djdk.nio.maxCachedBufferSize=2000000

-Djdk.attach.allowAttachSelf=true

-XX:+UnlockDiagnosticVMOptions

-XX:+UseAESCTRIntrinsics

|                                          |                                                              |                                                              |
| ---------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| -Xmx3G                                   | 设置堆的最大大小                                             | <= pod memory                                                |
| -Xms3G                                   | 设置堆的初始化大小                                           | 一般Xms=Xmx,防止扩容和缩容                                   |
| -XX:+UseG1GC                             | 是用G1 垃圾回收机制                                          |                                                              |
| -XX:G1HeapRegionSize=32M                 | 当使用G1收集器时，设置java堆被分割的region大小               | 1M~32M 默认根据堆内存最优化设置                              |
| -XX:+UseGCOverheadLimit                  | 限制GC的运行时间，通过统计GC时间来预测是否要OOM了，提前抛出异常，防止OOM发生 | 并行/并发回收器在GC回收时间过长时会抛出OutOfMemroyError。过长的定义是，超过98%的时间用来做GC并且回收了不到2%的堆内存。用来避免内存过小造成应用不能正常工作 |
| -XX:+ExplicitGCInvokesConcurrent         | 在内存数据库的上下文中，我们将堆外内存与热点的G1收集器结合使用。然而，当堆外内存使用量达到MaxDirectMemorySize时，JDK代码会使用System.GC（）触发一个完整的GC。这会导致一个漫长而痛苦的停止世界GC，这似乎也会将所有当前活动集放在旧一代中，绕过幸存者（从而增加裙带关系问题）。当设置-XX:+ExplicitGCInvokesConcurrent时不会发生这种情况：GC要快得多，并且尊重幸存者。 | 我发现使用此选项的唯一缺点是默认情况下并发GC不会卸载类（请参阅JDK-6541037）。但如果这是您的问题，您可以使用-XX:+ExplicitGCInvokesConcurrentAndUnloadsClasses（使用报错）。 |
| -XX:+HeapDumpOnOutOfMemoryError          | OOM时堆内存dump到当前目录                                    |                                                              |
| -XX:+ExitOnOutOfMemoryError              | 传递此参数时，抛出OutOfMemoryError时JVM将立即退出。如果您想终止应用程序，则可以传递此参数。 |                                                              |
| -XX:OnOutOfMemoryError=kill -9 %p        |                                                              |                                                              |
| -XX:-UseBiasedLocking                    | 禁用偏向锁                                                   | 默认开启，不禁用， 如果使用的是大量的没有竞争的同步，使用偏向锁会提升性能 |
| -XX:ReservedCodeCacheSize=512M           | ReservedCodeCacheSize（和InitialCodeCacheSize）是Java Hotspot VM的（即时）编译器的一个选项。基本上，它设置编译器代码缓存的最大大小。 |                                                              |
| -XX:PerMethodRecompilationCutoff=10000   | 有些工人的任务只占用几分钟的CPU时间，而另一些工人的任务则占用长达2小时的CPU时间！不同的查询运行会显示这种情况会发生在不同的工作人员身上，所以这对任何一个单独的工作人员来说都不是问题。这可能是JVM代码去优化的一个潜在问题。 | 在重新编译一个方法一定次数后，JVM拒绝再这样做，并将以解释模式运行该方法，不同工人的相同任务所使用的CPU相差约30倍。这是编译代码与解释代码的典型区别。 |
| -XX:PerBytecodeRecompilationCutoff=10000 | 以下JVM选项已添加到Trino JVM.config文件中，以帮助解决此问题： | 这些设置增加了重新编译的截止限制。自348版本以来，它们现在也包含在Trino附带的默认jvm.config设置中。 |
| -Djdk.nio.maxCachedBufferSize=2000000    | 在JDK 1.8u102或更新版本中，使用Djdk.nio.maxCachedBufferSize JVM属性来限制每个线程的DirectByteBuffer大小。 |                                                              |
| -Djdk.attach.allowAttachSelf=true        | 在java 9中，默认行为被更改，以防止连接到当前虚拟机，并回到旧的方式，您需要将系统属性jdk.attach.allowAttachSelf设置为tr |                                                              |
| -XX:+UnlockDiagnosticVMOptions           | 此PR将选项-XX:+UnlockDiagnosticVMOptions和-XX:+UseAESCTRIntrinsics添加到core/doker/default/etc/jvm.config和core/trino-server rpm/src/main/resources/config/jvm.config.中，以启用对ARM64上AES CTR/GCM的内在支持。 |                                                              |
| -XX:+UseAESCTRIntrinsics                 | 此PR将选项-XX:+UnlockDiagnosticVMOptions和-XX:+UseAESCTRIntrinsics添加到core/doker/default/etc/jvm.config和core/trino-server rpm/src/main/resources/config/jvm.config.中，以启用对ARM64上AES CTR/GCM的内在支持 |                                                              |

-server

-Xmx3G

-Xms3G

-XX:+UseG1GC

-XX:G1HeapRegionSize=32M

-XX:+UseGCOverheadLimit

-XX:+ExplicitGCInvokesConcurrent

-XX:+HeapDumpOnOutOfMemoryError

-XX:+ExitOnOutOfMemoryError

-XX:OnOutOfMemoryError=kill -9 %p

-XX:-UseBiasedLocking

-XX:ReservedCodeCacheSize=512M

-XX:PerMethodRecompilationCutoff=10000

-XX:PerBytecodeRecompilationCutoff=10000

-Djdk.nio.maxCachedBufferSize=2000000

-Djdk.attach.allowAttachSelf=true

-XX:+UnlockDiagnosticVMOptions

-XX:+UseAESCTRIntrinsics