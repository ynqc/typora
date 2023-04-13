TEXT BOOLEAN FLOAT DATETIMEnginx.ingress.kubernetes.io/proxy-buffer-size: 256k
    nginx.ingress.kubernetes.io/proxy-connect-timeout: '480'
    nginx.ingress.kubernetes.io/proxy-read-timeout: '480'
    nginx.ingress.kubernetes.io/proxy-send-timeout: '480'





Trino 性能对比

Cassandra 4个node

每个node cup 8， memory 20G

写超时20s， 读超时5秒



Trino 

coordinator

Cup 4 memory 4G worker 4个， 每个worker cup 2， memory 4G

graphql - cup 2， memory 8G

trino - up 2， memory 4G 

数据库 feature_jic_trino



group code Group_72619   - 20064 /  Group_83901 - 3

select count(*) from activity;   40 0869

select count(*) from exposure_test; 511 2710

select count(*) from portfolio;    23130

select count(*) from portfolio_property_by_tenant; 5

select count(*) from portfolio_property_by_portfolio;  12 4250

select  count(*) from performance_analysis;

select count(*), portfolio_group_code from portfolio_by_group group by portfolio_group_code

select  count(*) from portfolio_by_group;  807984

select count(*), portfolio_group_code from portfolio_by_group  where portfolio_group_code='ds_nestedgrp' group by portfolio_group_code



query MyQuery {
  portfolios {
    code
    closingMethod
    lastLoadedDate
    name
    openDate
    status
    taxStatus
  }
}

data 3.4M   23130 items

|         | graphql | 浏览器 |
| ------- | ------- | ------ |
| trino   | 7.46s   | 14.38s |
|         | 6.85s   | 13.84s |
| graphql | 7.71s   | 15.40s |
|         | 7.14s   | 14.04s |
| 10 + 24 | 7.18s   | 21.06s |

query MyQuery {
  portfolios {
    code
    closingMethod
    lastLoadedDate
    name
    openDate
    status
    taxStatus
    properties {
      code
      entities
      name
      type
      value
      valueCode
    }
  }
}

data 32M   23130 items 每个portfolio 有 10个properties

|         | graphql | 浏览器 |
| ------- | ------- | ------ |
| trino   | 1.0m    | 2.1m   |
|         | 1.0m    | 2.2m   |
| graphql | 2.5m    | 3.8m   |
|         | 2.5m    | 3.3m   |
| 10 + 24 | 2.5m    | 3.7m   |



**TODO**

query MyQuery {
  insights(portfolioGroupCode: "Group_83901") {
    totalAUM {
      total
    }
  }
}

Group_83901 - Group_72619

|         | graphql        | 浏览器   |
| ------- | -------------- | -------- |
| trino   | 5.32s（1.59s） | 6.21s    |
|         | 256.62ms       | 258.71ms |
| graphql | 528.09ms       | 1.10s    |
|         | 291.02m        | 293.07ms |
| 10 + 24 | 265.63ms       | 268.00ms |

|         | graphql  | 浏览器   |
| ------- | -------- | -------- |
| trino   | 4.77s    | 5.63s    |
|         | 324.00ms | 326.49ms |
| graphql | 7.59s    | 8.46s    |
|         | 6.36s    | 6.37s    |
| 10 + 24 | 6.28s    | 6.28s    |

query MyQuery {
  insights(portfolioGroupCode: "Group_72619") {
    summaryActivity(dateRange: MTD) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      contributions
      dividentsInterest
      endingMarketValue
      expenses
      fees
      feesAndExpenses
      gainAndLoss
      netFlows
      realizedGL
      unrealizedGL
      withdrawals
    }
  }
}

Group_83901 - Group_72619

|         | graphql  | 浏览器   |
| ------- | -------- | -------- |
| trino   | 1.28s    | 1.28s    |
|         | 1.54s    | 2.41s    |
| graphql | 589.43ms | 590.90ms |
|         | 564.80ms | 567.29s  |
| 10 + 24 | 515.38ms | 517.59ms |

|         | graphql | 浏览器 |
| ------- | ------- | ------ |
| trino   | 1.26s   | 1.26s  |
|         | 1.24s   | 1.24s  |
| graphql | 5.89s   | 6.38s  |
|         | 5.98s   | 5.99s  |
| 10 + 24 | 5.54s   | 5.54s  |



