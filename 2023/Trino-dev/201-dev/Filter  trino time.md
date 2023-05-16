Trino

201 - https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/
		600 - https://graphql-trino-refactor-graphql.ssnc-corp.cloud/



graphql

201 - https://feature-aoc-28970-graphql.ssnc-corp.cloud/

600 - https://feature-aoc-31567-graphql.ssnc-corp.cloud/

大小（1.7KB）

| Browser                    | Total Time | Start Time                 | End Time                   |
| -------------------------- | ---------- | -------------------------- | -------------------------- |
|                            |            |                            |                            |
| Waiting for server respons | 897.61     |                            |                            |
| Initial connection         | 490.67     |                            |                            |
| content download           | 0.37       |                            |                            |
| Request                    | 1390       |                            |                            |
|                            |            |                            |                            |
| **Graphql record time**    |            |                            |                            |
| as_of_date trino query     | 24.11      | 2023-03-30T07:43:35.388Z   | 2023-03-30T07:43:35.412Z   |
| custom_property_portfolio  | 29.64      | 2023-03-30T07:43:35.425Z   | 2023-03-30T07:43:35.454Z   |
| Filter  trino query        | 341        | 2023-03-30 07:44:52.945048 | 2023-03-30 07:44:52.945048 |
| graphql query              | 342        | 2023-03-30 07:44:52.944942 | 2023-03-30 07:44:53,287    |
| Graphql resolver           | 367        | 2023-03-30 07:44:52.931289 | 2023-03-30 07:44:53,298    |
| Graphql request            | 375        | 2023-03-30 07:44:52.925602 | 2023-03-30 07:44:53,300    |
| **Trino record time**      |            | 2023-03-30T07:44:52.957Z   | 2023-03-30T07:44:53.285Z   |
| elapsedTime                | 328.71ms   |                            |                            |
| queuedTime                 | 867.82us   |                            |                            |
| resourceWaitingTime        | 3.91ms     |                            |                            |
| dispatchingTime            | 39.80us    |                            |                            |
| executionTime              | 323.90ms   |                            |                            |
| analysisTime               | 4.14ms     |                            |                            |
| planningTime               | 78.75ms    |                            |                            |
| finishingTime              | 2.30ms     |                            |                            |

|                             | 时间     | 开始时间                   | 结束时间                 |
| --------------------------- | -------- | -------------------------- | ------------------------ |
| **浏览器**                  |          |                            |                          |
| Waiting for server response | 1050     |                            |                          |
| Initial connection          | 732.04   |                            |                          |
| content download            | 0.46     |                            |                          |
| Request                     | 2070     |                            |                          |
|                             |          |                            |                          |
| **Graphql 记录时间**        |          |                            |                          |
| aas_of_date trino query     | 0        | 2023-03-30 07:53:43,879    | 2023-03-30 07:53:43,879  |
| custom_property_portfolio   | 0        | 2023-03-30 07:53:43,886    | 2023-03-30 07:53:43,886  |
| Filter  trino query         | 475      | 2023-03-30 07:53:43.888250 | 2023-03-30 07:53:44,363  |
| graphql query               | 475      | 2023-03-30 07:53:43.888149 | 2023-03-30 07:53:44,363  |
| Graphql resolver            | 497      | 2023-03-30 07:53:43.878437 | 2023-03-30 07:53:44,376  |
| Graphql request             | 505      | 2023-03-30 07:53:43.873888 | 2023-03-30 07:53:44,379  |
| **Trino 记录时间**          |          | 2023-03-30T07:53:43.900Z   | 2023-03-30T07:53:44.361Z |
| elapsedTime                 | 461.83ms |                            |                          |
| queuedTime                  | 1.13ms   |                            |                          |
| resourceWaitingTime         | 5.77ms   |                            |                          |
| dispatchingTime             | 62.03us  |                            |                          |
| executionTime               | 454.88ms |                            |                          |
| analysisTime                | 6.21ms   |                            |                          |
| planningTime                | 86.34ms  |                            |                          |
| finishingTime               | 2.45ms   |                            |                          |





|                             | 时间     | 开始时间                   | 结束时间                 |
| --------------------------- | -------- | -------------------------- | ------------------------ |
| **浏览器**                  |          |                            |                          |
| Waiting for server response | 1350     |                            |                          |
| Initial connection          | 539.14   |                            |                          |
| content download            | 0.63     |                            |                          |
| Request                     | 2230     |                            |                          |
|                             |          |                            |                          |
| **Graphql 记录时间**        |          |                            |                          |
| as_of_date trino query      | 0        | 2023-03-30 07:58:38,212    | 2023-03-30 07:58:38,212  |
| custom_property_portfolio   | 0        | 2023-03-30 07:58:38,219    | 2023-03-30 07:58:38,219  |
| Filter  trino query         | 393      | 2023-03-30 07:58:38.221801 | 2023-03-30 07:58:38,615  |
| graphql query               | 393      | 2023-03-30 07:58:38.221690 | 2023-03-30 07:58:38,615  |
| Graphql resolver            | 415      | 2023-03-30 07:58:38.210674 | 2023-03-30 07:58:38,626  |
| Graphql request             | 422      | 2023-03-30 07:58:38.205803 | 2023-03-30 07:58:38,628  |
| **Trino 记录时间**          |          | 2023-03-30T07:58:38.232Z   | 2023-03-30T07:58:38.232Z |
| elapsedTime                 | 381.64ms |                            |                          |
| queuedTime                  | 901.45us |                            |                          |
| resourceWaitingTime         | 4.91ms   |                            |                          |
| dispatchingTime             | 47.80us  |                            |                          |
| executionTime               | 375.78ms |                            |                          |
| analysisTime                | 5.10ms   |                            |                          |
| planningTime                | 89.01ms  |                            |                          |
| finishingTime               | 2.69ms   |                            |                          |



