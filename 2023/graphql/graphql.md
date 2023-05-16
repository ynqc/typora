ds_nestedgrp/auto_pa_esg_cbus

1 优化trino 往graphql 传数据时间

2 重复sql 请求

3 cache







none trino http://10.2.96.132:31989/

trino.  http://10.2.96.132:30434/

trino http://10.2.96.132:30478/



1 **query getPortfolioGroups {  portfolioGroups {      code      name  }}**

```sql
select A.tenant_id as tenant_id,A.portfolio_group_code as code,A.di_load_date as last_loaded_date,A.portfolio_group_name as name,A.portfolio_group_type as group_type from portfolio_group_by_code A , portfolio_group_ui B where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-10 01:10:33.460 UTC' and B.tenant_id=A.tenant_id and B.portfolio_group_code=A.portfolio_group_code ORDER BY A.portfolio_group_name ASC 
```

portfolio_group_by_code  - PRIMARY KEY((tenant_id, portfolio_group_code))

**time  549-  time 265**



2 query getPortfolioGroups {    portfolioGroups(code: "ds_nestedgrp") {      portfolios {        code        name      }    }  }

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_group pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-10 01:10:23.572 UTC' and pt.portfolio_group_code='ds_nestedgrp' ORDER BY pt.portfolio_name ASC
```

portfolio_by_group - PRIMARY KEY((tenant_id, portfolio_group_code), portfolio_code)

time 410 -  time 258



3 query getAllETLAsOfDate {        portfolio: asOfDates(entity: PORTFOLIO) {                date                etlStartTime            },portfolio_group: asOfDates(entity: PORTFOLIO_GROUP) {                date                etlStartTime            },exposure: asOfDates(entity: EXPOSURE) {                date                etlStartTime            },performance: asOfDates(entity: PERFORMANCE) {                date                etlStartTime            },activity: asOfDates(entity: ACTIVITY) {                date                etlStartTime            }       }

```sql
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

etl_audit - PRIMARY KEY(tenant_id, ui_feature)

time 615 -  time 541.5



4 query getExposureComparisonHeaderInfo {  portfolioGroups(code: "ds_nestedgrp") {    lastLoadedDate    portfolios {      code    }  }}

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_group pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-10 01:10:23.572 UTC' and pt.portfolio_group_code='ds_nestedgrp' ORDER BY pt.portfolio_name ASC
```

```sql
select A.tenant_id as tenant_id,A.portfolio_group_code as code,A.di_load_date as last_loaded_date,A.portfolio_group_name as name,A.portfolio_group_type as group_type from portfolio_group_by_code A , portfolio_group_ui B where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-10 01:10:33.460 UTC' and B.tenant_id=A.tenant_id and B.portfolio_group_code=A.portfolio_group_code and A.portfolio_group_code='ds_nestedgrp' ORDER BY A.portfolio_group_name ASC 
```

time 390 -  time 252



**5 query getExposureTopBottomChanges {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomHoldings(dateRange: YTD, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "ASSET_CLASS"}, limit: 3) {      top {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }      bottom {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }    }  }}**

**发了26个sql 查询，NO！！！！**

**time 3.19s -  time 429**



6query getExposureComparisonMultidatePresetDatesQuery {  asOfDates(entity: EXPOSURE) {    periodDates {      ...lastSevenDays      ...lastFourQuarters    }  }}fragment lastSevenDays on PeriodDates {  lastSevenDays: lastSevenDays {    dates(timeInterval: DAILY)    fromDate    toDate  }}fragment lastFourQuarters on PeriodDates {  lastFourQuarters: oneYear {    dates(timeInterval: QUARTERLY)    fromDate    toDate  }}

```sql
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

etl_audit - PRIMARY KEY(tenant_id, ui_feature)

time 474 -  time 461



7 query getExposureComparisonDatesQuery {  asOfDates(entity: EXPOSURE) {    periodDates {      mtd {        fromDate      }      qtd {        fromDate      }      ytd {        fromDate      }    }    date  }}

```sql
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 324 -  time 220



**8 query getExposureComparisonPositions {  asOfDates(entity: EXPOSURE) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    positions(dateRange: YTD, filters: [], grouping: {groupby: "ASSET_CLASS", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["ASSET_CLASS", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}**

N 个query

**time 2.58s -  time 569**



9 query getExposureComparisonLastLoadedDate {  asOfDates(entity: EXPOSURE) {    etlStartTime  }}

```sql
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 256 -  time 220



**10 query getExposureTopBottomChanges {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomHoldings(dateRange: $DATERANGEINUPPER, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "$EXPOSUREGROUPBY"}, limit: 3) {      top {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }      bottom {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "$EXPOSUREGROUPBY") {          code          name          value          type        }      }    }  }}**

**n个请求**

**time 3.45s -   time 1005**

