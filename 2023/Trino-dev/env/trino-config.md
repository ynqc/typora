-server
-Xms16G
-Xmx16G
-Xss8M
-XX:+UseG1GC
-XX:G1HeapWastePercent=5
-XX:+ParallelRefProcEnabled
-XX:ParallelGCThreads=48
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:OnOutOfMemoryError=kill -9 %p
-DHADOOP_USER_NAME=hdfs
-XX:+ExitOnOutOfMemoryError
-XX:PerMethodRecompilationCutoff=10000
-XX:PerBytecodeRecompilationCutoff=10000
-XX:ReservedCodeCacheSize=2G
-XX:+UseCodeCacheFlushing
-XX:NativeMemoryTracking=detail
-XX:+PrintCompilation
-XX:+CITime
-XX:+PrintCodeCache
-Djdk.nio.maxCachedBufferSize=4000000
-Djdk.attach.allowAttachSelf=true
-XX:G1HeapRegionSize=32M


-server
-Xms450G
-Xmx450G
-Xss8M
-XX:+UseG1GC
-XX:G1HeapWastePercent=5
-XX:+ParallelRefProcEnabled
-XX:ParallelGCThreads=48
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:OnOutOfMemoryError=kill -9 %p
-DHADOOP_USER_NAME=hdfs
<!-- -Dpresto-temporarily-allow-java8=true -->
-XX:PerMethodRecompilationCutoff=10000
-XX:PerBytecodeRecompilationCutoff=10000
-XX:ReservedCodeCacheSize=2G
-XX:+UseCodeCacheFlushing
-XX:NativeMemoryTracking=detail
-XX:+PrintCompilation
-XX:+CITime
-XX:+PrintCodeCache
-Djdk.nio.maxCachedBufferSize=4000000
-Djdk.attach.allowAttachSelf=true
-XX:G1HeapRegionSize=32M


coordinator=false
node-scheduler.include-coordinator=true
http-server.http.port=8080
query.max-memory=10GB
query.max-memory-per-node=5GB
memory.heap-headroom-per-node=5GB
discovery.uri=http://di-trino-cass:8080

Trino 372正式发布 删除对保留内存池的支持。 不能再使用配置属性experimental.reserved-pool-disabled=true
启动压缩以减少在 GCS 上假脱机的数据量(exchange.compression-enabled=true)

query.max-memory=16GB
query.max-history=3000
query.low-memory-killer.delay=1m
query.client.timeout=5m
query.max-memory-per-node=4GB
query.low-memory-killer.policy=total-reservation-on-blocked-nodes
query.max-total-memory=24GB
query.max-length=600000
http-server.log.max-size=100MB
http-server.http.port=8080
http-server.accept-queue-size=16000
http-server.http.selector-threads=32
http-server.threads.max=500
http-server.http.acceptor-threads=32
http-server.threads.min=50
http-server.log.max-history=10
log.max-size=100MB
log.max-history=10
distributed-sort=true
exchange.http-client.idle-timeout=1m
exchange.compression-enabled=true
optimizer.enable-intermediate-aggregations=true
optimizer.default-filter-factor-enabled=true
optimizer.join-reordering-strategy=AUTOMATIC
optimizer.optimize-mixed-distinct-aggregations=true
optimizer.use-mark-distinct=true
task.concurrency=32
task.max-worker-threads=100
task.max-leaf-splits-per-node=50
node-scheduler.use-cacheable-white-list=true
node-scheduler.max-splits-per-node=100
node-scheduler.include-coordinator=false
node-scheduler.max-splits-increment-for-caching=300
join-distribution-type=AUTOMATIC
join-max-broadcast-table-size=2GB
writer-min-size=128MB
discovery.uri=http://di-trino-cass:8080
memory.heap-headroom-per-node=5GB
coordinator=false



config.properties
coordinator=true

node-scheduler.include-coordinator=false

http-server.http.port=8080

query.max-memory=10GB

query.max-memory-per-node=5GB

memory.heap-headroom-per-node=5GB

