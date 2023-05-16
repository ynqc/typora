5  getExposureTopBottomChanges

query getExposureTopBottomChanges {  insights(portfolioGroupCode: "$PORTFOLIO_GROUP") {    topBottomHoldings(dateRange: YTD, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "ASSET_CLASS"}, limit: 3) {      top {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }      bottom {        marketValueChange        weightChange        aumPercent        aumChangePercent        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }    }  }}

Sql 26个

发现
（1）26个请求，减少重复请求使用cache
（2） 减少不必要请求
（3）优化query 太长了（pivot 有可能以后需要db提供好pivot 的table）

```
14个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201
Elapsed Time	25.15ms
Queued Time	173.53us
Analysis Time	1.87ms
Planning Time	11.66ms
Execution Time	22.98ms

2个 select * from (select A.as_of_date as as_of_date ,sum(A.total_market) as market_value from exposure_test A , (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2021-12-31 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code group by A.as_of_date) A ORDER BY as_of_date DESC
Elapsed Time	446.74ms
Queued Time	324.35us
Analysis Time	10.05ms
Planning Time	220.12ms
Execution Time	436.39ms

3个 select investment_property_name as name,investment_property_value_type as type,di_load_date as last_loaded_date from investment_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:18:50.260 UTC'
Elapsed Time	45.75ms
Queued Time	157.31us
Analysis Time	942.01us
Planning Time	17.68ms
Execution Time	44.56ms

3个 select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'
Elapsed Time	45.55ms
Queued Time	218.45us
Analysis Time	952.19us
Planning Time	22.08ms
Execution Time	44.31ms

3个 select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata  
Elapsed Time	321.90ms
Queued Time	255.04us
Analysis Time	885.50us
Planning Time	4.12ms
Execution Time	320.58ms

1个 select pq.as_of_date as as_of_date,pq.market_value as market_value,CASE when pq.as_of_date=timestamp '2022-04-30 00:00:00.000 UTC' then pq.market_value/98912920.98250338  when pq.as_of_date=timestamp '2021-12-31 00:00:00.000 UTC' then pq.market_value/86408207.63047665  END as weight,pq.ASSET_CLASS_CODE as ASSET_CLASS_CODE,pq.ASSET_CLASS as ASSET_CLASS from (select A.as_of_date as as_of_date ,SUM(A.market_value) as market_value,A.ASSET_CLASS_CODE as ASSET_CLASS_CODE,A.ASSET_CLASS as ASSET_CLASS from (select (CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG_CODE when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT_CODE END) as ASSET_CLASS_CODE ,(CASE when A.is_short_asset=True then alias_pivot_investment_property.ASSET_TYPE_LONG when A.is_short_asset=False then alias_pivot_investment_property.ASSET_TYPE_SHORT END) as ASSET_CLASS,A.as_of_date as as_of_date,A.investment_id as investment_id,A.is_short_asset as is_short_asset,A.total_market as market_value,A.portfolio_code as portfolio_code,A.portfolio_name as portfolio_name,A.price as price,A.quantity as quantity,A.weight as weight,A.tenant_id as tenant_id,A.di_load_date as last_loaded_date from exposure_test A left join (select investment_id as investment_id,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value_code END) as ANALYST_CODE,MAX(CASE WHEN investment_property_name='ANALYST' THEN investment_property_value END) as ANALYST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST' THEN investment_property_value END) as AUTO_PA_CSP_LIST,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LISTCODES_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LISTCODES_1' THEN investment_property_value END) as AUTO_PA_CSP_LISTCODES_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value_code END) as AUTO_PA_CSP_LIST_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_LIST_1' THEN investment_property_value END) as AUTO_PA_CSP_LIST_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_BASE_IG_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_BASE_IG_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_BASE_IG_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value_code END) as AUTO_PA_CSP_MAPPING_CUSTOM_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_MAPPING_CUSTOM_1' THEN investment_property_value END) as AUTO_PA_CSP_MAPPING_CUSTOM_1,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value_code END) as AUTO_PA_CSP_RANGE_ALPHA_1_CODE,MAX(CASE WHEN investment_property_name='AUTO_PA_CSP_RANGE_ALPHA_1' THEN investment_property_value END) as AUTO_PA_CSP_RANGE_ALPHA_1,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value_code END) as CURRENCY_CODE,MAX(CASE WHEN investment_property_name='CURRENCY' THEN investment_property_value END) as CURRENCY,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value_code END) as INDUSTRY_GROUP_CODE,MAX(CASE WHEN investment_property_name='INDUSTRY_GROUP' THEN investment_property_value END) as INDUSTRY_GROUP,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value_code END) as INVESTMENT_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT' THEN investment_property_value END) as INVESTMENT,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value_code END) as INVESTMENT_TYPE_CODE,MAX(CASE WHEN investment_property_name='INVESTMENT_TYPE' THEN investment_property_value END) as INVESTMENT_TYPE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value_code END) as ISSUER_CODE,MAX(CASE WHEN investment_property_name='ISSUER' THEN investment_property_value END) as ISSUER,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value_code END) as MARKET_CODE,MAX(CASE WHEN investment_property_name='MARKET' THEN investment_property_value END) as MARKET,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value_code END) as MS_STYLE_CODE,MAX(CASE WHEN investment_property_name='MS_STYLE' THEN investment_property_value END) as MS_STYLE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value_code END) as SECTOR_CODE,MAX(CASE WHEN investment_property_name='SECTOR' THEN investment_property_value END) as SECTOR,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value_code END) as SEGMENT_CODE,MAX(CASE WHEN investment_property_name='SEGMENT' THEN investment_property_value END) as SEGMENT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value_code END) as ASSET_TYPE_SHORT_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_SHORT' THEN investment_property_value END) as ASSET_TYPE_SHORT,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value_code END) as ASSET_TYPE_LONG_CODE,MAX(CASE WHEN investment_property_name='ASSET_TYPE_LONG' THEN investment_property_value END) as ASSET_TYPE_LONG from investment_property_by_investment  where tenant_id=201 group by investment_id) alias_pivot_investment_property on alias_pivot_investment_property.investment_id = A.investment_id, (select alias_portfolio_by_group.portfolio_code as portfolio_code,alias_portfolio_by_group.portfolio_group_code as portfolio_group_code,alias_portfolio_by_group.tenant_id as tenant_id from portfolio_by_group alias_portfolio_by_group  where alias_portfolio_by_group.portfolio_group_code='ds_nestedgrp' and alias_portfolio_by_group.tenant_id=201 and alias_portfolio_by_group.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC') alias_portfolio_by_group where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2021-12-31 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and alias_portfolio_by_group.tenant_id=A.tenant_id and alias_portfolio_by_group.portfolio_code=A.portfolio_code ORDER BY A.as_of_date DESC) A group by A.as_of_date,A.tenant_id,A.ASSET_CLASS_CODE,A.ASSET_CLASS) pq ORDER BY market_value desc,ASSET_CLASS desc  OFFSET 0
Elapsed Time	2.10s
Queued Time	213.68us
Analysis Time	24.74ms
Planning Time	188.09ms
Execution Time	2.07s

```

