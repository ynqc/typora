14 getAllActiveReturnDeviations

query getAllActiveReturnDeviations {
  insights(portfolioGroupCode: "ds_nestedgrp") {
    mtd: activeReturnDeviations(period: MTD) {
      ...ActiveChartSchema
    }
  }
}

fragment ActiveChartSchema on ActiveReturnDeviationReport {
  deviations {
    deviation
  }
  mean
  standardDeviation
  portfolioCount
  sigmaIntervals {
    totalMarketValue
    portfolioCount
    proportionOfAUM
    sigmaFrom
    sigmaTo
  }
}

发现： 

（1）都符合条件，但是trino 版本就是慢， why？

(2) trino command line 执行快

```
3个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	42.08ms
Queued Time	1.30ms
Analysis Time	1.57ms
Planning Time	13.62ms
Execution Time	39.83ms
```

```sql
1个 select A.portfolio_code as portfolio_code,A.market_value as market_value,A.active_return_mtd as active_return,A.mean_mtd as mean,A.deviation_from_mean_mtd as deviation,A.standard_deviation_mtd as standard_deviation,A.di_load_date as last_loaded_date from standard_deviation A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.as_of_date=date '2022-04-30' and A.di_load_date=timestamp '2023-05-14 14:33:39.096 UTC' and A.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code

Elapsed Time	143.18ms
Queued Time	219.84us
Analysis Time	3.68ms
Planning Time	86.69ms
Execution Time	139.07ms
```