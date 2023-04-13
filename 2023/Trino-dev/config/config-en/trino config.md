##### config.properties

- trino - https://trino.io/docs/current/admin/properties-resource-management.html

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



|                                                              |                                                              |                                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| query.max-memory                                             | This is the max amount of user memory a query can use across the entire cluster. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query across all workers hits this limit it is killed.**Default value:** `20GB` | <= pod memory                                                |
| query.max-history=100                                        | The maximum number of queries to keep in the query history to provide statistics and other information. If this amount is reached, queries are removed based on age. | **Default value:** `100`                                     |
| query.low-memory-killer.delay=5m                             | The amount of time a query is allowed to recover between running out of memory and being killed, if `query.low-memory-killer.policy` or `task.low-memory-killer.policy` is set to value differnt than `none`. | **Default value:** `5m`                                      |
| query.client.timeout=5m                                      | Configures how long the cluster runs without contact from the client application, such as the CLI, before it abandons and cancels its work. | **Default value:** `5m`                                      |
| query.max-memory-per-node=1GB                                | This is the max amount of user memory a query can use on a worker. User memory is allocated during execution for things that are directly attributable to, or controllable by, a user query. For example, memory used by the hash tables built during execution, memory used during sorting, etc. When the user memory allocation of a query on any worker hits this limit, it is killed.  The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config.The sum of query.max-memory-per-node and memory.heap-headroom-per-node must be less than the maximum heap size in the JVM on the node. See JVM config. | **Default value:** (JVM max memory * 0.3)                    |
| query.low-memory-killer.policy=total-reservation-on-blocked-nodes | Configures the behavior to handle killing running queries in the event of low memory availability. Supports the following values:`none` - Do not kill any queries in the event of low memory. `total-reservation` - Kill the query currently using the most total memory. `total-reservation-on-blocked-nodes` - Kill the query currently using the most memory specifically on nodes that are now out of memory. | **Default value:** `total-reservation-on-blocked-nodes`      |
| query.max-length=1000000                                     | The maximum number of characters allowed for the SQL query text. Longer queries are not processed, and terminated with error `QUERY_TEXT_TOO_LARGE`. | **Default value:** `1,000,000` **Maximum value:** `1,000,000,000` |
| http-server.http.port=8080                                   | Specifies the port for the HTTP server. Trino uses HTTP for all communication, internal and external. |                                                              |
| log.max-size=100MB                                           | The maximum file size for the general application log file.  | **Default value:** `100MB`                                   |
| log.max-history=30                                           | The maximum number of general application log files to use, before log rotation replaces old content. | **Default value:** `30`                                      |
| distributed-sort=true                                        | Distributed sort allows to sort data, which exceeds `query.max-memory-per-node`. Distributed sort is enabled via the `distributed_sort` session property, or `distributed-sort` configuration property set in `etc/config.properties` of the coordinator. Distributed sort is enabled by default.When distributed sort is enabled, the sort operator executes in parallel on multiple nodes in the cluster. Partially sorted data from each Trino worker node is then streamed to a single worker node for a final merge. This technique allows to utilize memory of multiple Trino worker nodes for sorting. The primary purpose of distributed sort is to allow for sorting of data sets which don’t normally fit into single node memory. Performance improvement can be expected, but it won’t scale linearly with the number of nodes, since the data needs to be merged by a single node. |                                                              |
| optimizer.join-reordering-strategy=AUTOMATIC                 | The join reordering strategy to use. `NONE` maintains the order the tables are listed in the query. `ELIMINATE_CROSS_JOINS` reorders joins to eliminate cross joins, where possible, and otherwise maintains the original query order. When reordering joins, it also strives to maintain the original table order as much as possible. `AUTOMATIC` enumerates possible orders, and uses statistics-based cost estimation to determine the least cost order. If stats are not available, or if for any reason a cost could not be computed, the `ELIMINATE_CROSS_JOINS` strategy is used. This can be specified on a per-query basis using the `join_reordering_strategy` session property. | **Default value:** `AUTOMATIC`                               |
| task.concurrency=4                                           | Default local concurrency for parallel operators, such as joins and aggregations. This value should be adjusted up or down based on the query concurrency and worker resource utilization. Lower values are better for clusters that run many queries concurrently, because the cluster is already utilized by all the running queries, so adding more concurrency results in slow downs due to context switching and other overhead. Higher values are better for clusters that only run one or a few queries at a time. This can also be specified on a per-query basis using the `task_concurrency` session property. | he number of physical CPUs of the node, with a minimum value of 2 and a maximum of 32 |
| task.http-response-threads=100                               | Maximum number of threads that may be created to handle HTTP responses. Threads are created on demand and are cleaned up when idle, thus there is no overhead to a large value, if the number of requests to be handled is small. More threads may be helpful on clusters with a high number of concurrent queries, or on clusters with hundreds or thousands of workers. | **Default value:** `100`                                     |
| task.max-worker-threads=8                                    | Sets the number of threads used by workers to process splits. Increasing this number can improve throughput, if worker CPU utilization is low and all the threads are in use, but it causes increased heap space usage. Setting the value too high may cause a drop in performance due to a context switching. The number of active threads is available via the `RunningSplits` property of the `trino.execution.executor:name=TaskExecutor.RunningSplits` JMX object. | Node CPUs * 2                                                |
| node-scheduler.max-splits-per-node=100                       | The target value for the total number of splits that can be running for each worker node, assuming all splits have the standard split weight.Using a higher value is recommended, if queries are submitted in large batches (e.g., running a large group of reports periodically), or for connectors that produce many splits that complete quickly but do not support assigning split weight values to express that to the split scheduler. Increasing this value may improve query latency, by ensuring that the workers have enough splits to keep them fully utilized.When connectors do support weight based split scheduling, the number of splits assigned will depend on the weight of the individual splits. If splits are small, more of them are allowed to be assigned to each worker to compensate.Setting this too high wastes memory and may result in lower performance due to splits not being balanced across workers. Ideally, it should be set such that there is always at least one split waiting to be processed, but not higher. | **Default value:** `100`                                     |
| join-distribution-type=AUTOMATIC                             | The type of distributed join to use. When set to `PARTITIONED`, Trino uses hash distributed joins. When set to `BROADCAST`, it broadcasts the right table to all nodes in the cluster that have data from the left table. Partitioned joins require redistributing both tables using a hash of the join key. This can be slower, sometimes substantially, than broadcast joins, but allows much larger joins. In particular broadcast joins are faster, if the right table is much smaller than the left. However, broadcast joins require that the tables on the right side of the join after filtering fit in memory on each node, whereas distributed joins only need to fit in distributed memory across all nodes. When set to `AUTOMATIC`, Trino makes a cost based decision as to which distribution type is optimal. It considers switching the left and right inputs to the join. In `AUTOMATIC` mode, Trino defaults to hash distributed joins if no cost could be computed, such as if the tables do not have statistics. This can be specified on a per-query basis using the `join_distribution_type` session property. | **Default value:** `AUTOMATIC`                               |
| join-max-broadcast-table-size=100MB                          | The join distribution type is automatically chosen when the join reordering strategy is set to `AUTOMATIC` or when the join distribution type is set to `AUTOMATIC`. In both cases, it is possible to cap the maximum size of the replicated table with the `join-max-broadcast-table-size` configuration property or with the `join_max_broadcast_table_size` session property. This allows you to improve cluster concurrency and prevent bad plans when the cost-based optimizer misestimates the size of the joined tables. | By default, the replicated table size is capped to 100MB.    |
| discovery.uri=http://di-trino-cass:8080                      | The Trino coordinator has a discovery service that is used by all the nodes to find each other. Every Trino instance registers itself with the discovery service on startup and continuously heartbeats to keep its registration active. The discovery service shares the HTTP server with Trino and thus uses the same port. Replace `example.net:8080` to match the host and port of the Trino coordinator. If you have disabled HTTP on the coordinator, the URI scheme must be `https`, not `http`. |                                                              |
| coordinator=false                                            |                                                              |                                                              |





