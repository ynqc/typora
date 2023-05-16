2023年3月30日 星期四

10:16

Summary actiivty

 

Trino

```
select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-03-29 01:46:51.329 UTC'
```



select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='MTD' and A.di_load_date=timestamp '2023-03-29 01:46:52.366 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code 

 

Graphql 

select as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date from etl_audit where tenant_id=201

select portfolio_code as code, portfolio_name as name, portfolio_group_code as portfolio_group_code, tenant_id as tenant_id, di_load_date as last_loaded_date, portfolio_status as status, close_method as closing_method, tax_status as tax_status, start_date as open_date from portfolio_by_group where portfolio_group_code='ds_nestedgrp' and tenant_id=201

select as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date from etl_audit where tenant_id=201

select as_of_date as as_of_date, sum(beginning_mv) as beginning_mv, sum(ending_mv) as ending_mv, sum(change_mv_pct) as change_mv_pct, sum(cash_mv) as cash_mv, sum(net_flows) as net_flows, sum(net_flows_pct) as net_flows_pct, sum(contributions) as contributions, sum(withdrawals) as withdrawals, sum(fees) as fees, sum(fees_pct) as fees_pct, sum(expenses) as expenses, sum(gain_loss) as gain_loss, sum(gain_loss_pct) as gain_loss_pct, sum(unreal_gl) as unreal_gl, sum(real_gl) as real_gl, sum(dividends_interests) as dividends_interests, di_load_date as last_loaded_date from activity where portfolio_code in ('ds_cashonly','ds_average','ds_cpg','ds_mbs','ds_mixgrp','ds_mix','ds_mixacct','ds_multicash','ds_tips') and tenant_id=201 and period_range_type='MTD' and as_of_date=date '2022-04-30' group by as_of_date, di_load_date





Groupby 



trino

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

```
select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-03-29 01:46:51.329 UTC'
```

```
select * from (select sum(A.beginning_mv) as beginning_mv ,sum(A.ending_mv) as ending_mv,sum(A.change_mv_pct) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC as CUSTODIAN_DESC from activity A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='MTD' and A.di_load_date=timestamp '2023-03-29 01:46:52.366 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by alias_pivot_portfolio_property.CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC) A
```



graphql

select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

select  portfolio_code as code, portfolio_name as name, portfolio_group_code as portfolio_group_code, tenant_id as tenant_id, di_load_date as last_loaded_date, portfolio_status as status, close_method as closing_method, tax_status as tax_status, start_date as open_date  from portfolio_by_group where  portfolio_group_code='ds_nestedgrp' and tenant_id=201



select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201



select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

select  portfolio_code as portfolio_code, portfolio_property_name as name, portfolio_property_value_code as code, portfolio_property_value_desc as value, di_load_date as last_loaded_date  from portfolio_property_by_portfolio where  tenant_id=201 and portfolio_code in ('ds_cashonly','ds_average','ds_cpg','ds_mbs','ds_mixgrp','ds_mix','ds_mixacct','ds_multicash','ds_tips')

select  as_of_date as as_of_date, portfolio_name as portfolio_name, portfolio_code as portfolio_code, beginning_mv as beginning_mv, ending_mv as ending_mv, change_mv_pct as change_mv_pct, cash_mv as cash_mv, net_flows as net_flows, net_flows_pct as net_flows_pct, contributions as contributions, withdrawals as withdrawals, fees as fees, fees_pct as fees_pct, expenses as expenses, gain_loss as gain_loss, gain_loss_pct as gain_loss_pct, unreal_gl as unreal_gl, real_gl as real_gl, dividends_interests as dividends_interests, di_load_date as last_loaded_date  from activity where  portfolio_code in ('ds_cashonly','ds_average','ds_cpg','ds_mbs','ds_mixgrp','ds_mix','ds_mixacct','ds_multicash','ds_tips') and tenant_id=201 and period_range_type='MTD' and as_of_date='2022-04-30’





Filter

trino

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

```
select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-03-29 01:46:51.329 UTC'
```