**11 query getExposureComparisonPositions {  asOfDates(entity: EXPOSURE) {    periodDates {      fiveYears {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    positions(dateRange: FiveYears, filters: [], grouping: {groupby: "ASSET_CLASS", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["ASSET_CLASS", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}**

**n个请求**

**time 2.54s -   time 885**



12 query getTopBottomPerformersQuery {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomPerformers(dateRange: YTD, limit: 5) {      top {        difference        portfolioName        portfolioCode      }      bottom {        difference        portfolioName        portfolioCode      }    }  }}

```sql
select A.as_of_date as as_of_date,A.di_load_date as last_loaded_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.market_value as market_value,A.portfolio_mtd as portfolio_mtd,A.benchmark_mtd as benchmark_mtd,(A.portfolio_mtd - A.benchmark_mtd) as difference_mtd,A.portfolio_qtd as portfolio_qtd,A.benchmark_qtd as benchmark_qtd,(A.portfolio_qtd - A.benchmark_qtd) as difference_qtd,A.portfolio_ytd as portfolio_ytd,A.benchmark_ytd as benchmark_ytd,(A.portfolio_ytd - A.benchmark_ytd) as difference_ytd,A.portfolio_last_one_year as portfolio_last_one_year,A.benchmark_last_one_year as benchmark_last_one_year,(A.portfolio_last_one_year - A.benchmark_last_one_year) as difference_last_one_year,A.portfolio_last_three_years as portfolio_last_three_years,A.benchmark_last_three_years as benchmark_last_three_years,(A.portfolio_last_three_years - A.benchmark_last_three_years) as difference_last_three_years,A.portfolio_last_five_years as portfolio_last_five_years,A.benchmark_last_five_years as benchmark_last_five_years,(A.portfolio_last_five_years - A.benchmark_last_five_years) as difference_last_five_years,A.portfolio_itd as portfolio_itd,A.benchmark_itd as benchmark_itd,(A.portfolio_itd - A.benchmark_itd) as difference_itd from performance_analysis A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:47:08.297 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code ORDER BY (A.portfolio_ytd - A.benchmark_ytd) desc,A.portfolio_code asc LIMIT 5
```

time 625 -  time 517



13 query getTopBottomPerformersQuery {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomPerformers(dateRange: MTD, limit: 5) {      top {        difference        portfolioName        portfolioCode      }      bottom {        difference        portfolioName        portfolioCode      }    }  }}

YTD/ITD/ThreeYears/FiveYears....

```sql
select A.as_of_date as as_of_date,A.di_load_date as last_loaded_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.market_value as market_value,A.portfolio_mtd as portfolio_mtd,A.benchmark_mtd as benchmark_mtd,(A.portfolio_mtd - A.benchmark_mtd) as difference_mtd,A.portfolio_qtd as portfolio_qtd,A.benchmark_qtd as benchmark_qtd,(A.portfolio_qtd - A.benchmark_qtd) as difference_qtd,A.portfolio_ytd as portfolio_ytd,A.benchmark_ytd as benchmark_ytd,(A.portfolio_ytd - A.benchmark_ytd) as difference_ytd,A.portfolio_last_one_year as portfolio_last_one_year,A.benchmark_last_one_year as benchmark_last_one_year,(A.portfolio_last_one_year - A.benchmark_last_one_year) as difference_last_one_year,A.portfolio_last_three_years as portfolio_last_three_years,A.benchmark_last_three_years as benchmark_last_three_years,(A.portfolio_last_three_years - A.benchmark_last_three_years) as difference_last_three_years,A.portfolio_last_five_years as portfolio_last_five_years,A.benchmark_last_five_years as benchmark_last_five_years,(A.portfolio_last_five_years - A.benchmark_last_five_years) as difference_last_five_years,A.portfolio_itd as portfolio_itd,A.benchmark_itd as benchmark_itd,(A.portfolio_itd - A.benchmark_itd) as difference_itd from performance_analysis A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:47:08.297 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code ORDER BY (A.portfolio_mtd - A.benchmark_mtd) desc,A.portfolio_code asc LIMIT 5
```

time 520 -  time 610

time 541 -  time 375

14  query getAllActiveReturnDeviations {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    mtd: activeReturnDeviations(period: MTD) {      ...ActiveChartSchema    }    qtd: activeReturnDeviations(period: QTD) {      ...ActiveChartSchema    }    ytd: activeReturnDeviations(period: YTD) {      ...ActiveChartSchema    }    sinceInception: activeReturnDeviations(period: ITD) {      ...ActiveChartSchema    }    oneYear: activeReturnDeviations(period: OneYear) {      ...ActiveChartSchema    }    threeYears: activeReturnDeviations(period: ThreeYears) {      ...ActiveChartSchema    }    fiveYears: activeReturnDeviations(period: FiveYears) {      ...ActiveChartSchema    }  }}fragment ActiveChartSchema on ActiveReturnDeviationReport {  deviations {    deviation  }  mean  standardDeviation  portfolioCount  sigmaIntervals {    totalMarketValue    portfolioCount    proportionOfAUM    sigmaFrom    sigmaTo  }}

n个请求

time 1.47s -  time 528



15 query getPerformanceAnalysisGridQuery {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    performance {      marketValue      portfolioName      portfolioCode      mtd {        difference        index        portfolio      }      qtd {        difference        index        portfolio      }      ytd {        difference        index        portfolio      }      sinceInception {        difference        index        portfolio      }      oneYear {        difference        index        portfolio      }      threeYears {        difference        index        portfolio      }      fiveYears {        difference        index        portfolio      }      properties(codes: "ANALYST") {        code        name        type        value      }    }  }}

```sql
select A.as_of_date as as_of_date,A.di_load_date as last_loaded_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.market_value as market_value,A.portfolio_mtd as portfolio_mtd,A.benchmark_mtd as benchmark_mtd,(A.portfolio_mtd - A.benchmark_mtd) as difference_mtd,A.portfolio_qtd as portfolio_qtd,A.benchmark_qtd as benchmark_qtd,(A.portfolio_qtd - A.benchmark_qtd) as difference_qtd,A.portfolio_ytd as portfolio_ytd,A.benchmark_ytd as benchmark_ytd,(A.portfolio_ytd - A.benchmark_ytd) as difference_ytd,A.portfolio_last_one_year as portfolio_last_one_year,A.benchmark_last_one_year as benchmark_last_one_year,(A.portfolio_last_one_year - A.benchmark_last_one_year) as difference_last_one_year,A.portfolio_last_three_years as portfolio_last_three_years,A.benchmark_last_three_years as benchmark_last_three_years,(A.portfolio_last_three_years - A.benchmark_last_three_years) as difference_last_three_years,A.portfolio_last_five_years as portfolio_last_five_years,A.benchmark_last_five_years as benchmark_last_five_years,(A.portfolio_last_five_years - A.benchmark_last_five_years) as difference_last_five_years,A.portfolio_itd as portfolio_itd,A.benchmark_itd as benchmark_itd,(A.portfolio_itd - A.benchmark_itd) as difference_itd,alias_pivot_portfolio_property.ANALYST_CODE as ANALYST_CODE,alias_pivot_portfolio_property.ANALYST_DESC as ANALYST_DESC,alias_pivot_portfolio_property.BASE_CURRENCY_CODE as BASE_CURRENCY_CODE,alias_pivot_portfolio_property.BASE_CURRENCY_DESC as BASE_CURRENCY_DESC,alias_pivot_portfolio_property.BROKER_REP_CODE as BROKER_REP_CODE,alias_pivot_portfolio_property.BROKER_REP_DESC as BROKER_REP_DESC,alias_pivot_portfolio_property.Benchmark_CODE as Benchmark_CODE,alias_pivot_portfolio_property.Benchmark_DESC as Benchmark_DESC,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC as CUSTODIAN_DESC,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_CODE as HNW_INSTITUTIONAL_CODE,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_DESC as HNW_INSTITUTIONAL_DESC,alias_pivot_portfolio_property.INVESTMENT_GOAL_CODE as INVESTMENT_GOAL_CODE,alias_pivot_portfolio_property.INVESTMENT_GOAL_DESC as INVESTMENT_GOAL_DESC,alias_pivot_portfolio_property.MANAGED_CODE as MANAGED_CODE,alias_pivot_portfolio_property.MANAGED_DESC as MANAGED_DESC,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_CODE as RELATIONSHIP_MANAGER_CODE,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_DESC as RELATIONSHIP_MANAGER_DESC from performance_analysis A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:47:08.297 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code  OFFSET 0
```

time 3.15s time 506



16 query getPerformanceLastLoadedDate {        asOfDates(entity: PERFORMANCE) {            etlStartTime        }    }

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 249 time 218

17 query getActivityDatesQuery {  asOfDates(entity: ACTIVITY) {    etlStartTime    periodDates {      mtd {        fromDate        toDate      }      qtd {        fromDate        toDate      }      ytd {        fromDate        toDate      }    }  }}

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 273 time 220



18 query ActivitySummary {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    summaryActivity(dateRange: YTD) {      beginningMarketValue      cashMarketValue      contributions      dividentsInterest      endingMarketValue      expenses      fees      feesAndExpenses      gainAndLoss      netFlows      realizedGL      unrealizedGL      withdrawals      changeMarketValuePercent    }  }}

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code
```

time 628 time 475



19 query getActivity {  asOfDates(entity: ACTIVITY) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ...summaryActivityFragment    activities(dateRange: YTD, grouping: {aggregate: {value: sum}, groupby: "CUSTODIAN"}) {      beginningMarketValue      endingMarketValue      changeMarketValuePercent      cashMarketValue      feesAndExpenses {        value        fees {          value          percent        }        expenses {          value        }      }      gainAndLoss {        percent        value        dividentsInterest {          value        }        realizedGL {          value        }        unrealizedGL {          value        }      }      netFlows {        value        percent        contributions {          value        }        withdrawals {          value        }      }      properties(codes: "$ACTIVITYGROUPBY") {        name        type        value      }    }  }}fragment summaryActivityFragment on Reports {  summaryActivity(dateRange: YTD) {    beginningMarketValue    cashMarketValue    contributions    dividentsInterest    endingMarketValue    expenses    fees    feesAndExpenses    gainAndLoss    netFlows    realizedGL    unrealizedGL    withdrawals    changeMarketValuePercent  }}

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code
```

```sql
select * from (select sum(A.beginning_mv) as beginning_mv ,sum(A.ending_mv) as ending_mv,sum(A.change_mv_pct) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,max(alias_pivot_portfolio_property.CUSTODIAN_DESC) as CUSTODIAN_DESC from activity A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by alias_pivot_portfolio_property.CUSTODIAN_CODE) A  OFFSET 0
```

time 2.6s time 542



20 query getPortfolioDetailInfo {        portfolios(code: "181GTC2") {            name          }    }

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_code='181GTC2' ORDER BY pt.portfolio_name ASC
```

time 616 time 542



21 query getPortfolioDetailLastLoadedDate {  asOfDates(entity: PORTFOLIO) {    etlStartTime  }}

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 303 time 263

22 query GetTMV {  portfolios(code: "$PORTFOLIO") {    summaryActivity(dateRange: MTD) {      endingMarketValue    }  }}

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC
```

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A  where A.tenant_id=201 and A.period_range_type='MTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and A.portfolio_code  in ('auto_pa_esg_cbus')
```

time 437 time 661 



23 query getExposureComparison {  portfolios(code: "$PORTFOLIO") {    topBottomHoldings(dateRange: YTD, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "ASSET_CLASS"}, limit: 8) {      top {        aumChangePercent        aumPercent        marketValueChange        properties(codes: "$PORTFOLIO") {          code          name          value          type        }      }    }  }}

n个请求

time 3.35s time 1.09s



24 query GetTotalCash {  portfolios(code: "$PORTFOLIO") {    summaryActivity(dateRange: MTD) {      cashMarketValue      endingMarketValue    }  }}

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A  where A.tenant_id=201 and A.period_range_type='MTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and A.portfolio_code  in ('auto_pa_esg_cbus')
```

time 424 time 337



25 query GetAttributes {  portfolios(code: "$PORTFOLIO") {    openDate    closingMethod    status    taxStatus    properties(codes: "CUSTODIAN") {      code      value      type    }  }}

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date,alias_pivot_portfolio_property.ANALYST_CODE as ANALYST_CODE,alias_pivot_portfolio_property.ANALYST_DESC as ANALYST_DESC,alias_pivot_portfolio_property.BASE_CURRENCY_CODE as BASE_CURRENCY_CODE,alias_pivot_portfolio_property.BASE_CURRENCY_DESC as BASE_CURRENCY_DESC,alias_pivot_portfolio_property.BROKER_REP_CODE as BROKER_REP_CODE,alias_pivot_portfolio_property.BROKER_REP_DESC as BROKER_REP_DESC,alias_pivot_portfolio_property.Benchmark_CODE as Benchmark_CODE,alias_pivot_portfolio_property.Benchmark_DESC as Benchmark_DESC,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,alias_pivot_portfolio_property.CUSTODIAN_DESC as CUSTODIAN_DESC,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_CODE as HNW_INSTITUTIONAL_CODE,alias_pivot_portfolio_property.HNW_INSTITUTIONAL_DESC as HNW_INSTITUTIONAL_DESC,alias_pivot_portfolio_property.INVESTMENT_GOAL_CODE as INVESTMENT_GOAL_CODE,alias_pivot_portfolio_property.INVESTMENT_GOAL_DESC as INVESTMENT_GOAL_DESC,alias_pivot_portfolio_property.MANAGED_CODE as MANAGED_CODE,alias_pivot_portfolio_property.MANAGED_DESC as MANAGED_DESC,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_CODE as RELATIONSHIP_MANAGER_CODE,alias_pivot_portfolio_property.RELATIONSHIP_MANAGER_DESC as RELATIONSHIP_MANAGER_DESC from portfolio_by_code pt left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = pt.portfolio_code where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC
```

time 615 time 446



26 query GetPerformanceGraph {  portfolios(code: "$PORTFOLIO") {    performance {      mtd {        portfolio        index        difference      }      qtd {        portfolio        index        difference      }      ytd {        portfolio        index        difference      }      oneYear {        portfolio        index        difference      }      threeYears {        portfolio        index        difference      }      fiveYears {        portfolio        index        difference      }      sinceInception {        portfolio        index        difference      }    }  }}

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC
```

```sql
select A.as_of_date as as_of_date,A.di_load_date as last_loaded_date,A.portfolio_name as portfolio_name,A.portfolio_code as portfolio_code,A.market_value as market_value,A.portfolio_mtd as portfolio_mtd,A.benchmark_mtd as benchmark_mtd,(A.portfolio_mtd - A.benchmark_mtd) as difference_mtd,A.portfolio_qtd as portfolio_qtd,A.benchmark_qtd as benchmark_qtd,(A.portfolio_qtd - A.benchmark_qtd) as difference_qtd,A.portfolio_ytd as portfolio_ytd,A.benchmark_ytd as benchmark_ytd,(A.portfolio_ytd - A.benchmark_ytd) as difference_ytd,A.portfolio_last_one_year as portfolio_last_one_year,A.benchmark_last_one_year as benchmark_last_one_year,(A.portfolio_last_one_year - A.benchmark_last_one_year) as difference_last_one_year,A.portfolio_last_three_years as portfolio_last_three_years,A.benchmark_last_three_years as benchmark_last_three_years,(A.portfolio_last_three_years - A.benchmark_last_three_years) as difference_last_three_years,A.portfolio_last_five_years as portfolio_last_five_years,A.benchmark_last_five_years as benchmark_last_five_years,(A.portfolio_last_five_years - A.benchmark_last_five_years) as difference_last_five_years,A.portfolio_itd as portfolio_itd,A.benchmark_itd as benchmark_itd,(A.portfolio_itd - A.benchmark_itd) as difference_itd from performance_analysis A  where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:47:08.297 UTC' and A.portfolio_code='auto_pa_esg_cbus'  OFFSET 0
```

time 616 time 540



27 query getActivityDatesQuery {  asOfDates(entity: ACTIVITY) {    etlStartTime    periodDates {      mtd {        fromDate        toDate      }      qtd {        fromDate        toDate      }      ytd {        fromDate        toDate      }    }  }}

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 486 time 459



28 query getPortfolioExposurePositions {  asOfDates(entity: EXPOSURE) {    periodDates {      lastDay {        fromDate        toDate      }    }  }  portfolios(code: "$PORTFOLIO") {    positions(dateRange: LastDay, filters: [], grouping: {groupby: "ASSET_CLASS", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["ASSET_CLASS", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}

n个sql

time 2.96 time 726



29 query getExposureComparisonDatesQuery {  asOfDates(entity: EXPOSURE) {    periodDates {      mtd {        fromDate      }      qtd {        fromDate      }      ytd {        fromDate      }    }    date  }}

```
select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
```

time 462 time 440



30 query ActivitySummaryPortDetails {  portfolios(code: "$PORTFOLIO") {    ...summaryActivityFragment  }}fragment summaryActivityFragment on Portfolio {  summaryActivity(dateRange: YTD) {    beginningMarketValue    cashMarketValue    contributions    dividentsInterest    endingMarketValue    expenses    fees    feesAndExpenses    gainAndLoss    netFlows    realizedGL    unrealizedGL    withdrawals    changeMarketValuePercent  }}

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC
```

```sql
select sum(A.beginning_mv) as beginning_mv,sum(A.ending_mv) as ending_mv,(sum(A.ending_mv)-sum(A.beginning_mv))/sum(A.beginning_mv) as change_mv_pct,sum(A.cash_mv) as cash_mv,sum(A.net_flows) as net_flows,sum(A.net_flows_pct) as net_flows_pct,sum(A.contributions) as contributions,sum(A.withdrawals) as withdrawals,sum(A.fees) + sum(A.expenses) as fees_expenses,sum(A.fees) as fees,sum(A.fees_pct) as fees_pct,sum(A.expenses) as expenses,sum(A.gain_loss) as gain_loss,sum(A.gain_loss_pct) as gain_loss_pct,sum(A.unreal_gl) as unreal_gl,sum(A.real_gl) as real_gl,sum(A.dividends_interests) as dividends_interests from activity A  where A.tenant_id=201 and A.period_range_type='YTD' and A.di_load_date=timestamp '2023-05-11 00:47:07.963 UTC' and A.portfolio_code  in ('auto_pa_esg_cbus')
```

time 631 time 501



31  CUSTODIAN

query getActivity {  asOfDates(entity: ACTIVITY) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ...summaryActivityFragment    activities(dateRange: YTD, grouping: {aggregate: {value: sum}, groupby: "$ACTIVITYGROUPBY"}) {      beginningMarketValue      endingMarketValue      changeMarketValuePercent      cashMarketValue      feesAndExpenses {        value        fees {          value          percent        }        expenses {          value        }      }      gainAndLoss {        percent        value        dividentsInterest {          value        }        realizedGL {          value        }        unrealizedGL {          value        }      }      netFlows {        value        percent        contributions {          value        }        withdrawals {          value        }      }      properties(codes: "$ACTIVITYGROUPBYNAME") {        name        type        value      }    }  }}fragment summaryActivityFragment on Reports {  summaryActivity(dateRange: YTD) {    beginningMarketValue    cashMarketValue    contributions    dividentsInterest    endingMarketValue    expenses    fees    feesAndExpenses    gainAndLoss    netFlows    realizedGL    unrealizedGL    withdrawals    changeMarketValuePercent  }}

n个请求

time 2.43 time 442



32  ANALYST

query getExposureComparisonPositions {  asOfDates(entity: EXPOSURE) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    positions(dateRange: YTD, filters: [], grouping: {groupby: "$EXPOSUREGROUPBY", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["$EXPOSUREGROUPBY", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}

n个请求

time 2.24s time 514



33 graphql error

query getPerformanceAnalysisGridQuery {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    performance (groupings: "CUSTODIAN") {      marketValue      portfolioName      portfolioCode      mtd {        difference        index        portfolio      }      qtd {        difference        index        portfolio      }      ytd {        difference        index        portfolio      }      sinceInception {        difference        index        portfolio      }      oneYear {        difference        index        portfolio      }      threeYears {        difference        index        portfolio      }      fiveYears {        difference        index        portfolio      }      properties(codes: "$PAGROUPBY") {        code        name        type        value      }    }  }}

```sql
select * from (select sum(A.market_value) as market_value ,sum(A.portfolio_mtd) as portfolio_mtd,sum(A.benchmark_mtd) as benchmark_mtd,(sum(A.portfolio_mtd) - sum(A.benchmark_mtd)) as difference_mtd,sum(A.portfolio_qtd) as portfolio_qtd,sum(A.benchmark_qtd) as benchmark_qtd,(sum(A.portfolio_qtd) - sum(A.benchmark_qtd)) as difference_qtd,sum(A.portfolio_ytd) as portfolio_ytd,sum(A.benchmark_ytd) as benchmark_ytd,(sum(A.portfolio_ytd) - sum(A.benchmark_ytd)) as difference_ytd,sum(A.portfolio_last_one_year) as portfolio_last_one_year,sum(A.benchmark_last_one_year) as benchmark_last_one_year,(sum(A.portfolio_last_one_year) - sum(A.benchmark_last_one_year)) as difference_last_one_year,sum(A.portfolio_last_three_years) as portfolio_last_three_years,sum(A.benchmark_last_three_years) as benchmark_last_three_years,(sum(A.portfolio_last_three_years) - sum(A.benchmark_last_three_years)) as difference_last_three_years,sum(A.portfolio_last_five_years) as portfolio_last_five_years,sum(A.benchmark_last_five_years) as benchmark_last_five_years,(sum(A.portfolio_last_five_years) - sum(A.benchmark_last_five_years)) as difference_last_five_years,sum(A.portfolio_itd) as portfolio_itd,sum(A.benchmark_itd) as benchmark_itd,(sum(A.portfolio_itd) -sum(A.benchmark_itd)) as difference_itd,A.as_of_date as as_of_date,alias_pivot_portfolio_property.CUSTODIAN_CODE as CUSTODIAN_CODE,max(alias_pivot_portfolio_property.CUSTODIAN_DESC) as CUSTODIAN_DESC from performance_analysis A left join (select portfolio_code as portfolio_code,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN portfolio_property_name='ANALYST' THEN portfolio_property_value_desc END) as ANALYST_DESC,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_code END) as BASE_CURRENCY_CODE,MAX(CASE WHEN portfolio_property_name='BASE_CURRENCY' THEN portfolio_property_value_desc END) as BASE_CURRENCY_DESC,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_code END) as BROKER_REP_CODE,MAX(CASE WHEN portfolio_property_name='BROKER_REP' THEN portfolio_property_value_desc END) as BROKER_REP_DESC,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_code END) as Benchmark_CODE,MAX(CASE WHEN portfolio_property_name='Benchmark' THEN portfolio_property_value_desc END) as Benchmark_DESC,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_code END) as CUSTODIAN_CODE,MAX(CASE WHEN portfolio_property_name='CUSTODIAN' THEN portfolio_property_value_desc END) as CUSTODIAN_DESC,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_code END) as HNW_INSTITUTIONAL_CODE,MAX(CASE WHEN portfolio_property_name='HNW_INSTITUTIONAL' THEN portfolio_property_value_desc END) as HNW_INSTITUTIONAL_DESC,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_code END) as INVESTMENT_GOAL_CODE,MAX(CASE WHEN portfolio_property_name='INVESTMENT_GOAL' THEN portfolio_property_value_desc END) as INVESTMENT_GOAL_DESC,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_code END) as MANAGED_CODE,MAX(CASE WHEN portfolio_property_name='MANAGED' THEN portfolio_property_value_desc END) as MANAGED_DESC,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_code END) as RELATIONSHIP_MANAGER_CODE,MAX(CASE WHEN portfolio_property_name='RELATIONSHIP_MANAGER' THEN portfolio_property_value_desc END) as RELATIONSHIP_MANAGER_DESC from portfolio_property_by_portfolio  where tenant_id=201 group by portfolio_code) alias_pivot_portfolio_property on alias_pivot_portfolio_property.portfolio_code = A.portfolio_code, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:47:08.297 UTC' and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by alias_pivot_portfolio_property.CUSTODIAN_CODE,A.as_of_date) A  OFFSET 0
```

time 2.85 time 434



34 query ESG_risk_dist_tile {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    riskDistribution {      asOfDate      distributions {        category        marketValue        weight      }      effectiveDate    }  }}

```
select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata
```

```sql
select esg.as_of_date as as_of_date,esg.id_181110142899 as id_181110142899,sum(esg.market_value) as market_value,max(esg.max_effective_date) as max_effective_date,sum(esg.id_181110112399 * esg.market_value /21938291.033002943) as id_181110112399,sum(esg.id_181137112399 * esg.market_value /21938291.033002943) as id_181137112399,sum(esg.id_181137192399 * esg.market_value /21938291.033002943) as id_181137192399,sum(esg.id_181137272399 * esg.market_value /21938291.033002943) as id_181137272399,sum(esg.id_191211112399 * esg.market_value /21938291.033002943) as id_191211112399,sum(esg.id_191111132799 * esg.market_value /21938291.033002943) as id_191111132799,sum(esg.id_191211152799 * esg.market_value /21938291.033002943) as id_191211152799 from exposure_esg_pivot esg , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg.tenant_id=201 and esg.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and esg.as_of_date=timestamp '2022-04-30' and alias_portfolio_by_group.tenant_id=esg.tenant_id and alias_portfolio_by_group.portfolio_code=esg.portfolio_code group by esg.tenant_id,esg.as_of_date,id_181110142899
```

```sql
select esg.tenant_id as tenant_id,max(esg.max_effective_date) as max_effective_date,sum(esg.market_value) as total_market_value,esg.as_of_date as as_of_date from exposure_esg_pivot esg , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg.tenant_id=201 and esg.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and esg.as_of_date=timestamp '2022-04-30' and alias_portfolio_by_group.tenant_id=esg.tenant_id and alias_portfolio_by_group.portfolio_code=esg.portfolio_code group by esg.tenant_id,esg.as_of_date
```

time 801 time 492



35 query getExposureComparisonHeaderInfo {  portfolioGroups(code: "$PORTFOLIO_GROUP") {    lastLoadedDate    portfolios {      code    }  }}

```sql
select A.tenant_id as tenant_id,A.portfolio_group_code as code,A.di_load_date as last_loaded_date,A.portfolio_group_name as name,A.portfolio_group_type as group_type from portfolio_group_by_code A , portfolio_group_ui B where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-11 00:17:49.316 UTC' and B.tenant_id=A.tenant_id and B.portfolio_group_code=A.portfolio_group_code and A.portfolio_group_code='ds_nestedgrp' ORDER BY A.portfolio_group_name ASC 
```

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_group pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC' and pt.portfolio_group_code='ds_nestedgrp' ORDER BY pt.portfolio_name ASC
```

