Trino coordinator/worker jvm.config (https://docs.oracle.com/en/java/javase/17/docs/specs/man/java.html)



-server

-Xmx3G

-Xms3G

-XX:+UseG1GC

-XX:G1HeapRegionSize=32M

-XX:+UseGCOverheadLimit

-XX:+ExplicitGCInvokesConcurrent

-XX:+HeapDumpOnOutOfMemoryError

-XX:HeapDumpPath=/var/log/java/java_heapdump.hprof              // new

-XX:+ExitOnOutOfMemoryError

-XX:OnOutOfMemoryError=kill -9 %p

-XX:-UseBiasedLocking

-XX:ReservedCodeCacheSize=100M                         // change

-XX:PerMethodRecompilationCutoff=10000

-XX:PerBytecodeRecompilationCutoff=10000

-Djdk.nio.maxCachedBufferSize=2000000

-Djdk.attach.allowAttachSelf=true

-XX:+UnlockDiagnosticVMOptions

-XX:+UseAESCTRIntrinsics

|                                                              |                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| -Xmx3G                                                       | Specifies the maximum size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 2 MB.  For server deployments, `-Xms` and `-Xmx` are often set to the same value. | <= pod memory                                                |
| -Xms3G                                                       | Specifies the maximum size (in bytes) of the heap. This value must be a multiple of 1024 and greater than 2 MB. | For server deployments, `-Xms` and `-Xmx` are often set to the same value. |
| -XX:+UseG1GC                                                 | Enables the use of the garbage-first (G1) garbage collector. It's a server-style garbage collector, targeted for multiprocessor machines with a large amount of RAM. This option meets GC pause time goals with high probability, while maintaining good throughput. | By default, this option is enabled and G1 is used as the default garbage collector. |
| -XX:G1HeapRegionSize=32M                                     | Sets the size of the regions into which the Java heap is subdivided when using the garbage-first (G1) collector. The value is a power of 2 and can range from 1 MB to 32 MB. The default region size is determined ergonomically based on the heap size with a goal of approximately 2048 regions. | 1M~32M default based on heap memory optimization settings    |
| -XX:+UseGCOverheadLimit                                      | Enables the use of a policy that limits the proportion of time spent by the JVM on GC before an `OutOfMemoryError` exception is thrown. This option is enabled, by default, and the parallel GC will throw an `OutOfMemoryError` if more than 98% of the total time is spent on garbage collection and less than 2% of the heap is recovered. When the heap is small, this feature can be used to prevent applications from running for long periods of time with little or no progress. To disable this option, specify the option `-XX:-UseGCOverheadLimit`. | by default enabled                                           |
| -XX:+ExplicitGCInvokesConcurrent                             | Enables invoking of concurrent GC by using the `System.gc()` request. This option is disabled by default and can be enabled only with the `-XX:+UseG1GC` option. | by default disabled                                          |
| -XX:+HeapDumpOnOutOfMemoryError                              | Enables the dumping of the Java heap to a file in the current directory by using the heap profiler (HPROF) when a java.lang.OutOfMemoryError exception is thrown. |                                                              |
| -XX:HeapDumpPath=path                                        | Sets the path and file name for writing the heap dump provided by the heap profiler (HPROF) when the `-XX:+HeapDumpOnOutOfMemoryError` option is set. By default, the file is created in the current working directory, and it's named `java_pid<pid>.hprof` where `<pid>` is the identifier of the process that caused the error. The following example shows how to set the default file explicitly (`%p` represents the current process identifier): |                                                              |
| -XX:+ExitOnOutOfMemoryError                                  | When passing this parameter, the JVM will immediately exit when an OutOfMemoryError is thrown. |                                                              |
| -XX:OnOutOfMemoryError=kill -9 %p                            | When OOM is killing a process                                |                                                              |
| -XX:-UseBiasedLocking                                        | Enables the use of biased locking. Some applications with significant amounts of uncontended synchronization may attain significant speedups with this flag enabled, but applications with certain patterns of locking may see slowdowns. | By default, this option is disabled.                         |
| -XX:ReservedCodeCacheSize=100M                               | Sets the maximum code cache size (in bytes) for JIT-compiled code. This option has a limit of 2 GB; otherwise, an error is generated. | the default size is 48 MB                                    |
| -XX:PerMethodRecompilationCutoff=10000 -XX:PerBytecodeRecompilationCutoff=10000 | that the CPU used by the same tasks on different workers vary by a factor of approximately 30. The following JVM options were added to the Trino jvm.config file to help with this issue:<br />-XX:PerMethodRecompilationCutoff=10000<br/>-XX:PerBytecodeRecompilationCutoff=10000<br/>These settings increased the recompilation cutoff limit. They are now also included in the default jvm.config settings that ship with Trino since the 348 release. |                                                              |
| -Djdk.nio.maxCachedBufferSize=2000000                        | In JDK 1.8u102 or later, use the Djdk.nio.maxCachedBufferSize JVM property to limit the DirectByteBuffer size for each thread. |                                                              |
| -Djdk.attach.allowAttachSelf=true                            | In Java 9, the default behavior has been changed to prevent connecting to the current virtual machine and returning to the old way. You need to set the system property jdk.attach.allowAttachSelf to true |                                                              |
| -XX:+UnlockDiagnosticVMOptions<br />-XX:+UseAESCTRIntrinsics | This PR adds the options - XX:+UnlockDiagnosticVMOptions and - XX:+UseAESCTRIntrinsics to the core/toker/default/etc/jvm. config and core/trino server rpm/src/main/resources/config/jvm. config. to enable built-in support for AES CTR/GCM on ARM64. |                                                              |

**Djdk.nio.maxCachedBufferSize**

Ability to limit the capacity of buffers that can be held in the temporary buffer cache:<br/>"The system property  jdk.nio.maxCachedBufferSize  has been introduced in 8u102 to limit the memory used by the "temporary buffer cache." The temporary buffer cache is a per-thread cache of direct memory used by the NIO implementation to support applications that do I/O with buffers backed by arrays in the Java heap. The value of the property is the maximum capacity of a direct buffer that can be cached. If the property is not set, then no limit is put on the size of buffers that are cached. Applications with certain patterns of I/O usage may benefit from using this property. In particular, an application that does I/O with large multi-megabyte buffers at startup but does I/O with small buffers may see a benefit to using this property. Applications that perform I/O using direct buffers will not see any benefit to using this system property."<br/><br/>Here are several important observations from experimenting with this property and reading the JDK source code:<br/><br/>You cannot use “M,” “GB,” and so on when specifying the value. Only plain numbers work, e.g. -Djdk.nio.maxCachedBufferSize=1000000 <br/><br/>The value is indeed “per thread." So, if you want to limit the total off-heap part of the RSS to R, you should estimate the number of I/O threads T and use R/T as the value of this property.<br/><br/>Setting -Djdk.nio.maxCachedBufferSize doesn’t prevent allocation of a big DirectByteBuffer — it only prevents this buffer from being cached and reused. So, if each of 10 threads allocates a 1GB HeapByteBuffer  and then invokes some I/O operation simultaneously, there will be a temporary RSS spike of up to 10GB, due to 10 temporary direct buffers. However, each of these DirectByteBuffers will be deallocated immediately after the I/O operation. In contrast, any direct buffers smaller than the threshold will stick in memory until their owner thread terminates.