query MyQuery {
  insights(portfolioGroupCode: "Group_83901") {
    activities(dateRange: MTD) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      endingMarketValue
      portfolioCode
      portfolioName
      properties {
        code
        entities
        name
        type
        value
      }
    }
  }
}

Group_83901 - Group_72619

2.7k 2 items           39.5M

|         | graphql  | 浏览器   |
| ------- | -------- | -------- |
| trino   | 1.46s    | 1.46s    |
|         | 1.40s    | 1.40s    |
| graphql | 731.40ms | 1.64ms   |
|         | 542.26ms | 544.29ms |
| 10 + 24 | 575.63ms | 577.95ms |

|         | graphql        | 浏览器 |
| ------- | -------------- | ------ |
| trino   | 1.3m           | 2.7m   |
|         | 1.3m           | 3.1m   |
| graphql | / 5m/5.1m 超时 | /      |
|         | /              | /      |
| 10 + 24 | / 10m超时      | /      |

![image-20230323195356446](../../../../../Library/Application Support/typora-user-images/image-20230323195356446.png)

![image-20230323200017961](../../../../../Library/Application Support/typora-user-images/image-20230323200017961.png)

![image-20230323201514347](../../../../../Library/Application Support/typora-user-images/image-20230323201514347.png)

query MyQuery {
  insights(portfolioGroupCode: "Group_72619") {
    activities(dateRange: MTD, grouping: {aggregate: {value: sum}, groupby: "BROKER_REP"}) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      endingMarketValue
      portfolioCode
      portfolioName
      properties {
        code
        entities
        name
        type
        value
      }
    }
  }
}

Group_72619 15items 6.5k

|         | graphql | 浏览器 |
| ------- | ------- | ------ |
| trino   | 1.41s   | 1.41s  |
|         | 1.52s   | 1.52s  |
| graphql | 1.7m    | 1.7m   |
|         | 1.8m    | 1.8m   |
| 10 + 24 | 1.8m    | 1.8m   |

Group_83901

|         | graphql  | 浏览器   |
| ------- | -------- | -------- |
| trino   | 1.34s    | 2.16s    |
|         | 1.45s    | 1.45s    |
| graphql | 593.52ms | 1.07s    |
|         | 519.16ms | 520.72ms |
| 10 + 24 | 538.44ms | 556.73ms |





 select * from portfolio_by_group where tenant_id=600 and portfolio_group_code='Group_83901';



select 
      date(A.as_of_date) as as_of_date, 
      sum(A.total_market) as market_value 
    from 
      exposure_test A
    where 
      A.tenant_id = 600 
      and A.as_of_date = date '2023-03-18' 
      and A.di_load_date = timestamp '2023-03-20 01:06:12.422 UTC' 
     and A.portfolio_code in ('Port_17951', 'Port_17952', 'Port_17953')
    group by 
      A.as_of_date

![image-20230324110515889](../../../../../Library/Application Support/typora-user-images/image-20230324110515889.png)

![image-20230324110801305](../../../../../Library/Application Support/typora-user-images/image-20230324110801305.png)

select 
      date(A.as_of_date) as as_of_date, 
      sum(A.total_market) as market_value 
    from 
      exposure_test A
    where 
      A.tenant_id = 600 
      and A.as_of_date = date '2023-03-18' 
      and A.di_load_date = timestamp '2023-03-20 01:06:12.422 UTC' 
     and A.portfolio_code in ('Port_10000','Port_10001','Port_10002','Port_10003','Port_10004','Port_10005','Port_10006','Port_10007','Port_10008','Port_10009','Port_10010','Port_10011','Port_10012','Port_10013','Port_10014','Port_10015','Port_10016','Port_10017','Port_10018','Port_10019','Port_10020','Port_10021','Port_10022','Port_10023','Port_10024','Port_10025','Port_10026','Port_10027','Port_10028','Port_10029','Port_10030','Port_10031','Port_10032','Port_10033','Port_10034','Port_10035','Port_10036','Port_10037','Port_10038','Port_10039','Port_10040','Port_10041','Port_10042','Port_10043','Port_10044','Port_10045','Port_10046','Port_10047','Port_10048','Port_10049','Port_10050','Port_10051','Port_10052','Port_10053','Port_10054','Port_10055','Port_10056','Port_10057','Port_10058','Port_10059','Port_10060','Port_10062','Port_10063','Port_10064','Port_10065','Port_10066','Port_10067','Port_10068','Port_10069','Port_10070','Port_10071','Port_10072','Port_10073','Port_10074','Port_10075','Port_10076','Port_10077','Port_10078','Port_10079','Port_10080')
    group by 
      A.as_of_date



