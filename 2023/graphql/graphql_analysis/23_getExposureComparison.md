23 getExposureComparison

query getExposureComparison {  portfolios(code: "auto_pa_esg_cbus") {    topBottomHoldings(dateRange: YTD, grouping: {aggregations: [{method: sum, field: "market_value"}], groupby: "ASSET_CLASS"}, limit: 8) {      top {        aumChangePercent        aumPercent        marketValueChange        properties(codes: "ASSET_CLASS") {          code          name          value          type        }      }    }  }}

28个sql

发现

（1）investment_esg_metadata  esg_code partition key



```sql
15个 select  as_of_date as as_of_date, etl_start_time as etl_start_time, tenant_id as tenant_id, ui_feature as entity, di_load_date as last_loaded_date  from etl_audit where  tenant_id=201

Elapsed Time	35.46ms
Queued Time	699.28us
Analysis Time	1.12ms
Planning Time	12.04ms
Execution Time	33.77ms
```

```sql
2个 select pt.portfolio_code as code,pt.portfolio_name as name,pt.portfolio_status as status,pt.close_method as closing_method,pt.tax_status as tax_status,pt.start_date as open_date,pt.di_load_date as last_loaded_date from portfolio_by_code pt  where pt.tenant_id=201 and pt.di_load_date=timestamp '2023-05-14 14:21:29.913 UTC' and pt.portfolio_code='auto_pa_esg_cbus' ORDER BY pt.portfolio_name ASC

Elapsed Time	64.83ms
Queued Time	399.44us
Analysis Time	1.43ms
Planning Time	25.07ms
Execution Time	63.04ms
```

```sql
2个 select * from (select A.as_of_date as as_of_date ,sum(A.total_market) as market_value from exposure_test A  where A.tenant_id=201 and A.di_load_date=timestamp '2023-05-14 14:26:52.546 UTC' and A.as_of_date  in ( timestamp '2021-12-31 00:00:00.000 ', timestamp '2022-04-30 00:00:00.000 ') and A.portfolio_code  in ('auto_pa_esg_cbus') group by A.as_of_date) A ORDER BY as_of_date DESC

Elapsed Time	75.14ms
Queued Time	173.38us
Analysis Time	1.92ms
Planning Time	37.99ms
Execution Time	72.94ms
```

```sql
3个 select investment_property_name as name,investment_property_value_type as type,di_load_date as last_loaded_date from investment_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:18:50.260 UTC'

Elapsed Time	39.72ms
Queued Time	253.64us
Analysis Time	1.02ms
Planning Time	18.26ms
Execution Time	38.40ms
```

```sql
3个 select portfolio_property_name as name,di_load_date as last_loaded_date from portfolio_property_by_tenant   where tenant_id=201 and di_load_date=timestamp '2023-05-14 14:33:46.532 UTC'

Elapsed Time	33.39ms
Queued Time	145.16us
Analysis Time	830.38us
Planning Time	18.33ms
Execution Time	32.24ms
```

```sql
3个 select esg_name as name,esg_code as code,esg_data_type as type from investment_esg_metadata  

Elapsed Time	238.82ms
Queued Time	165.54us
Analysis Time	785.98us
Planning Time	3.65ms
Execution Time	237.77ms
```

