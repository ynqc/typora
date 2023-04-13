Trino

201 - https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/
		600 - https://graphql-trino-refactor-graphql.ssnc-corp.cloud/



graphql

201 - https://feature-aoc-28970-graphql.ssnc-corp.cloud/

600 - https://feature-aoc-31567-graphql.ssnc-corp.cloud/

大小（1.6KB）

|                             | 时间     | 开始时间                   | 结束时间                 |
| --------------------------- | -------- | -------------------------- | ------------------------ |
| **浏览器**                  |          |                            |                          |
| Waiting for server response | 1740     |                            |                          |
| Initial connection          | 568.16   |                            |                          |
| content download            | 0.48     |                            |                          |
| Request                     | 2580     |                            |                          |
|                             |          |                            |                          |
| **Graphql 记录时间**        |          |                            |                          |
| as_of_date trino query      | 0        | 2023-03-30 08:52:30,784    | 2023-03-30 08:52:30,784  |
| custom_property_portfolio   | 47       | 2023-03-30 08:52:30.789046 | 2023-03-30 08:52:30,836  |
| Group  trino query          | 993      | 2023-03-30 08:52:30.839708 | 2023-03-30 08:52:31,833  |
| graphql query               | 994      | 2023-03-30 08:52:30.839587 | 2023-03-30 08:52:31,833  |
| Graphql resolver            | 1068     | 2023-03-30 08:52:31,833    | 2023-03-30 08:52:31,842  |
| Graphql request             | 1074     | 2023-03-30 08:52:30.768881 | 2023-03-30 08:52:31,843  |
| **Trino 记录时间**          |          | 2023-03-30T08:52:30.844Z   | 2023-03-30T08:52:31.828Z |
| elapsedTime                 | 984.62ms |                            |                          |
| queuedTime                  | 426.24us |                            |                          |
| resourceWaitingTime         | 3.61ms   |                            |                          |
| dispatchingTime             | 33.28us  |                            |                          |
| executionTime               | 980.55ms |                            |                          |
| analysisTime                | 3.87ms   |                            |                          |
| planningTime                | 65.54ms  |                            |                          |
| finishingTime               | 786.76us |                            |                          |

|                             | 时间   | 开始时间                   | 结束时间                 |
| --------------------------- | ------ | -------------------------- | ------------------------ |
| **浏览器**                  |        |                            |                          |
| Waiting for server response | 1630   |                            |                          |
| Initial connection          | 522.04 |                            |                          |
| content download            | 0.95   |                            |                          |
| Request                     | 2460   |                            |                          |
|                             |        |                            |                          |
| **Graphql 记录时间**        |        |                            |                          |
| as_of_date trino query      | 0      | 2023-03-30 08:57:43,885    | 2023-03-30 08:57:43,885  |
| custom_property_portfolio   | 0      | 2023-03-30 08:57:43,888    | 2023-03-30 08:57:43,888  |
| Groupby  trino query        | 1011   | 2023-03-30 08:57:43.891442 | 2023-03-30 08:57:44,903  |
| graphql query               | 1012   | 2023-03-30 08:57:43.891338 | 2023-03-30 08:57:44,903  |
| Graphql resolver            | 1036   | 2023-03-30 08:57:43.877146 | 2023-03-30 08:57:44,913  |
| Graphql request             | 1043   | 2023-03-30 08:57:43.871570 | 2023-03-30 08:57:44,914  |
| **Trino 记录时间**          |        | 2023-03-30T08:57:43.903Z   | 2023-03-30T08:57:44.902Z |
| elapsedTime                 |        | 999.39ms                   |                          |
| queuedTime                  |        | 1.01ms                     |                          |
| resourceWaitingTime         |        | 4.32                       |                          |
| dispatchingTime             |        | 41.13us                    |                          |
| executionTime               |        | 994.02ms                   |                          |
| analysisTime                |        | 4.65                       |                          |
| planningTime                |        | 63.48ms                    |                          |
| finishingTime               |        | 1.95                       |                          |





|                             | 时间     | 开始时间                   | 结束时间                 |
| --------------------------- | -------- | -------------------------- | ------------------------ |
| **浏览器**                  |          |                            |                          |
| Waiting for server response | 1650     |                            |                          |
| Initial connection          | 743.07   |                            |                          |
| content download            | 0.51     |                            |                          |
| Request                     | 2390     |                            |                          |
|                             |          |                            |                          |
| **Graphql 记录时间**        |          |                            |                          |
| as_of_date trino query      | 0        | 2023-03-30 09:02:12,728    | 2023-03-30 09:02:12,728  |
| custom_property_portfolio   | 0        | 2023-03-30 09:02:12,731    | 2023-03-30 09:02:12,731  |
| group trino query           | 1009     | 2023-03-30 09:02:12.733129 | 2023-03-30 09:02:13,742  |
| graphql query               | 1009     | 2023-03-30 09:02:12.733032 | 2023-03-30 09:02:13,742  |
| Graphql resolver            | 1029     | 2023-03-30 09:02:12.722818 | 2023-03-30 09:02:13,751  |
| Graphql request             | 1034     | 2023-03-30 09:02:12.718191 | 2023-03-30 09:02:13,753  |
| **Trino 记录时间**          |          | 2023-03-30T09:02:12.743Z   | 2023-03-30T09:02:13.736Z |
| elapsedTime                 | 939.78   |                            |                          |
| queuedTime                  | 986.12us |                            |                          |
| resourceWaitingTime         | 3.35     |                            |                          |
| dispatchingTime             | 55.95us  |                            |                          |
| executionTime               | 989.39ms |                            |                          |
| analysisTime                | 3.79     |                            |                          |
| planningTime                | 74.30ms  |                            |                          |
| finishingTime               | 1.81ms   |                            |                          |