```
select A.as_of_date as as_of_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.beginning_mv as beginning_mv,A.ending_mv as ending_mv,A.change_mv_pct as change_mv_pct,A.cash_mv as cash_mv,A.net_flows as net_flows,A.net_flows_pct as net_flows_pct,A.contributions as contributions,A.withdrawals as withdrawals,A.fees as fees,A.fees_pct as fees_pct,A.expenses as expenses,A.gain_loss as gain_loss,A.gain_loss_pct as gain_loss_pct,A.unreal_gl as unreal_gl,A.real_gl as real_gl,A.dividends_interests as dividends_interests,A.di_load_date as last_loaded_date,alias_pivot_portfolio_property.ANALYST_CODE as ANALYST_CODE,alias_pivot_portfolio_property.ANALYST_DESC as ANALYST_DESC,alias_pivot_portfolio_property.BASE_CURRENCY_CODE as BASE_CURRENCY_CODE,alias_pivot_portfolio_property.BASE_CURRENCY_DESC as BASE_CURRENCY_DESC,alias_pivot_portfolio_property.BROKER_REP_CODE as BROKER_REP_CODE,alias_pivot_portfolio_property.BROKER_REP_DESC as BROKER_REP_DESC,alias_pivot_portfolio_property.Benchmark_CODE as Benchmark_CODE,alias_pivot_portfolio_property.Benchmark_DESC as Benchmark_DESC,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC as CUSTODIAN_DESC,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_CODE as HNW_INSTITUTIONAL_CODE,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_DESC as HNW_INSTITUTIONAL_DESC,alias_pivot_portfolio_property.INVESTMENT_GOAL_CODE as INVESTMENT_GOAL_CODE,alias_pivot_portfolio_property.INVESTMENT_GOAL_DESC as INVESTMENT_GOAL_DESC,alias_pivot_portfolio_property.MANAGED_CODE as MANAGED_CODE,alias_pivot_portfolio_property.MANAGED_DESC as MANAGED_DESC,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_CODE as RELATIONSHIP_MANAGER_CODE,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_DESC as RELATIONSHIP_MANAGER_DESC from activity A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='MTD' and A.di_load_date=timestamp '2023-03-29 01:46:52.366 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code and ( alias_pivot_portfolio_property.CUSTODIAN_CODE = 'Unclassified') ORDER BY A.as_of_date DESC
```





graphql

select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

select  portfolio_code as code, portfolio_name as name, portfolio_group_code as portfolio_group_code, tenant_id as tenant_id, di_load_date as last_loaded_date, portfolio_status as status, close_method as closing_method, tax_status as tax_status, start_date as open_date  from portfolio_by_group where  portfolio_group_code='ds_nestedgrp' and tenant_id=201

select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

select  portfolio_code as portfolio_code, portfolio_property_name as name, portfolio_property_value_code as code, portfolio_property_value_desc as value, di_load_date as last_loaded_date  from portfolio_property_by_portfolio where  tenant_id=201 and portfolio_code in ('ds_cashonly','ds_average','ds_cpg','ds_mbs','ds_mixgrp','ds_mix','ds_mixacct','ds_multicash','ds_tips')

select  as_of_date as as_of_date, portfolio_name as portfolio_name, portfolio_code as portfolio_code, beginning_mv as beginning_mv, ending_mv as ending_mv, change_mv_pct as change_mv_pct, cash_mv as cash_mv, net_flows as net_flows, net_flows_pct as net_flows_pct, contributions as contributions, withdrawals as withdrawals, fees as fees, fees_pct as fees_pct, expenses as expenses, gain_loss as gain_loss, gain_loss_pct as gain_loss_pct, unreal_gl as unreal_gl, real_gl as real_gl, dividends_interests as dividends_interests, di_load_date as last_loaded_date  from activity where  portfolio_code in ('ds_cashonly','ds_average','ds_cpg','ds_mbs','ds_mixgrp','ds_mix','ds_mixacct','ds_multicash','ds_tips') and tenant_id=201 and period_range_type='MTD' and as_of_date='2022-04-30'