time 426 time 548



36 query getExposureTopBottomChanges {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomHoldings(dateRange: $DATERANGEEXPORESGINUPPER, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "ASSET_CLASS"}, limit: 3) {      top {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }      bottom {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }    }  }}

n个sql

time 2.92s  time 858



37 query ESG_Summary_total {  insights(portfolioGroupCode: "auto_pa_esg_top") {    summaryESG(asOfDates: ["2022-04-30"]) {      asOfDate      ESGRisk {        category        governanceRiskScore        exposureClassification        environmentalRiskScore        socialRiskScore        score        managementClassification      }      carbonRisk {        carbonEmissions        score        carbonIntensity        category      }      marketValue    }  }}

```
select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata
```

```sql
select sum(esg_group.market_value) as total_market_value,esg_group.as_of_date as as_of_date from exposure_esg_grouped_pivot esg_group , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='auto_pa_esg_top' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg_group.tenant_id=201 and esg_group.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and esg_group.as_of_date  in ( timestamp '2022-04-30') and esg_group.id_181110112399  is not null and alias_portfolio_by_group.tenant_id=esg_group.tenant_id and alias_portfolio_by_group.portfolio_code=esg_group.portfolio_code group by esg_group.tenant_id,esg_group.as_of_date
```

```sql
select esg_group.as_of_date as as_of_date,CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END as total_market_value,sum(esg_group.id_181110112399 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_181110112399,sum(esg_group.id_181137112399 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_181137112399,sum(esg_group.id_181137192399 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_181137192399,sum(esg_group.id_181137272399 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_181137272399,sum(esg_group.id_191211112399 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_191211112399,sum(esg_group.id_191111132799 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_191111132799,sum(esg_group.id_191211152799 * esg_group.market_value /(CASE when esg_group.as_of_date=date '2022-04-30' then 10307630.631139733  END)) as id_191211152799 from exposure_esg_grouped_pivot esg_group , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='auto_pa_esg_top' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg_group.tenant_id=201 and esg_group.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and esg_group.as_of_date  in ( timestamp '2022-04-30') and alias_portfolio_by_group.tenant_id=esg_group.tenant_id and alias_portfolio_by_group.portfolio_code=esg_group.portfolio_code group by esg_group.tenant_id,esg_group.as_of_date
```

