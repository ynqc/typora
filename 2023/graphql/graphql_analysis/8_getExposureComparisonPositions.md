8 getExposureComparisonPositions

query getExposureComparisonPositions {  asOfDates(entity: EXPOSURE) {    periodDates {      ytd {        fromDate        toDate      }    }  }  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    positions(dateRange: YTD, filters: [], grouping: {groupby: "ASSET_CLASS", aggregations: [{method: sum, field: "market_value"}]}) {      asOfDate      marketValue      portfolioCode      portfolioName      price      quantity      weight      properties(codes: ["ASSET_CLASS", "INVESTMENT"]) {        code        name        type        value        valueCode      }    }  }}

18个sql

发现

(1)pivot 占用时间了，优化

(2) portfolio_by_group  tenant_id, portfolio_code partitionkey

(3) investment_property_by_investment慢

(4) inner join 慢



```
10个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	42.52ms
Queued Time	484.70us
Analysis Time	1.99ms
Planning Time	14.99ms
Execution Time	40.17ms
```

```
2个 select investment_property_name as name,investment_property_value_type as type,di_load_date as last_loaded_date from investment_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:18:50.260 UTC'

Elapsed Time	58.25ms
Queued Time	401.34us
Analysis Time	1.13ms
Planning Time	18.33ms
Execution Time	56.81ms
```

```sql
2个 select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'

Elapsed Time	35.07ms
Queued Time	175.33us
Analysis Time	1.55ms
Planning Time	15.90ms
Execution Time	33.23ms
```

```sql
2个 select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata  

Elapsed Time	222.90ms
Queued Time	174.43us
Analysis Time	984.34us
Planning Time	4.29ms
Execution Time	221.59ms
```

```sql
select * from (select A.as_of_date as as_of_date ,sum(A.total_market) as market_value from exposure_test A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='$PORTFOLIO_GROUP' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2021-12-31 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by A.as_of_date) A ORDER BY as_of_date DESC

Elapsed Time	152.93ms
Queued Time	187.08us
Analysis Time	3.15ms
Planning Time	75.95ms
Execution Time	149.44ms
```

```sql
select pq.as_of_date as as_of_date,pq.market_value as market_value,pq.ASSET_CLASS_CODE as ASSET_CLASS_CODE,pq.ASSET_CLASS as ASSET_CLASS from (select A.as_of_date as as_of_date ,SUM(A.market_value) as market_value,A.ASSET_CLASS_CODE as ASSET_CLASS_CODE,A.ASSET_CLASS as ASSET_CLASS from (select (CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG_CODE when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT_CODE END) as ASSET_CLASS_CODE ,(CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT END) as ASSET_CLASS,A.as_of_date as as_of_date,A.investment_id as investment_id,A.is_short_asset as is_short_asset,A.total_market as market_value,A.portfolio_code as portfolio_code,A.portfolio_name as portfolio_name,A.price as price,A.quantity as quantity,A.weight as weight,A.tenant_id as tenant_id,A.di_load_date as last_loaded_date from exposure_test A left join (select investment_id as investment_id,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value END) as ANALYST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value END) as AUTO_PA_CSP_LIST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value END) as AUTO_PA_CSP_LIST_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA_1,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value_code END) as CURRENCY_CODE,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value END) as CURRENCY,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value_code END) as INDUSTRY_GROUP_CODE,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value END) as INDUSTRY_GROUP,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value_code END) as INVESTMENT_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value END) as INVESTMENT,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value_code END) as INVESTMENT_TYPE_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value END) as INVESTMENT_TYPE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value_code END) as ISSUER_CODE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value END) as ISSUER,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value_code END) as MARKET_CODE,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value END) as MARKET,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value_code END) as MS_STYLE_CODE,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value END) as MS_STYLE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value_code END) as SECTOR_CODE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value END) as SECTOR,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value_code END) as SEGMENT_CODE,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value END) as SEGMENT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value_code END) as ASSET_TYPE_SHORT_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value END) as ASSET_TYPE_SHORT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value_code END) as ASSET_TYPE_LONG_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value END) as ASSET_TYPE_LONG from investment_property_by_investment  where tenant_id=201 group by investment_id) alias_pivot_investment_property on alias_pivot_investment_property.investment_id = A.investment_id, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='$PORTFOLIO_GROUP' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2021-12-31 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code ORDER BY A.as_of_date DESC) A group by A.as_of_date,A.tenant_id,A.ASSET_CLASS_CODE,A.ASSET_CLASS) pq  OFFSET 0

Elapsed Time	1.48s
Queued Time	260.65us
Analysis Time	31.11ms
Planning Time	157.86ms
Execution Time	1.45s
```