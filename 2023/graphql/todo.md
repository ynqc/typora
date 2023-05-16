trino config  -》 confluence 配置 thread， worker

过query

201 namespace



tombstone    

（1）并发， 单个结果验证

（2）trino 版本， tombstone 版本  ， 具体分析，执行计划           

（3）哪些有的好，有的坏request

最终， 好在哪，坏在哪？



trino 现在慢

（1）请求重复 - redis

（2）db tombstone

（3）db partition key

（4） 网络传输

（5）



 

|                       | trino version graphql                                       | None trino version graphql                                   | Tombstone           |
| --------------------- | ----------------------------------------------------------- | ------------------------------------------------------------ | ------------------- |
| graphql link          | https://performance-testing-graphql.ssnc-corp.cloud/graphql | https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/graphql |                     |
| graphql version       | /main/di-graphql-strawberry-poc:0.0.1374                    | /release/di-graphql-strawberry-poc:0.12.0                    |                     |
| replicas              | 2                                                           | 2                                                            | 2                   |
| graphql cup/memory    | 2/8G                                                        | 2/8G                                                         | 2/8G                |
| graphql worker/thread | 5/6                                                         | 5/6                                                          | 5/6                 |
| trino                 | data-insight/trino                                          | /                                                            | data-insight/trino  |
| db schema             | performance_testing                                         | performance_testing                                          | performance_testing |

 

| trino  | coordinator | worker |
| ------ | ----------- | ------ |
| number | 1           | 4      |
| cup    | 2           | 2      |
| memory | 4G          | 4G     |









