19 getActivity

query getActivity {  asOfDates(entity: ACTIVITY) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ...summaryActivityFragment    activities(dateRange: YTD, grouping: {aggregate: {value: sum}, groupby: "CUSTODIAN"}) {      beginningMarketValue      endingMarketValue      changeMarketValuePercent      cashMarketValue      feesAndExpenses {        value        fees {          value          percent        }        expenses {          value        }      }      gainAndLoss {        percent        value        dividentsInterest {          value        }        realizedGL {          value        }        unrealizedGL {          value        }      }      netFlows {        value        percent        contributions {          value        }        withdrawals {          value        }      }      properties(codes: "$ACTIVITYGROUPBY") {        name        type        value      }    }  }}fragment summaryActivityFragment on Reports {  summaryActivity(dateRange: YTD) {    beginningMarketValue    cashMarketValue    contributions    dividentsInterest    endingMarketValue    expenses    fees    feesAndExpenses    gainAndLoss    netFlows    realizedGL    unrealizedGL    withdrawals    changeMarketValuePercent  }}

9个 sql

发现 

（1） portfolio_property_by_portfolio 慢，将来需要数据库提供table

5个stage

2 activity（18.60ms） ,4 portfolio_by_group（16.35ms）

3 portfolio_property_by_portfolio（1个worker， 1.31s）

1 = 2+3+4（4个worker， 1.33s， left join, aggregate, / inner join, localexchange/ aggregate）

0 output（4个worker 1.33s）



```sql
6个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	28.33ms
Queued Time	544.43us
Analysis Time	995.82us
Planning Time	12.82ms
Execution Time	26.86ms
```

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-14 14:34:00.642 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code

Elapsed Time	138.37ms
Queued Time	190.72us
Analysis Time	3.82ms
Planning Time	75.00ms
Execution Time	134.27ms
```

```sql
select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'

Elapsed Time	27.28ms
Queued Time	136.88us
Analysis Time	760.50us
Planning Time	14.87ms
Execution Time	26.30ms

```

```sql
select * from (select sum(A.beginning_mv) as beginning_mv ,sum(A.ending_mv) as ending_mv,sum(A.change_mv_pct) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,max(alias_pivot_portfolio_property.CUSTODIAN_DESC) as CUSTODIAN_DESC from activity A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-14 14:34:00.642 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by alias_pivot_portfolio_property.CUSTODIAN_CODE) A  OFFSET 0

Elapsed Time	1.46s
Queued Time	160.51us
Analysis Time	6.56ms
Planning Time	106.20ms
Execution Time	1.45s
```