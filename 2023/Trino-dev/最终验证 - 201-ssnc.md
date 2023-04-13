http://10.2.96.132:32106/graphql         trino

http://10.2.96.132:30103/                 graphql

|           | node number | Node cup | node memory | Read timeout | Write timeout |
| --------- | ----------- | -------- | ----------- | ------------ | ------------- |
| Cassandra | 5           | 8        | 20G         | 5s           | 20s           |

|       | worker | worker cup | worker memory |
| ----- | ------ | ---------- | ------------- |
| Trino | 4      | 2          | 4G            |

|          | CUP  | Memory | Timeout |
| -------- | ---- | ------ | ------- |
| Graphql2 | 2    | 8G     | 8m      |

| feature_aoc_28312_csp | activity | exposure_test | performance_analysis | portfolio | portfolio_by_group | portfolio_property_by_tenant | portfolio_property_by_portfolio |
| --------------------- | -------- | ------------- | -------------------- | --------- | ------------------ | ---------------------------- | ------------------------------- |
| Items                 | 420      | 11969         | 60                   | 12610     | 14468              | 9                            | 122649                          |



| feature_aoc_28312_csp | ds_nestedgrp | PeformanceTestGroup |
| --------------------- | ------------ | ------------------- |
| Items                 | 9            | 10000               |

query MyQuery {
  insights(portfolioGroupCode: "ds_nestedgrp") {
    summaryActivity(dateRange: MTD) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      contributions
      dividentsInterest
      endingMarketValue
      expenses
      fees
      feesAndExpenses
      gainAndLoss
      netFlows
      realizedGL
      unrealizedGL
      withdrawals
    }
  }
}

| Graphql   | Portfolio code   | Items | Data size | Db       | browser  |
| --------- | ---------------- | ----- | --------- | -------- | -------- |
|           | ds_nestedgrp     | 1     | 550B      | 682.62ms | 1.57s    |
|           |                  |       |           | 632.77ms | 637.73ms |
| **Trino** | **ds_nestedgrp** |       |           | 656.99ms | 1.19s    |
|           |                  |       |           | 750.53ms | 750.20ms |

query MyQuery {
  insights(portfolioGroupCode: "ds_nestedgrp") {
    activities(dateRange: MTD, grouping: {aggregate: {value: sum}, groupby: "CUSTODIAN"}) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      endingMarketValue
      properties {
        code
        name
        value
        valueCode
      }
    }
  }
}

| Graphql   | Portfolio code   | Items | Data size | Db       | browser  |
| --------- | ---------------- | ----- | --------- | -------- | -------- |
|           | ds_nestedgrp     | 1     | 550B      | 649.09ms | 1.32s    |
|           |                  |       |           | 527.99ms | 530.99ms |
| **Trino** | **ds_nestedgrp** |       |           | 910.73ms | 912.71ms |
|           |                  |       |           | 866.99ms | 868.84ms |

=

query MyQuery {
  insights(portfolioGroupCode: "ds_nestedgrp") {
    activities(dateRange: MTD, filters: {andOr: AND, field: "CUSTODIAN", value: "Unclassified", operator: EQUAL}) {
      beginningMarketValue
      cashMarketValue
      changeMarketValuePercent
      endingMarketValue
      properties {
        code
        name
        value
        valueCode
      }
    }
  }
}



| Graphql   | Portfolio code   | Items | Data size | Db       | browser  |
| --------- | ---------------- | ----- | --------- | -------- | -------- |
|           | ds_nestedgrp     | 1     | 550B      | 607.43ms | 609.79ms |
|           |                  |       |           | 535.03ms | 537.03ms |
| **Trino** | **ds_nestedgrp** |       |           | 873.96ms | 1.59s    |
|           |                  |       |           | 955.85ms | 957.83ms |