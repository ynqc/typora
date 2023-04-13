



Filter and or 加了括号 ()

Groupby 后filter

| JSON key | Value                                                        | Type        |
| -------- | ------------------------------------------------------------ | ----------- |
| Main     | “Activity”                                                   | String      |
| Froms    | [@(pivot_portfolio_property_by_portfolio), “activity” ]      | Array       |
|          | @(“str”)                                                     |             |
|          | “str”                                                        |             |
| Columns  | {'sum(A.beginning_mv)': 'beginning_mv', 'sum(A.ending_mv)': 'ending_mv'} | String/Dict |
|          | *                                                            |             |
|          | “column”: “alisa column”                                     |             |
| Filters  | {"A.tenant_id": 201,  @key1(‘as_od_date’): ‘2020-12-31’, @key1(‘as_od_date’): ‘2020-10-31’,} | Dict        |
|          | “key”:  “value”                                              |             |
|          | “key”@operator(>): “key”:  “value”                           |             |
|          | “key”@and_or(and): “key”:  “value”                           |             |
|          | @key{index}(“key”): “key”:  “value”                          |             |
| groups   | [“as_of_date”, “portfolio_code”]                             | Array       |
|          | “str”                                                        |             |
| sorts    | {“as_of_date”: “asc”, “portfolio_code”: desc}                | Dict        |
|          | “key”:  desc/asc                                             |             |
| paging   | {“limit”: 10, “offset”: 0}                                   | Dict        |
|          | limit                                                        | Number      |
|          | Offset                                                       | Number      |
| joins    | [{“table”: “portfolio_by_group”, “type”: “inner/left/rigth”, “on”: {"D.tenant_id": "A.tenant_id"}}] | Array       |
|          | table                                                        | String      |
|          | Type                                                         | String      |
|          | On                                                           | Dict        |
| unions   | Json                                                         | Json        |

