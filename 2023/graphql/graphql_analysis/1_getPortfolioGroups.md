1 getPortfolioGroups

query getPortfolioGroups {  portfolioGroups {      code      name  }}

sql 2个

发现
（1）用portfolio_group 这张table不用portfolio_group_by_code 这张table，或者portfolio_group_by_code  改partition key（tenant_id, portfolio_group_code， 删除portfolio_group_code）
（2） join table 
（3）sortby（拿掉所有不必要sortby），sortby不是在cassandra scan上执行，多了一个stage最后执行

(4) 不稳定stage 4 扫描速度大不一样，某一个worker 网络不稳定， 待加强分析



```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	51.48ms
Queued Time	567.56us
Analysis Time	2.56ms
Planning Time	17.68ms
Execution Time	48.00ms

1个执行计划
ID	   Host	      State					    Rows	Rows/s	Bytes	Bytes/s	Elapsed	CPU   Time	Mem 	Peak Mem
0.0.0	10.42.6.78	FINISHED	0	0	0	1	8.00	560	     0	    0	    14.29ms	      3.80ms	0B	503B
```

```sql
select A.tenant_id as tenant_id,A.portfolio_group_code as code,A.di_load_date as last_loaded_date,A.portfolio_group_name as name,A.portfolio_group_type as group_type from portfolio_group_by_code A , portfolio_group_ui B where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:21:46.867 UTC' and B.tenant_id=A.tenant_id and B.portfolio_group_code=A.portfolio_group_code ORDER BY A.portfolio_group_name ASC 

Elapsed Time	2.57s
Queued Time	250.91us
Analysis Time	1.94ms
Planning Time	57.06ms
Execution Time	2.57s

5个执行计划（20230515_071822_00001_be8r6）



```