select * from (select date(A.as_of_date) as as_of_date ,sum(A.total_market) as market_value from exposure_test_clone A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='Group_83901') alias_portfolio_by_group where A.tenant_id=600 and A.as_of_date=date '2023-03-18' and A.di_load_date=timestamp '2023-03-20 01:06:12.422 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by A.as_of_date) A ORDER BY as_of_date DESC

Total AUM 慢的原因:

原本咱们这两个请求：（1）select * from portfolio_by_group where tenant_id=600 and portfolio_group_code='Group_83901';

（2）select    date(A.as_of_date) as as_of_date,    sum(A.total_market) as market_value   from    exposure_test A  where    A.tenant_id = 600    and A.as_of_date = date '2023-03-18'    and A.di_load_date = timestamp '2023-03-20 01:06:12.422 UTC'    and A.portfolio_code in ('Port_17951', 'Port_17952', 'Port_17953')  group by    A.as_of_date

合并成一个后

select * from (select date(A.as_of_date) as as_of_date ,sum(A.total_market) as market_value from exposure_test_clone2 A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group where alias_portfolio_by_group.portfolio_group_code='Group_72619') alias_portfolio_by_group where A.tenant_id=600 and A.as_of_date=date '2023-03-18' and A.di_load_date=timestamp '2023-03-20 01:06:12.422 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by A.as_of_date) A ORDER BY as_of_date DESC

 

给trino执行，sql执行计划里面分为多个其中 两个，一个是select * from portfolio_by_group where tenant_id=600 and portfolio_group_code='Group_83901';， 一个是select    date(A.as_of_date) as as_of_date,    sum(A.total_market) as market_value   from    exposure_test ，

第二个exposure 的filter tenant_id， as_of_date， di_load_date， portfolio_code 都没有，所以第二个执行计划慢，导致总体sql 运行时间慢，

而exposure filter 之所以没有生效是因为exposure_test 这样表的schema 设置为

