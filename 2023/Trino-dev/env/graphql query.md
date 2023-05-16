Trino

201 - https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/
		600 - https://graphql-trino-refactor-graphql.ssnc-corp.cloud/





Redis: 10.43.1.14



graphql

201 - https://feature-aoc-28970-graphql.ssnc-corp.cloud/

600 - https://feature-aoc-31567-graphql.ssnc-corp.cloud/



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
      }
    }
  }
}



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
      }
    }
  }
}