graphql

|                                 | 时间   | 开始时间                   | 结束时间                |
| ------------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                      |        |                            |                         |
| Waiting for server response     | 964.06 |                            |                         |
| Initial connection              | 1210   |                            |                         |
| content download                | 0.61   |                            |                         |
| Request                         | 2310   |                            |                         |
|                                 |        |                            |                         |
| **Graphql 记录时间**            |        |                            |                         |
| Get portfolio_code cassandra    | 10     | 2023-03-30 09:30:32.898892 | 2023-03-30 09:30:32,909 |
| as_of_date trino query          | 3      | 2023-03-30 09:30:32.892491 | 2023-03-30 09:30:32,896 |
| Di_load_date                    | 4      | 2023-03-30 09:30:32.921969 | 2023-03-30 09:30:32,926 |
| Di_load_date                    | 4      | 2023-03-30 09:30:32.928537 | 2023-03-30 09:30:32,933 |
| portfolio_property_by_portfolio | 118    | 2023-03-30 09:30:32.934936 | 2023-03-30 09:30:33,053 |
| groupby  query                  | 122    | 2023-03-30 09:30:33.069608 | 2023-03-30 09:30:33,192 |
| Graphql resolver                | 399    | 2023-03-30 09:30:32.892034 | 2023-03-30 09:30:33,291 |
| Graphql request                 | 407    | 2023-03-30 09:30:32.886168 | 2023-03-30 09:30:33,293 |

|                                 | 时间   | 开始时间                   | 结束时间                |
| ------------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                      |        |                            |                         |
| Waiting for server response     | 1100   |                            |                         |
| Initial connection              | 492.75 |                            |                         |
| content download                | 0.30   |                            |                         |
| Request                         | 1940   |                            |                         |
|                                 |        |                            |                         |
| **Graphql 记录时间**            |        |                            |                         |
| Get portfolio_code cassandra    | 12     | 2023-03-30 09:35:21.056115 | 2023-03-30 09:35:21,068 |
| as_of_date trino query          | 10     | 2023-03-30 09:35:21.042846 | 2023-03-30 09:35:21,053 |
| Di_load_date                    | 3      | 2023-03-30 09:35:21.080399 | 2023-03-30 09:35:21,083 |
| Di_load_date                    | 2      | 2023-03-30 09:35:21.085615 | 2023-03-30 09:35:21,088 |
| portfolio_property_by_portfolio | 118    | 2023-03-30 09:35:21.090003 | 2023-03-30 09:35:21,208 |
| groupby  query                  | 115    | 2023-03-30 09:35:21.225184 | 2023-03-30 09:35:21,341 |
| Graphql resolver                | 416    | 2023-03-30 09:35:21.042400 | 2023-03-30 09:35:21,458 |
| Graphql request                 | 422    | 2023-03-30 09:35:21.038302 | 2023-03-30 09:35:21,460 |





|                                 | 时间   | 开始时间                   | 结束时间                |
| ------------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                      |        |                            |                         |
| Waiting for server response     | 967.81 |                            |                         |
| Initial connection              | 547.07 |                            |                         |
| content download                | 0.31   |                            |                         |
| Request                         | 1520   |                            |                         |
|                                 |        |                            |                         |
| **Graphql 记录时间**            |        |                            |                         |
| Get portfolio_code cassandra    | 11     | 2023-03-30 09:38:35.153068 | 2023-03-30 09:38:35,164 |
| as_of_date trino query          | 2      | 2023-03-30 09:38:35.146682 | 2023-03-30 09:38:35,149 |
| Di_load_date                    | 2      | 2023-03-30 09:38:35.177698 | 2023-03-30 09:38:35,179 |
| Di_load_date                    | 2      | 2023-03-30 09:38:35.181815 | 2023-03-30 09:38:35,184 |
| portfolio_property_by_portfolio | 120    | 2023-03-30 09:38:35.186442 | 2023-03-30 09:38:35,307 |
| groupby  query                  | 117    | 2023-03-30 09:38:35.324580 | 2023-03-30 09:38:35,442 |
| Graphql resolver                | 400    | 2023-03-30 09:38:35.146174 | 2023-03-30 09:38:35,546 |
| Graphql request                 | 406    | 2023-03-30 09:38:35.140888 | 2023-03-30 09:38:35,547 |