time 1.28s  time 674



38 query ESG_Category_L1 {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ESG(filters: [{andOr: OR, field: "as_of_date", value: "2022-04-30", operator: EQUAL}], grouping: {groupby: "id_181110142899", aggregations: {method: sum, field: ""}}) {      investmentId      investmentName      marketValue      portfolioCode      portfolioName      properties {        entities        code        type        name        value        valueCode      }    }  }}

```
select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata
```

```sql
select * from (select esg_group.as_of_date as as_of_date ,esg_group.id_181110142899 as id_181110142899,sum(esg_group.id_181110112399 * esg_group.market_value) as id_181110112399,sum(esg_group.id_181137112399 * esg_group.market_value) as id_181137112399,sum(esg_group.id_181137192399 * esg_group.market_value) as id_181137192399,sum(esg_group.id_181137272399 * esg_group.market_value) as id_181137272399,sum(esg_group.id_191211112399 * esg_group.market_value) as id_191211112399,sum(esg_group.id_191111132799 * esg_group.market_value) as id_191111132799,sum(esg_group.id_191211152799 * esg_group.market_value) as id_191211152799,sum(esg_group.market_value) as market_value from exposure_esg_grouped_pivot esg_group , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='$PORTFOLIO_GROUP' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg_group.tenant_id=201 and esg_group.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and alias_portfolio_by_group.tenant_id=esg_group.tenant_id and alias_portfolio_by_group.portfolio_code=esg_group.portfolio_code and ( esg_group.as_of_date = timestamp '2022-04-30') group by esg_group.tenant_id,esg_group.as_of_date,esg_group.id_181110142899) esg_group ORDER BY id_181110142899 ASC
```

