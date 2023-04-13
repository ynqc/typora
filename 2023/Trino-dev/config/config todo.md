- 一个节点 fail, 导致了整个查询 fail 

    ​     https://trino.io/docs/current/installation/query-resiliency.html



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



Trino优化方向

- 是用 spill  https://trino.io/docs/current/admin/properties-spilling.html
- Trino支持多个存储系统，包括HDFS、Amazon S3和Google Cloud Storage。选择最适合的存储系统
- 减少磁盘溢出的数量提高查询性能
- 如果数据已分区，请确保使用 partition pruning来消除不必要的数据扫描。
- 选择适合的数据格式，以优化查询性能。例如，像Parquet和ORC这样的列式格式通常比像CSV这样的行式格式更有效率。
- 使用Trino的查询优化功能
- 压缩您的数据以减少存储需求并提高查询性能。Trino支持多种压缩格式，包括gzip、snappy和lz4