discovery-server.enabled=true

discovery.uri=http://localhost:8080

jvm.config
-server

-Xmx16G

-XX:+UseG1GC

-XX:G1HeapRegionSize=32M

-XX:+UseGCOverheadLimit

-XX:+ExplicitGCInvokesConcurrent

-XX:+HeapDumpOnOutOfMemoryError

-XX:+ExitOnOutOfMemoryError

-Djdk.attach.allowAttachSelf=true

-XX:-UseBiasedLocking

-XX:ReservedCodeCacheSize=512M

-XX:PerMethodRecompilationCutoff=10000

-XX:PerBytecodeRecompilationCutoff=10000

-Djdk.nio.maxCachedBufferSize=2000000

-XX:+UnlockDiagnosticVMOptions

-XX:+UseAESCTRIntrinsics



-server
-Xmx16G
-XX:+UseG1GC
-XX:G1HeapRegionSize=32M
-XX:+UseGCOverheadLimit
-XX:+ExplicitGCInvokesConcurrent
-XX:+HeapDumpOnOutOfMemoryError
-XX:+ExitOnOutOfMemoryError
-Djdk.attach.allowAttachSelf=true
-XX:-UseBiasedLocking
-XX:ReservedCodeCacheSize=512M
-XX:PerMethodRecompilationCutoff=10000
-XX:PerBytecodeRecompilationCutoff=10000
-Djdk.nio.maxCachedBufferSize=2000000
-XX:+UnlockDiagnosticVMOptions
-XX:+UseAESCTRIntrinsics





query.max-memory=16GB
query.max-history=3000
query.low-memory-killer.delay=1m
query.client.timeout=5m
query.max-memory-per-node=4GB
query.low-memory-killer.policy=total-reservation-on-blocked-nodes
query.max-total-memory=24GB
query.max-length=600000
http-server.log.max-size=100MB
http-server.http.port=8080
http-server.accept-queue-size=16000
http-server.http.selector-threads=32
http-server.threads.max=500
http-server.http.acceptor-threads=32
http-server.threads.min=50
http-server.log.max-history=10
log.max-size=100MB
log.max-history=10
distributed-sort=true
exchange.http-client.idle-timeout=1m
exchange.compression-enabled=true

optimizer.enable-intermediate-aggregations=true
optimizer.default-filter-factor-enabled=true
optimizer.join-reordering-strategy=AUTOMATIC
optimizer.optimize-mixed-distinct-aggregations=true
optimizer.use-mark-distinct=true
task.concurrency=32


task.max-worker-threads=100
<!-- task.max-leaf-splits-per-node=50 -->
node-scheduler.max-splits-per-node=100

<!-- node-scheduler.use-cacheable-white-list=true

node-scheduler.max-splits-increment-for-caching=300 -->



node-scheduler.include-coordinator=false
join-distribution-type=AUTOMATIC
join-max-broadcast-table-size=2GB
writer-min-size=128MB
discovery-server.enabled=true
discovery.uri=http://localhost:8080
memory.heap-headroom-per-node=5GB
coordinator=true

https://www.cnblogs.com/meicanhong/p/16830571.html

https://github.com/trinodb/trino/issues/6405


query.max-memory=16GB 默认值是20G
https://zhuanlan.zhihu.com/p/145858444



query.max-memory=16GB
query.max-history=3000
query.low-memory-killer.delay=1m
query.client.timeout=5m
query.max-memory-per-node=4GB
query.max-total-memory-per-node=6G
query.low-memory-killer.policy=total-reservation-on-blocked-nodes
query.max-total-memory=24GB
query.max-length=600000
http-server.log.max-size=100MB
http-server.http.port=8080
http-server.accept-queue-size=16000
<!-- http-server.http.selector-threads=32 -->
http-server.threads.max=500
http-server.http.acceptor-threads=32
http-server.threads.min=50
http-server.log.max-history=10
log.max-size=100MB
log.max-history=10
distributed-sort=true
exchange.http-client.idle-timeout=1m
exchange.compression-enabled=true
optimizer.enable-intermediate-aggregations=true
optimizer.default-filter-factor-enabled=true
optimizer.join-reordering-strategy=AUTOMATIC
optimizer.optimize-mixed-distinct-aggregations=true
optimizer.use-mark-distinct=true
task.concurrency=8
task.http-response-threads=8
task.max-worker-threads=8
node-scheduler.max-splits-per-node=100
node-scheduler.include-coordinator=false
join-distribution-type=AUTOMATIC
join-max-broadcast-table-size=2GB
writer-min-size=128MB
discovery.uri=http://di-trino-cass:8080
memory.heap-headroom-per-node=5GB
coordinator=false