time 826  time 528



39 query ESG_Portfolio_L2 {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ESG(filters: [{andOr: OR, field: "as_of_date", value: "$FROMORTODATE", operator: EQUAL}, {andOr: AND, field: "id_181110142899", value: "Unclassified", operator: EQUAL}], grouping: {groupby: "portfolio_code", aggregations: {method: sum, field: ""}}) {      investmentId      investmentName      marketValue      portfolioCode      portfolioName      properties {        entities        code        type        name        value        valueCode      }    }  }}

```sql
select esg.as_of_date as as_of_date,esg.portfolio_code as portfolio_code,esg.portfolio_name as portfolio_name,esg.id_181110112399 as id_181110112399,esg.id_181110142899 as id_181110142899,esg.id_181110182899 as id_181110182899,esg.id_181110202899 as id_181110202899,esg.id_181137112399 as id_181137112399,esg.id_181137192399 as id_181137192399,esg.id_181137272399 as id_181137272399,esg.id_181138112899 as id_181138112899,esg.id_181138132899 as id_181138132899,esg.id_181138152899 as id_181138152899,esg.id_191111132799 as id_191111132799,esg.id_191211112399 as id_191211112399,esg.id_191211152799 as id_191211152799,esg.id_191214112899 as id_191214112899,esg.max_effective_date as max_effective_date,esg.market_value as market_value,esg.tenant_id as tenant_id,esg.di_load_date as last_loaded_date from exposure_esg_grouped_pivot esg , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg.tenant_id=201 and esg.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and alias_portfolio_by_group.tenant_id=esg.tenant_id and alias_portfolio_by_group.portfolio_code=esg.portfolio_code and ( esg.as_of_date = timestamp '2022-04-30' and esg.id_181110142899 = 'Unclassified')
```

