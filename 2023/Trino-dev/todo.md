1 为啥慢小数据

2 sql 执行计划 

所有ui query - sql table  - partitionkey 改变试试 - 该那些table schema

（1）试试为啥慢

（2）table 怎么改



3 sql-generator refactor

Main

https://feature-aoc-28970-graphql.ssnc-corp.cloud/

Trino 

https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/



比较常关注的就是elapsedTime，代表体感查询时间，其他指标例如queuedTime（在等待执行队列中的时间）、planningTime（执行计划生成时间）、finishingTime（commit元数据与等待客户端消费结果等时间）、analysisTime（从存储引擎拉取元数据并赋予执行计划的时间）、executionTime（从结束queued状态起到执行结束的时间，包括planning）等也值得关注。
------------------------------------------------
版权声明：本文为CSDN博主「书忆江南」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_33588730/article/details/129047228

| Elapsed Time   | 939.15ms |
| -------------- | -------- |
| Queued Time    | 920.67us |
| Analysis Time  | 4.60ms   |
| Planning Time  | 81.89ms  |
| Execution Time | 933.93ms |

{"name": "graphql", "asctime": "2023-03-29 17:59:05,376", "levelname": "INFO", "message": "trino query end: total time 4298 from 2023-03-29 17:59:01.078332"}
{"name": "graphql", "asctime": "2023-03-29 17:59:05,377", "levelname": "INFO", "message": "activity query end: total time 4298 from 2023-03-29 17:59:01.078303"}
{"name": "graphql", "asctime": "2023-03-29 17:59:05,422", "levelname": "INFO", "message": "activity resolver end: total time 4385 from 2023-03-29 17:59:01.036075"}
{"name": "graphql", "asctime": "2023-03-29 17:59:05,425", "levelname": "INFO", "message": "graphql request end: total time 4405 from 2023-03-29 17:59:01.019769"}





| Elapsed Time   | 1.10s    |
| -------------- | -------- |
| Queued Time    | 206.47us |
| Analysis Time  | 5.39ms   |
| Planning Time  | 123.70ms |
| Execution Time | 1.10s    |

{"name": "graphql", "asctime": "2023-03-29 17:43:10,048", "levelname": "INFO", "message": "trino query end: total time 2171 from 2023-03-29 17:43:07.876084"}
{"name": "graphql", "asctime": "2023-03-29 17:43:10,048", "levelname": "INFO", "message": "activity query end: total time 2172 from 2023-03-29 17:43:07.876051"}
{"name": "graphql", "asctime": "2023-03-29 17:43:10,095", "levelname": "INFO", "message": "activity resolver end: total time 2268 from 2023-03-29 17:43:07.826939"}
{"name": "graphql", "asctime": "2023-03-29 17:43:10,099", "levelname": "INFO", "message": "graphql request end: total time 2287 from 2023-03-29 17:43:07.811245"}