https://zhuanlan.zhihu.com/p/145858444


Perf Flame Graph 图 https://zhuanlan.zhihu.com/p/614212908 



query.max-memory=16GB
query.max-history=100
query.low-memory-killer.delay=5m
query.client.timeout=5m
query.max-memory-per-node=1GB
query.low-memory-killer.policy=total-reservation-on-blocked-nodes
query.max-length=1000000
http-server.log.max-history=15
http-server.log.max-size=100MB
http-server.http.port=8080
log.max-size=100MB
log.max-history=30
distributed-sort=true
optimizer.join-reordering-strategy=AUTOMATIC
task.concurrency=4
task.http-response-threads=100
task.max-worker-threads=8
node-scheduler.max-splits-per-node=100
join-distribution-type=AUTOMATIC
join-max-broadcast-table-size=100MB
discovery.uri=http://di-trino-cass:8080
coordinator=false

性能
worker 数据传输转换（hive， spill， 压缩）
数据库 partition， presto/trino（trino 分层）
trino file 扫描浪费时间（开始的时候先扫描file 这个oom）

Hardware: Ensure that you have enough compute and storage resources to support your workload. Trino is designed to scale horizontally, so adding more nodes to your cluster can help improve performance.
Memory: Increase the memory allocated to Trino workers. This will help reduce the number of spills to disk and improve query performance.
Configuration: Optimize the Trino configuration based on the size of your cluster and the nature of your workload. This includes settings such as query.max-memory-per-node, query.max-memory, query.max-total-memory-per-node, and query.max-total-memory.
Storage: Trino supports several storage systems, including HDFS, Amazon S3, and Google Cloud Storage. Choose the storage system that best fits your use case and optimize its performance.
Partitioning: If your data is partitioned, ensure that you are using partition pruning to eliminate unnecessary data scans.
Data Formats: Choose the appropriate data formats for your data to optimize query performance. For example, columnar formats like Parquet and ORC are often more efficient than row-based formats like CSV.
Query Optimization: Use Trino's query optimization features such as cost-based optimization and predicate pushdown to optimize your queries.
Compression: Compress your data to reduce storage requirements and improve query performance. Trino supports several compression formats, including gzip, snappy, and lz4.

硬件：确保您具有足够的计算和存储资源来支持您的工作负载。 Trino旨在水平扩展，因此向集群添加更多节点可以帮助提高性能。
内存：增加分配给Trino工作节点的内存。这将有助于减少磁盘溢出的数量，并提高查询性能。
配置：根据集群的大小和工作负载的性质优化Trino配置。这包括设置，例如query.max-memory-per-node、query.max-memory、query.max-total-memory-per-node和query.max-total-memory等。
存储：Trino支持多个存储系统，包括HDFS、Amazon S3和Google Cloud Storage。选择最适合您用例的存储系统并优化其性能。
分区：如果您的数据已分区，请确保使用分区剪枝来消除不必要的数据扫描。
数据格式：选择适合您数据的数据格式，以优化查询性能。例如，像Parquet和ORC这样的列式格式通常比像CSV这样的行式格式更有效率。
查询优化：使用Trino的查询优化功能，例如基于成本的优化和谓词下推，来优化您的查询。
压缩：压缩您的数据以减少存储需求并提高查询性能。Trino支持多种压缩格式，包括gzip、snappy和lz4。