time 1.73s  time 463



40 query ESG_Investment_L3 {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    ESG(filters: [{andOr: OR, field: "as_of_date", value: "$FROMORTODATE", operator: EQUAL}, {andOr: AND, field: "portfolio_code", value: "ds_average", operator: EQUAL}]) {      investmentId      investmentName      marketValue      portfolioCode      portfolioName      properties {        entities        code        type        name        value        valueCode      }    }  }}

```
select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata
```

```sql
select esg.investment_id as investment_id,esg.investment_name as investment_name,esg.as_of_date as as_of_date,esg.portfolio_code as portfolio_code,esg.portfolio_name as portfolio_name,esg.id_181110112399 as id_181110112399,esg.id_181110142899 as id_181110142899,esg.id_181110182899 as id_181110182899,esg.id_181110202899 as id_181110202899,esg.id_181137112399 as id_181137112399,esg.id_181137192399 as id_181137192399,esg.id_181137272399 as id_181137272399,esg.id_181138112899 as id_181138112899,esg.id_181138132899 as id_181138132899,esg.id_181138152899 as id_181138152899,esg.id_191111132799 as id_191111132799,esg.id_191211112399 as id_191211112399,esg.id_191211152799 as id_191211152799,esg.id_191214112899 as id_191214112899,pass_threshold_carbon_risk_score as pass_threshold_carbon_risk_score,pass_threshold_esg_risk_score as pass_threshold_esg_risk_score,weight_carbon_risk_score as weight_carbon_risk_score,weight_esg_risk_score as weight_esg_risk_score,esg.max_effective_date as max_effective_date,esg.market_value as market_value,esg.tenant_id as tenant_id,esg.di_load_date as last_loaded_date from exposure_esg_pivot esg , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-11 00:22:53.040 UTC') alias_portfolio_by_group where esg.tenant_id=201 and esg.di_load_date=timestamp '2023-05-11 00:23:44.600 UTC' and alias_portfolio_by_group.tenant_id=esg.tenant_id and alias_portfolio_by_group.portfolio_code=esg.portfolio_code and ( esg.as_of_date = timestamp '2022-04-30' and esg.portfolio_code = 'ds_average')
```

time 1.45s  time 601

