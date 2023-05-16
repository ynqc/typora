15 getPerformanceAnalysisGridQuery



query getPerformanceAnalysisGridQuery {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    performance {      marketValue      portfolioName      portfolioCode      mtd {        difference        index        portfolio      }      qtd {        difference        index        portfolio      }      ytd {        difference        index        portfolio      }      sinceInception {        difference        index        portfolio      }      oneYear {        difference        index        portfolio      }      threeYears {        difference        index        portfolio      }      fiveYears {        difference        index        portfolio      }      properties(codes: "ANALYST") {        code        name        type        value      }    }  }}

发现 

（1）长query 运行时间长， 5个stage， portfolio_property_by_portfolio 慢，将来需要数据库提供table

```
3个
20230515_093824_00083_be8r6
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	36.64ms
Queued Time	614.91us
Analysis Time	960.63us
Planning Time	11.72ms
Execution Time	35.16ms
```

```sql
select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'

Elapsed Time	30.35ms
Queued Time	148.91us
Analysis Time	807.72us
Planning Time	14.98ms
Execution Time	29.29ms
```

```sql
select A.as_of_date as as_of_date,A.di_load_date as last_loaded_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.market_value as market_value,A.portfolio_mtd as portfolio_mtd,A.benchmark_mtd as benchmark_mtd,(A.portfolio_mtd - A.benchmark_mtd) as difference_mtd,A.portfolio_qtd as portfolio_qtd,A.benchmark_qtd as benchmark_qtd,(A.portfolio_qtd - A.benchmark_qtd) as difference_qtd,A.portfolio_ytd as portfolio_ytd,A.benchmark_ytd as benchmark_ytd,(A.portfolio_ytd - A.benchmark_ytd) as difference_ytd,A.portfolio_last_one_year as portfolio_last_one_year,A.benchmark_last_one_year as benchmark_last_one_year,(A.portfolio_last_one_year - A.benchmark_last_one_year) as difference_last_one_year,A.portfolio_last_three_years as portfolio_last_three_years,A.benchmark_last_three_years as benchmark_last_three_years,(A.portfolio_last_three_years - A.benchmark_last_three_years) as difference_last_three_years,A.portfolio_last_five_years as portfolio_last_five_years,A.benchmark_last_five_years as benchmark_last_five_years,(A.portfolio_last_five_years - A.benchmark_last_five_years) as difference_last_five_years,A.portfolio_itd as portfolio_itd,A.benchmark_itd as benchmark_itd,(A.portfolio_itd - A.benchmark_itd) as difference_itd,alias_pivot_portfolio_property.ANALYST_CODE as ANALYST_CODE,alias_pivot_portfolio_property.ANALYST_DESC as ANALYST_DESC,alias_pivot_portfolio_property.BASE_CURRENCY_CODE as BASE_CURRENCY_CODE,alias_pivot_portfolio_property.BASE_CURRENCY_DESC as BASE_CURRENCY_DESC,alias_pivot_portfolio_property.BROKER_REP_CODE as BROKER_REP_CODE,alias_pivot_portfolio_property.BROKER_REP_DESC as BROKER_REP_DESC,alias_pivot_portfolio_property.Benchmark_CODE as Benchmark_CODE,alias_pivot_portfolio_property.Benchmark_DESC as Benchmark_DESC,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC as CUSTODIAN_DESC,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_CODE as HNW_INSTITUTIONAL_CODE,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_DESC as HNW_INSTITUTIONAL_DESC,alias_pivot_portfolio_property.INVESTMENT_GOAL_CODE as INVESTMENT_GOAL_CODE,alias_pivot_portfolio_property.INVESTMENT_GOAL_DESC as INVESTMENT_GOAL_DESC,alias_pivot_portfolio_property.MANAGED_CODE as MANAGED_CODE,alias_pivot_portfolio_property.MANAGED_DESC as MANAGED_DESC,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_CODE as RELATIONSHIP_MANAGER_CODE,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_DESC as RELATIONSHIP_MANAGER_DESC from performance_analysis A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:33:39.096 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code  OFFSET 0

Elapsed Time	1.89s
Queued Time	183.24us
Analysis Time	6.32ms
Planning Time	158.05ms
Execution Time	1.88s
```