|                              | 时间   | 开始时间                   | 结束时间                |
| ---------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                   |        |                            |                         |
| Waiting for server response  | 642.67 |                            |                         |
| Initial connection           | 495.61 |                            |                         |
| content download             | 0.31   |                            |                         |
| Request                      | 1450   |                            |                         |
|                              |        |                            |                         |
| **Graphql 记录时间**         |        |                            |                         |
| Get portfolio_code cassandra | 5      | 2023-03-30 08:02:44.053055 | 2023-03-30 08:02:44,058 |
| as_of_date trino query       | 2      | 2023-03-30 08:02:44.047934 | 2023-03-30 08:02:44,050 |
| Di_load_date                 | 2      | 2023-03-30 08:02:44.064949 | 2023-03-30 08:02:44,067 |
| Di_load_date                 | 4      | 2023-03-30 08:02:44.068980 | 2023-03-30 08:02:44,073 |
| portfolio_property           | 3      | 2023-03-30 08:02:44.075606 | 2023-03-30 08:02:44,079 |
| Filter  query                | 6      | 2023-03-30 08:02:44.081949 | 2023-03-30 08:02:44,088 |
| Graphql resolver             | 56     | 2023-03-30 08:02:44.047446 | 2023-03-30 08:02:44,103 |
| Graphql request              | 65     | 2023-03-30 08:02:44.043089 | 2023-03-30 08:02:44,108 |

|                              | 时间   | 开始时间                   | 结束时间                |
| ---------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                   |        |                            |                         |
| Waiting for server response  | 1500   |                            |                         |
| Initial connection           | 554.82 |                            |                         |
| content download             | 0.41   |                            |                         |
| Request                      | 2380   |                            |                         |
|                              |        |                            |                         |
| **Graphql 记录时间**         |        |                            |                         |
| Get portfolio_code cassandra | 3      | 2023-03-30 08:09:34.924600 | 2023-03-30 08:09:34,928 |
| as_of_date trino query       | 2      | 2023-03-30 08:09:34.919767 | 2023-03-30 08:09:34,922 |
| Di_load_date                 | 3      | 2023-03-30 08:09:34.934738 | 2023-03-30 08:09:34,938 |
| Di_load_date                 | 2      | 2023-03-30 08:09:34.940618 | 2023-03-30 08:09:34,942 |
| portfolio_property           | 3      | 2023-03-30 08:09:34.944652 | 2023-03-30 08:09:34,948 |
| Filter  query                | 3      | 2023-03-30 08:09:34.951294 | 2023-03-30 08:09:34,955 |
| Graphql resolver             | 51     | 2023-03-30 08:09:34.919335 | 2023-03-30 08:09:34,970 |
| Graphql request              | 58     | 2023-03-30 08:09:34.915139 | 2023-03-30 08:09:34,973 |





|                              | 时间   | 开始时间                   | 结束时间                |
| ---------------------------- | ------ | -------------------------- | ----------------------- |
| **浏览器**                   |        |                            |                         |
| Waiting for server response  | 629.70 |                            |                         |
| Initial connection           | 515.87 |                            |                         |
| content download             | 0.28   |                            |                         |
| Request                      | 1150   |                            |                         |
|                              |        |                            |                         |
| **Graphql 记录时间**         |        |                            |                         |
| Get portfolio_code cassandra | 3      | 2023-03-30 08:13:56.796161 | 2023-03-30 08:13:56,799 |
| as_of_date trino query       | 2      | 2023-03-30 08:13:56.791210 | 2023-03-30 08:13:56,793 |
| Di_load_date                 | 2      | 2023-03-30 08:13:56.805657 | 2023-03-30 08:13:56,807 |
| Di_load_date                 | 3      | 2023-03-30 08:13:56.809571 | 2023-03-30 08:13:56,813 |
| portfolio_property           | 3      | 2023-03-30 08:13:56.815015 | 2023-03-30 08:13:56,818 |
| Filter  query                | 4      | 2023-03-30 08:13:56.821375 | 2023-03-30 08:13:56,825 |
| Graphql resolver             | 50     | 2023-03-30 08:13:56.790758 | 2023-03-30 08:13:56,841 |
| Graphql request              | 59     | 2023-03-30 08:13:56.786315 | 2023-03-30 08:13:56,845 |