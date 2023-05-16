



**1 Dashboard - First query**  

(1)  query getAllETLAsOfDate {  portfolio: asOfDates(entity: PORTFOLIO) {    date    etlStartTime  }  portfolio_group: asOfDates(entity: PORTFOLIO_GROUP) {    date    etlStartTime  }  exposure: asOfDates(entity: EXPOSURE) {    date    etlStartTime    periodDates {      ...lastSevenDays      ...lastFourQuarters      mtd {        fromDate      }      qtd {        fromDate      }      ytd {        fromDate      }    }    date  }  performance: asOfDates(entity: PERFORMANCE) {    date    etlStartTime  }  activity: asOfDates(entity: ACTIVITY) {    date    etlStartTime    periodDates {      mtd {        fromDate        toDate      }      qtd {        fromDate        toDate      }      ytd {        fromDate        toDate      }    }  }} fragment lastSevenDays on PeriodDates {  lastSevenDays: lastSevenDays {    dates(timeInterval: DAILY)    fromDate    toDate  }} fragment lastFourQuarters on PeriodDates {  lastFourQuarters: oneYear {    dates(timeInterval: QUARTERLY)    fromDate    toDate  }}

(2) query getExposureComparisonHeaderInfo {  portfolioGroups(code: \*"ds_nestedgrp\") {    code    name    lastLoadedDate    portfolios {      code    }  }}

(3) query getTopBottomPerformersQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: MTD, limit: 5) {              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(4) query ActivitySummary {     insights(portfolioGroupCode: "ds_nestedgrp") {         ...summaryActivityFragment     } } fragment summaryActivityFragment on Reports {     summaryActivity(dateRange: YTD) {     beginningMarketValue     cashMarketValue     contributions     dividentsInterest     endingMarketValue     expenses     fees     feesAndExpenses     gainAndLoss     netFlows     realizedGL     unrealizedGL     withdrawals     changeMarketValuePercent } }

(5) query getPortfolioGroups {   portfolioGroups {       code       name   }   }

(6) query getPropertyMetadata {  propertyMetadata {    code    name    type    entities  }}

(7) query getPerformanceAnalysisCardQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: YTD, limit: 5) {              lastLoadedDate              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(8) query getAllActiveReturnDeviations($portfolioGroupCode: String!) {        insights(portfolioGroupCode: $portfolioGroupCode) {        mtd: activeReturnDeviations(period: MTD) {                    ...ActiveChartSchema                },qtd: activeReturnDeviations(period: QTD) {                    ...ActiveChartSchema                },ytd: activeReturnDeviations(period: YTD) {                    ...ActiveChartSchema                },sinceInception: activeReturnDeviations(period: ITD) {                    ...ActiveChartSchema                },oneYear: activeReturnDeviations(period: OneYear) {                    ...ActiveChartSchema                },threeYears: activeReturnDeviations(period: ThreeYears) {                    ...ActiveChartSchema                },fiveYears: activeReturnDeviations(period: FiveYears) {                    ...ActiveChartSchema                }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }    

(9) query getExposureComparisonCardQuery ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"ds_nestedgrp","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":4,"dateRange":"YTD","aggregations":[{"method":"sum","field":"market_value"}]}



**2 Dashboard - Second query** 

(1) query getExposureComparisonHeaderInfo {        portfolioGroups(code: "ds_nestedgrp") {            lastLoadedDate            portfolios {                code            }        }    }

(2) query getPerformanceAnalysisCardQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: YTD, limit: 5) {                lastLoadedDate                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(3) query getExposureComparisonCardQuery ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"ds_nestedgrp","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":4,"dateRange":"YTD","aggregations":[{"method":"sum","field":"market_value"}]}

(4) query getActiveReturnDeviations($portfolioGroupCode: String!, $period: PerformanceDateRangeInput!) {        insights(portfolioGroupCode: $portfolioGroupCode) {          activeReturnDeviations(period: $period) {            ...ActiveChartSchema          }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }
"variables":{"portfolioGroupCode":"ds_nestedgrp","period":"YTD"}

(5) query ActivitySummary {        insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }



**3 Exposure Comparison**  

(1)query getExposureComparisonHeaderInfo {     portfolioGroups(code: "ds_nestedgrp") {         lastLoadedDate         portfolios {             code         }     } }

(2) query getGroupByOptions {      groupByOptions {        l1        l2        l3        l4        name      }    }

(3) query getExposureTopBottomChanges ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {   insights(portfolioGroupCode: $code) {     topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {       top {         marketValueChange         weightChange         aumPercent         aumChangePercent         properties(codes: $propertyCodes) {           code           name           value           type         }       }       bottom {         marketValueChange         weightChange         aumPercent         aumChangePercent         properties(codes: $propertyCodes) {           code           name           value           type         }       }     }   } }
"variables":{"code":"ds_nestedgrp","dateRange":"YTD","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":3,"aggregations":[{"method":"sum","field":"market_value"}]}

(4) query getExposureComparisonPositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) {   insights(portfolioGroupCode: $code) {       positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {         asOfDate         marketValue         portfolioCode         portfolioName         price         quantity         weight         properties(codes: $propertyCodes) {           code           name           type           value           valueCode         }       }     }   }
"variables":{"code":"ds_nestedgrp","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}

**4 Performance Analysis**

(1) query getExposureComparisonHeaderInfo {       portfolioGroups(code: "ds_nestedgrp") {           lastLoadedDate           portfolios {               code           }       }   }

(2) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: MTD, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(3) query getPerformanceAnalysisGridQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            performance {                marketValue                portfolioName                portfolioCode                properties(codes: ["Benchmark", "ANALYST"]) {                    code                    value                    type                }                mtd {        difference        index        portfolio    }qtd {        difference        index        portfolio    }ytd {        difference        index        portfolio    }sinceInception {        difference        index        portfolio    }oneYear {        difference        index        portfolio    }threeYears {        difference        index        portfolio    }fiveYears {        difference        index        portfolio    }            }        }    }

