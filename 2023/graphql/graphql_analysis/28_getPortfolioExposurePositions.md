28 getPortfolioExposurePositions

query getPortfolioExposurePositions {  asOfDates(entity: EXPOSURE) {    periodDates {      lastDay {        fromDate        toDate      }    }  }  portfolios(code: "$PORTFOLIO") {    positions(dateRange: LastDay, filters: [], grouping: {groupby: "ASSET_CLASS", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["ASSET_CLASS", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}

25个sql

发现

（1）0stage 两个project 空的

(2)investment_property_by_investment 符合partition key但是慢





5个stage

2 exposure_test（2个worker 17.06ms） ,  4 investment_property_by_investment（1个worker 1.04s）

3 investment_property_by_investment（4个worker， 1.07s，aggregate，  project()， aggregate， localexchange）

1 = 2+3（4个worker， 1.11s， aggregate，  project(),  project()，left join, aggregate, localexchange/ aggregate）

0 output（4个worker 1.08s，output， project(),  project(), aggregate, localexchange）

```sql
13个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	34.80ms
Queued Time	781.75us
Analysis Time	1.23ms
Planning Time	13.30ms
Execution Time	33.01ms
```

```sql
3个 select investment_property_name as name,investment_property_value_type as type,di_load_date as last_loaded_date from investment_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:18:50.260 UTC'

Elapsed Time	36.16ms
Queued Time	160.05us
Analysis Time	1.09ms
Planning Time	20.26ms
Execution Time	34.73ms
```

```sql
select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC

Elapsed Time	66.75ms
Queued Time	216.03us
Analysis Time	1.41ms
Planning Time	23.27ms
Execution Time	65.02ms
```

```sql
3个 select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata 

Elapsed Time	461.43ms
Queued Time	244.59us
Analysis Time	832.59us
Planning Time	5.27ms
Execution Time	460.27ms
```

```sql
3个 select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'

Elapsed Time	29.30ms
Queued Time	201.39us
Analysis Time	1.03ms
Planning Time	15.47ms
Execution Time	27.96ms
```

```sql
select * from (select A.as_of_date as as_of_date ,sum(A.total_market) as market_value from exposure_test A  where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2022-04-29 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and A.portfolio_code  in ('auto_pa_esg_cbus') group by A.as_of_date) A ORDER BY as_of_date DESC

Elapsed Time	74.03ms
Queued Time	190.73us
Analysis Time	1.94ms
Planning Time	34.54ms
Execution Time	71.43ms
```

```sql
select pq.as_of_date as as_of_date,pq.market_value as market_value,CASE when pq.as_of_date=timestamp '2022-04-30 00:00:00.000 UTC' then pq.market_value/-7304450.227361111  when pq.as_of_date=timestamp '2022-04-29 00:00:00.000 UTC' then pq.market_value/-7304430.328888889  END as weight,pq.ASSET_CLASS_CODE as ASSET_CLASS_CODE,pq.ASSET_CLASS as ASSET_CLASS from (select A.as_of_date as as_of_date ,SUM(A.market_value) as market_value,A.ASSET_CLASS_CODE as ASSET_CLASS_CODE,A.ASSET_CLASS as ASSET_CLASS from (select (CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG_CODE when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT_CODE END) as ASSET_CLASS_CODE ,(CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT END) as ASSET_CLASS,A.as_of_date as as_of_date,A.investment_id as investment_id,A.is_short_asset as is_short_asset,A.total_market as market_value,A.portfolio_code as portfolio_code,A.portfolio_name as portfolio_name,A.price as price,A.quantity as quantity,A.weight as weight,A.tenant_id as tenant_id,A.di_load_date as last_loaded_date from exposure_test A left join (select investment_id as investment_id,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value END) as ANALYST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value END) as AUTO_PA_CSP_LIST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value END) as AUTO_PA_CSP_LIST_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA_1,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value_code END) as CURRENCY_CODE,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value END) as CURRENCY,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value_code END) as INDUSTRY_GROUP_CODE,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value END) as INDUSTRY_GROUP,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value_code END) as INVESTMENT_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value END) as INVESTMENT,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value_code END) as INVESTMENT_TYPE_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value END) as INVESTMENT_TYPE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value_code END) as ISSUER_CODE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value END) as ISSUER,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value_code END) as MARKET_CODE,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value END) as MARKET,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value_code END) as MS_STYLE_CODE,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value END) as MS_STYLE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value_code END) as SECTOR_CODE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value END) as SECTOR,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value_code END) as SEGMENT_CODE,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value END) as SEGMENT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value_code END) as ASSET_TYPE_SHORT_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value END) as ASSET_TYPE_SHORT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value_code END) as ASSET_TYPE_LONG_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value END) as ASSET_TYPE_LONG from investment_property_by_investment  where tenant_id=201 group by investment_id) alias_pivot_investment_property on alias_pivot_investment_property.investment_id = A.investment_id where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2022-04-29 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and A.portfolio_code  in ('auto_pa_esg_cbus') ORDER BY A.as_of_date DESC) A group by A.as_of_date,A.tenant_id,A.ASSET_CLASS_CODE,A.ASSET_CLASS) pq  OFFSET 0

Elapsed Time	1.26s
Queued Time	216.59us
Analysis Time	8.52ms
Planning Time	119.85ms
Execution Time	1.25s
```