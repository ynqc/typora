##### config.properties

worker  - https://trino.io/docs/current/admin/properties-resource-management.html

query.max-memory=4GB
 	query.max-history=100 // default
 query.low-memory-killer.delay=5m // 没用了因为query.low-memory-killer.policy=total-reservation-on-blocked-nodes
 query.client.timeout=5m // default
 query.max-memory-per-node=1GB // jvm*0.3
 query.low-memory-killer.policy=total-reservation-on-blocked-nodes //default
 query.max-length=1000000  // default
 http-server.log.max-history=15      // 30// 删除
 http-server.log.max-size=100MB   //default // 删除
 http-server.http.port=8080
 log.max-size=100MB
 log.max-history=30
 distributed-sort=true
 optimizer.join-reordering-strategy=AUTOMATIC // default
 task.concurrency=4          // =cup 
 task.http-response-threads=100  //default
 task.max-worker-threads=8    //to 4
 node-scheduler.max-splits-per-node=100
 join-distribution-type=AUTOMATIC
 join-max-broadcast-table-size=100MB
 discovery.uri=http://di-trino-cass:8080
 coordinator=false



|                                                              |                                                              |                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| query.max-memory                                             | This is the max amount of user memory a query can use across the entire cluster. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query across all workers hits this limit it is killed.**Default value:** `20GB` | 这是查询可以在整个集群中使用的最大用户内存量。用户内存是在执行过程中为可直接归因于用户查询或可由用户查询控制的事物分配的。例如，执行过程中生成的哈希表所使用的内存、排序过程中使用的内存等。当查询在所有工作线程中的用户内存分配达到此限制时，它将被终止。 | <= pod memory                                                |
| query.max-history=100                                        | The maximum number of queries to keep in the query history to provide statistics and other information. If this amount is reached, queries are removed based on age. | 要保留在查询历史记录中以提供统计信息和其他信息的最大查询数。如果达到此金额，将根据年龄删除查询。**默认值：**`100` | **Default value:** `100`                                     |
| query.low-memory-killer.delay=5m                             | The amount of time a query is allowed to recover between running out of memory and being killed, if `query.low-memory-killer.policy` or `task.low-memory-killer.policy` is set to value differnt than `none`. | 如果将`query.low memory killer.policy`或`task.low memorykiller.polcy`设置为与`none`不同的值，则允许查询在内存不足和被终止之间恢复的时间。**默认值：**`5m` | **Default value:** `5m`                                      |
| query.client.timeout=5m                                      | Configures how long the cluster runs without contact from the client application, such as the CLI, before it abandons and cancels its work. | 配置在放弃和取消工作之前，群集在没有客户端应用程序（如CLI）联系的情况下运行的时间。**默认值：**`5m` | **Default value:** `5m`                                      |
| query.max-memory-per-node=1GB                                | This is the max amount of user memory a query can use on a worker. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query on any worker hits this limit, it is killed.  The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config.The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config. | 这是查询可以在工作线程上使用的最大用户内存量。用户内存是在执行过程中为可直接归因于用户查询或可由用户查询控制的事物分配的。例如，执行过程中生成的哈希表使用的内存、排序过程中使用的内存等。当查询在任何工作线程上的用户内存分配达到此限制时，它就会被终止。<br/>query.max-memory-per-node和memory.heap-headroom-per-node的总和必须小于节点上JVM中的最大堆大小。请参阅JVM配置。<br/>不适用于启用任务级重试的查询（重试策略=task） | **Default value:** (JVM max memory * 0.3)                    |
| query.low-memory-killer.policy=total-reservation-on-blocked-nodes | Configures the behavior to handle killing running queries in the event of low memory availability. Supports the following values:`none` - Do not kill any queries in the event of low memory. `total-reservation` - Kill the query currently using the most total memory. `total-reservation-on-blocked-nodes` - Kill the query currently using the most memory specifically on nodes that are now out of memory. | `none`-在内存不足的情况下，不要终止任何查询`total-reservation`-终止当前使用最多总内存的查询`total-reservation-on-blocked-nodes`-终止当前使用最多内存的查询，特别是在内存不足的节点上**默认值：**`total-reservation-on-blocked-nodes` | **Default value:** `total-reservation-on-blocked-nodes`      |
| query.max-length=1000000                                     | The maximum number of characters allowed for the SQL query text. Longer queries are not processed, and terminated with error `QUERY_TEXT_TOO_LARGE`. | SQL查询文本允许的最大字符数。较长的查询不会被处理，并以错误QUERY_TEXT_TOO_LARGE终止。 | **Default value:** `1,000,000` **Maximum value:** `1,000,000,000` |
| http-server.log.max-history=15                               |                                                              |                                                              |                                                              |
| http-server.log.max-size=100MB                               |                                                              |                                                              |                                                              |
| http-server.http.port=8080                                   | Specifies the port for the HTTP server. Trino uses HTTP for all communication, internal and external. | 指定HTTP服务器的端口。Trino将HTTP用于所有内部和外部通信。    |                                                              |
| log.max-size=100MB                                           | The maximum file size for the general application log file.  | 常规应用程序日志文件的最大文件大小。                         | **Default value:** `100MB`                                   |
| log.max-history=30                                           | The maximum number of general application log files to use, before log rotation replaces old content. | 在日志轮换替换旧内容之前，要使用的常规应用程序日志文件的最大数量。 | **Default value:** `30`                                      |
| distributed-sort=true                                        | Distributed sort allows to sort data, which exceeds `query.max-memory-per-node`. Distributed sort is enabled via the `distributed_sort` session property, or `distributed-sort` configuration property set in `etc/config.properties` of the coordinator. Distributed sort is enabled by default.When distributed sort is enabled, the sort operator executes in parallel on multiple nodes in the cluster. Partially sorted data from each Trino worker node is then streamed to a single worker node for a final merge. This technique allows to utilize memory of multiple Trino worker nodes for sorting. The primary purpose of distributed sort is to allow for sorting of data sets which don’t normally fit into single node memory. Performance improvement can be expected, but it won’t scale linearly with the number of nodes, since the data needs to be merged by a single node. | 分布式排序允许对超过query.max-memory-per-node的数据进行排序。分布式排序是通过协调器的etc/config.properties中设置的Distributed_sort会话属性或Distributed-sort配置属性启用的。默认情况下启用分布式排序。<br/>启用分布式排序时，排序运算符在集群中的多个节点上并行执行。然后，来自每个Trino工作节点的部分排序数据流式传输到单个工作节点，以进行最终合并。该技术允许利用多个Trino工作节点的内存进行排序。分布式排序的主要目的是允许对通常不适合单节点内存的数据集进行排序。性能改进是可以预期的，但它不会随着节点数量线性扩展，因为数据需要由单个节点合并。 |                                                              |
| optimizer.join-reordering-strategy=AUTOMATIC                 | The join reordering strategy to use. `NONE` maintains the order the tables are listed in the query. `ELIMINATE_CROSS_JOINS` reorders joins to eliminate cross joins, where possible, and otherwise maintains the original query order. When reordering joins, it also strives to maintain the original table order as much as possible. `AUTOMATIC` enumerates possible orders, and uses statistics-based cost estimation to determine the least cost order. If stats are not available, or if for any reason a cost could not be computed, the `ELIMINATE_CROSS_JOINS` strategy is used. This can be specified on a per-query basis using the `join_reordering_strategy` session property. | 要使用的联接重新排序策略。NONE维护表在查询中列出的顺序。ELIMINATE_CROSS_JOINS在可能的情况下重新排序联接以消除交叉联接，并在其他情况下保持原始查询顺序。在对联接进行重新排序时，它还尽量保持原始表的顺序。AUTOMATIC枚举可能的订单，并使用基于统计的成本估计来确定成本最低的订单。如果统计数据不可用，或者由于任何原因无法计算成本，则使用ELIMINATE_CROSS_JOINS策略。这可以使用join_reordering_strategy会话属性在每个查询的基础上指定。 | **Default value:** `AUTOMATIC`                               |
| task.concurrency=4                                           | Default local concurrency for parallel operators, such as joins and aggregations. This value should be adjusted up or down based on the query concurrency and worker resource utilization. Lower values are better for clusters that run many queries concurrently, because the cluster is already utilized by all the running queries, so adding more concurrency results in slow downs due to context switching and other overhead. Higher values are better for clusters that only run one or a few queries at a time. This can also be specified on a per-query basis using the `task_concurrency` session property. | 并行运算符（如联接和聚合）的默认本地并发性。该值应根据查询并发性和工作资源利用率进行上调或下调。对于同时运行许多查询的集群来说，值越低越好，因为所有正在运行的查询都已经使用了集群，因此添加更多的并发会导致由于上下文切换和其他开销而导致的速度减慢。对于一次只运行一个或几个查询的集群，值越高越好。这也可以使用task_concurrency会话属性在每个查询的基础上指定。 | he number of physical CPUs of the node, with a minimum value of 2 and a maximum of 32（节点的物理CPU数量，最小值为2，最大值为32） |
| task.http-response-threads=100                               | Maximum number of threads that may be created to handle HTTP responses. Threads are created on demand and are cleaned up when idle, thus there is no overhead to a large value, if the number of requests to be handled is small. More threads may be helpful on clusters with a high number of concurrent queries, or on clusters with hundreds or thousands of workers. | 可以创建以处理HTTP响应的最大线程数。线程是按需创建的，并在空闲时进行清理，因此，如果要处理的请求数量很小，则不会产生较大值的开销。在具有大量并发查询的集群上，或者在具有数百或数千个工作线程的集群中，更多的线程可能会有所帮助。 | **Default value:** `100`                                     |
| task.max-worker-threads=8                                    | Sets the number of threads used by workers to process splits. Increasing this number can improve throughput, if worker CPU utilization is low and all the threads are in use, but it causes increased heap space usage. Setting the value too high may cause a drop in performance due to a context switching. The number of active threads is available via the `RunningSplits` property of the `trino.execution.executor:name=TaskExecutor.RunningSplits` JMX object. | 设置工作人员用于处理拆分的线程数。如果工作CPU利用率较低并且所有线程都在使用，但会导致堆空间使用率增加，那么增加这个数字可以提高吞吐量。将该值设置得过高可能会由于上下文切换而导致性能下降。活动线程的数量可通过trino.execution.executor:name=TaskExecutor.RunningSplits JMX对象的RunningSplts属性获得。 | Node CPUs * 2                                                |
| node-scheduler.max-splits-per-node=100                       | The target value for the total number of splits that can be running for each worker node, assuming all splits have the standard split weight.Using a higher value is recommended, if queries are submitted in large batches (e.g., running a large group of reports periodically), or for connectors that produce many splits that complete quickly but do not support assigning split weight values to express that to the split scheduler. Increasing this value may improve query latency, by ensuring that the workers have enough splits to keep them fully utilized.When connectors do support weight based split scheduling, the number of splits assigned will depend on the weight of the individual splits. If splits are small, more of them are allowed to be assigned to each worker to compensate.Setting this too high wastes memory and may result in lower performance due to splits not being balanced across workers. Ideally, it should be set such that there is always at least one split waiting to be processed, but not higher. | 假设所有拆分都具有标准拆分权重，则每个工作节点可以运行的拆分总数的目标值。<br/>如果查询是以大批量提交的（例如，定期运行一大组报告），或者对于生成快速完成但不支持分配拆分权重值以将其表达给拆分调度程序的许多拆分的连接器，建议使用更高的值。通过确保工作人员有足够的拆分来充分利用它们，增加这个值可以提高查询延迟。<br/>如果连接器确实支持基于权重的拆分调度，则分配的拆分数量将取决于单个拆分的权重。如果拆分很小，则允许将更多的拆分分配给每个工人进行补偿。<br/>设置得太高会浪费内存，并可能导致性能降低，这是因为工作线程之间的拆分不平衡。理想情况下，应该将其设置为始终至少有一个分割等待处理，但不能更高。 | **Default value:** `100`                                     |
| join-distribution-type=AUTOMATIC                             | The type of distributed join to use. When set to `PARTITIONED`, Trino uses hash distributed joins. When set to `BROADCAST`, it broadcasts the right table to all nodes in the cluster that have data from the left table. Partitioned joins require redistributing both tables using a hash of the join key. This can be slower, sometimes substantially, than broadcast joins, but allows much larger joins. In particular broadcast joins are faster, if the right table is much smaller than the left. However, broadcast joins require that the tables on the right side of the join after filtering fit in memory on each node, whereas distributed joins only need to fit in distributed memory across all nodes. When set to `AUTOMATIC`, Trino makes a cost based decision as to which distribution type is optimal. It considers switching the left and right inputs to the join. In `AUTOMATIC` mode, Trino defaults to hash distributed joins if no cost could be computed, such as if the tables do not have statistics. This can be specified on a per-query basis using the `join_distribution_type` session property. | 要使用的分布式联接的类型。当设置为PARTITIONED时，Trino使用哈希分布式联接。当设置为BROADCAST时，它将右表广播给集群中具有左表数据的所有节点。分区联接需要使用联接键的散列重新分发这两个表。这可能比广播联接慢，有时甚至慢得多，但允许更大的联接。特别是，如果右表比左表小得多，那么广播连接会更快。然而，广播联接要求过滤后联接右侧的表适合每个节点的内存，而分布式联接只需要适合所有节点的分布式内存。当设置为AUTOMATIC时，Trino会根据成本决定哪种配送类型是最佳的。它考虑将左右输入切换到联接。在AUTOMATIC模式下，如果无法计算成本，例如如果表没有统计信息，Trino默认为散列分布式联接。这可以使用join_distribution_type会话属性在每个查询的基础上指定。 | **Default value:** `AUTOMATIC`                               |
| join-max-broadcast-table-size=100MB                          | The join distribution type is automatically chosen when the join reordering strategy is set to `AUTOMATIC` or when the join distribution type is set to `AUTOMATIC`. In both cases, it is possible to cap the maximum size of the replicated table with the `join-max-broadcast-table-size` configuration property or with the `join_max_broadcast_table_size` session property. This allows you to improve cluster concurrency and prevent bad plans when the cost-based optimizer misestimates the size of the joined tables. | 当联接重新排序策略设置为AUTOMATIC或当联接分布类型设置为AUTOMATIC时，将自动选择联接分布类型。在这两种情况下，都可以使用join-max广播表大小配置属性或join_max_broadcast_table_size会话属性来限制复制表的最大大小。这使您能够在基于成本的优化器错误估计联接表的大小时提高集群并发性并防止坏计划。<br/>默认情况下，已复制表的大小上限为100MB。 | By default, the replicated table size is capped to 100MB.    |
| discovery.uri=http://di-trino-cass:8080                      | The Trino coordinator has a discovery service that is used by all the nodes to find each other. Every Trino instance registers itself with the discovery service on startup and continuously heartbeats to keep its registration active. The discovery service shares the HTTP server with Trino and thus uses the same port. Replace `example.net:8080` to match the host and port of the Trino coordinator. If you have disabled HTTP on the coordinator, the URI scheme must be `https`, not `http`. | Trino协调器有一个发现服务，所有节点都使用该服务来查找彼此。每个Trino实例在启动时都会向发现服务注册自己，并不断检测信号以保持注册活动。发现服务与Trino共享HTTP服务器，因此使用相同的端口。替换example.net:8080以匹配Trino协调器的主机和端口。如果在协调器上禁用了HTTP，则URI方案必须是https，而不是HTTP。 |                                                              |
| coordinator=false                                            |                                                              |                                                              |                                                              |





**worker**

query.max-memory=4GB
 	query.max-history=100 // default
 query.client.timeout=5m // default
 query.max-memory-per-node=1.2GB // jvm*0.3
 query.low-memory-killer.policy=total-reservation-on-blocked-nodes //default
 query.max-length=1000000  // default
 http-server.http.port=8080
 log.max-size=100MB
 log.max-history=30
 distributed-sort=true
 task.concurrency=2
 task.http-response-threads=100 
 task.max-worker-threads=4
 discovery.uri=http://di-trino-cass:8080
 coordinator=false





**coordinator**



query.max-memory=4GB
 	query.max-history=100 // default
 query.client.timeout=5m // default
 query.max-memory-per-node=1.2GB // jvm*0.3
 query.low-memory-killer.policy=total-reservation-on-blocked-nodes //default
 query.max-length=1000000  // default
 http-server.http.port=8080
 log.max-size=100MB
 log.max-history=30
 distributed-sort=true
 task.concurrency=2      // =cup 
 task.http-response-threads=100 
 task.max-worker-threads=4

coordinator=true
       node-scheduler.include-coordinator=false
       discovery.uri=http://localhost:8080