(4) query getAllActiveReturnDeviations($portfolioGroupCode: String!) {        insights(portfolioGroupCode: $portfolioGroupCode) {        mtd: activeReturnDeviations(period: MTD) {                    ...ActiveChartSchema                },qtd: activeReturnDeviations(period: QTD) {                    ...ActiveChartSchema                },ytd: activeReturnDeviations(period: YTD) {                    ...ActiveChartSchema                },sinceInception: activeReturnDeviations(period: ITD) {                    ...ActiveChartSchema                },oneYear: activeReturnDeviations(period: OneYear) {                    ...ActiveChartSchema                },threeYears: activeReturnDeviations(period: ThreeYears) {                    ...ActiveChartSchema                },fiveYears: activeReturnDeviations(period: FiveYears) {                    ...ActiveChartSchema                }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }
"variables":{"portfolioGroupCode":"ds_nestedgrp"}

 **5 Activity**  

(1) query getExposureComparisonHeaderInfo {        portfolioGroups(code: "ds_nestedgrp") {            lastLoadedDate            portfolios {                code            }        }    }

(2) query ActivitySummary {        insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(3) query getActivity {      insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment            activities(dateRange: YTD, grouping: { aggregate: { value: sum }, groupby: "ANALYST"}) {beginningMarketValue endingMarketValue changeMarketValuePercent cashMarketValue                feesAndExpenses {value  fees {value percent} expenses {value}}                gainAndLoss {percent value dividentsInterest {value} realizedGL {value} unrealizedGL {value}}netFlows {value percent contributions {value} withdrawals {value}}                properties(codes: ["ANALYST"]) {                    code                    value                    type                }            }           }    }        fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

 **6 Household Details** 

(1) query getGroupByOptions {      groupByOptions {        l1        l2        l3        l4        name      }    }

(2) query getHouseholdHeaderData($code: String!) { portfolioGroups(code: $code) {      portfolios {        code      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp"}

(3) query ActivitySummary {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(4) query getHouseholdTopBottomHoldings ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD","groupBy":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}],"limit":8,"propertyCodes":"ANALYST"}

(5) query getExposureComparisonPositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) { insights(portfolioGroupCode: $code) {        positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {          asOfDate          marketValue          portfolioCode          portfolioName          price          quantity          weight          properties(codes: $propertyCodes) {            code            name            type            value            valueCode          }        }      }    }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}

(6) query getPerformanceAnalysisGridQuery {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            performance {                marketValue                portfolioName                portfolioCode                properties(codes: ["Benchmark", "ANALYST"]) {                    code                    value                    type                }                mtd {        difference        index        portfolio    }qtd {        difference        index        portfolio    }ytd {        difference        index        portfolio    }sinceInception {        difference        index        portfolio    }oneYear {        difference        index        portfolio    }threeYears {        difference        index        portfolio    }fiveYears {        difference        index        portfolio    }            }        }    }

(7) query getActivity {     insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {             activities(dateRange: YTD, grouping: { aggregate: { value: sum }, groupby: "ANALYST"}) {beginningMarketValue endingMarketValue changeMarketValuePercent cashMarketValue                feesAndExpenses {value  fees {value percent} expenses {value}}                gainAndLoss {percent value dividentsInterest {value} realizedGL {value} unrealizedGL {value}}netFlows {value percent contributions {value} withdrawals {value}}                properties(codes: ["ANALYST"]) {                    code                    value                    type                }            }           }    } 

(8) query getHouseholdPerformanceTileData($code: String) {    portfolioGroups(code: $code) {      portfolios {        name        code        performance {          ytd {            difference          }          mtd {            difference          }          qtd {            difference          }        }  ...summaryActivityFragment       }    }  }

fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

"variables":{"code":"auto_BrokerDupNameGrp"}



**7 Portfolio Details**  

(1) query getPortfolioDetailInfo {  portfolios(code: "ds_multicash") {    name    openDate    closingMethod    status    taxStatus    properties(codes: "INVESTMENT_GOAL") {      code      value      type    }  }}

(2) query getGroupByOptions {        groupByOptions {          l1          l2          l3          l4          name        }      }

(3) query GetTMV {        portfolios(code: "ds_multicash") {          summaryActivity(dateRange: MTD) {            endingMarketValue          }        }      }

(4) query GetPerformanceGraph {        portfolios(code: "ds_multicash") {          performance {            mtd {              portfolio              index              difference            }            qtd {              portfolio              index              difference            }            ytd {              portfolio              index              difference            }            oneYear {              portfolio              index              difference            }            threeYears {              portfolio              index              difference            }            fiveYears {              portfolio              index              difference            }            sinceInception {              portfolio              index              difference            }          }        }      }

(5) query getPortfolioExposurePositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) {  portfolios(code: $code) {        positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {          asOfDate          marketValue          portfolioCode          portfolioName          price          quantity          weight          properties(codes: $propertyCodes) {            code            name            type            value            valueCode          }        }      }    }
"variables":{"code":"ds_multicash","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}

(6) query getExposureHeader($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {        portfolios(code: $code) {          topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {                bottom {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }                top {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }              }        }    }
"variables":{"code":"ds_multicash","dateRange":"YTD","groupBy":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}],"limit":8,"propertyCodes":"ANALYST"}

(7)query ActivitySummaryPortDetails {        portfolios(code: "ds_multicash") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Portfolio {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