CREATE TABLE feature_jic_trino.exposure_test_clone (

  as_of_date timestamp,

  asset_class_code text,

  asset_class_name text,

  di_load_date timestamp,

  industry_group_code text,

  industry_group_name text,

  investment_code text,

  investment_name text,

  investment_type_code text,

  investment_type_name text,

  is_short_asset boolean,

  portfolio_code text,

  portfolio_name text,

  price decimal,

  quantity decimal,

  sector_code text,

  sector_name text,

  tenant_id int,

  total_market decimal,

  weight decimal,

  PRIMARY KEY((portfolio_code, tenant_id, as_of_date)

portfolio_code 为第一个partitionkey，而咱们的sql 里面没有portfolio_code的filter ，所以以后顺序的partition key filter 都没有生效，

 

修改办法，Hao 把 exposure_test 这样table 修改为了PRIMARY KEY((tenant_id, as_of_date), portfolio_code, is_short_asset, investment_type_code, investment_code) 后，totalAUM 请求时间少了1s 左右

![image-20230324153202599](../../../../../Library/Application Support/typora-user-images/image-20230324153202599.png)

activity  PRIMARY KEY((tenant_id, portfolio_code, period_range_type), as_of_date)

exposure_test PRIMARY KEY((portfolio_code, tenant_id, as_of_date)



（1）Total aum 为什么 —————– 需要数据库结构修改

（2）更好的trino 掉用cassandra 数据方式

​            1 遵循数据库partition key 设置 顺序与规则 



（3）Cache 为什么生效有实效不确定

（4）超时 1m - 5/4/10m 不确定

（5） Cassandra 数据memory使用 超了

（6）graphql 黑盒在干嘛. -     1m (graphql 升级后会不会好点)

（7）ingress 设置不对



1000

============ before query 2023-03-27 11:41:47.969190
============ after query 2023-03-27 11:41:49.958435
============ graphql 2023-03-27 11:41:50.177600
============ on_request_end 2023-03-27 11:41:51.453962

2000

============ before query 2023-03-27 11:42:45.535409
============ after query 2023-03-27 11:42:47.637064
============ graphql 2023-03-27 11:42:48.091400
============ on_request_end 2023-03-27 11:42:50.722579



5000

============ before query 2023-03-27 11:43:11.737456
============ after query 2023-03-27 11:43:14.895629
============ graphql 2023-03-27 11:43:15.948919
============ on_request_end 2023-03-27 11:43:22.696765

10000

============ before query 2023-03-27 11:43:40.000903
============ after query 2023-03-27 11:43:44.396723
============ graphql 2023-03-27 11:43:46.493781
============ on_request_end 2023-03-27 11:43:59.853156



没有properties

============ before query 2023-03-27 11:45:13.655395
============ after query 2023-03-27 11:45:19.551781
============ graphql 2023-03-27 11:45:21.259995
============ on_request_end 2023-03-27 11:45:22.913147



============ before query 2023-03-27 11:46:08.406616
============ after query 2023-03-27 11:46:13.283257
============ graphql 2023-03-27 11:46:15.078668
============ on_request_end 2023-03-27 11:46:16.733853



20000

============ before query 2023-03-27 11:46:50.230757
============ after query 2023-03-27 11:46:57.881273
============ graphql 2023-03-27 11:47:01.325887
============ on_request_end 2023-03-27 11:47:04.681317

不带props

============ before query 2023-03-27 11:47:54.671949
============ after query 2023-03-27 11:48:02.137010
============ graphql 2023-03-27 11:48:06.499011
============ on_request_end 2023-03-27 11:48:09.864326

prop

============ before query 2023-03-27 11:48:22.633417
============ after query 2023-03-27 11:48:30.814553
============ graphql 2023-03-27 11:48:35.128206
============ on_request_end 2023-03-27 11:49:01.570908

子resolver 慢

50000

|                           | db   | graphql |
| ------------------------- | ---- | ------- |
| 不带prop不带resolver      | 11   | 5       |
| 不带prop                  | 11   | 5       |
| 带prop 不带resolver       | 10   | 30      |
|                           | 12   | 30      |
|                           |      |         |
| 带prop 带resolver         | 10   | 46      |
|                           | 11   | 30      |
| 升级后带prop 带resolver   | 10   | 40      |
| 升级后带prop 不带resolver | 8    | 32      |
| 数据结构不变 数组只一项   | 14   | 13      |
| async                     | 11   | 49      |
| 去掉async                 | 10   | 42      |
| DisableValidation         | 12   | 39      |
| ParserCache               | 9    | 40      |



============ before query 2023-03-27 13:45:57.266592
============ after query 2023-03-27 13:46:08.187919
============ graphql 2023-03-27 13:46:15.400291
============ on_request_end 2023-03-27 13:46:20.802193

对于我们的用例，我们向客户端发送几千个对象。 我们目前使用的是普通的 JSON API，但正在考虑改用 GraphQL。 但是，当返回几千个对象时，解析值的开销使其使用起来不切实际。 例如，下面的示例返回 10000 个带有 ID 字段的对象，运行大约需要 10 秒。

有没有推荐的方法来提高性能？ 到目前为止我成功使用的方法是使用现有的解析器来解析查询，然后通过直接创建字典来生成响应，这避免了解析/完成每个值的开销。

graphql

(1) graphql 嵌套结构返回慢

(2) 嵌套resolver

(3) 去掉async awite

(4) 创建dict 数据直接响应，避免每个值解析/完成 (没用)

(5) graphql升级(没用)

(6) 增大graphql cup memory (没用)

(7) graphql 换库（Graphene / strawberry， 或者不用graphql）

(8) 大数据 分页返回



Trino

(1) worker 数量, worker cup 与memory

db

(1) 改变数据库结构

(2) 合理join quey



Trino demo

Redis cache 时间/cup

trino 小数据慢， 大数据快， 原因      —-         cassandra 并行 值得吗？

2万条上下（小大数据分割线），201， JIC （SSNC）



Group_1100811q



23108 | Group_71184          
 23005 | Group_89301 
