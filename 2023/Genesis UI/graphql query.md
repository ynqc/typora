1 Dashboard

(1) query getAllETLAsOfDate {  portfolio: asOfDates(entity: PORTFOLIO) {          date          etlStartTime      },portfolio_group: asOfDates(entity: PORTFOLIO_GROUP) {          date          etlStartTime      },exposure: asOfDates(entity: EXPOSURE) {          date          etlStartTime      },performance: asOfDates(entity: PERFORMANCE) {          date          etlStartTime      },activity: asOfDates(entity: ACTIVITY) {          date          etlStartTime      } }

(2) query getExposureComparisonMultidatePresetDatesQuery {  asOfDates(entity: EXPOSURE) {    periodDates {      ...lastSevenDays      ...lastFourQuarters    }  }}fragment lastSevenDays on PeriodDates {  lastSevenDays: lastSevenDays {    dates(timeInterval: DAILY)    fromDate    toDate  }}fragment lastFourQuarters on PeriodDates {  lastFourQuarters: oneYear {    dates(timeInterval: QUARTERLY)    fromDate    toDate  }}

(3) query getPerformanceLastLoadedDate {    asOfDates(entity: PERFORMANCE) {        etlStartTime    }}

(4) query getExposureComparisonHeaderInfo {    portfolioGroups(code: "ds_nestedgrp") {        lastLoadedDate        portfolios {            code        }    }}

(5) query getTopBottomPerformersQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: MTD, limit: 5) {              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(6) query getTopBottomPerformersQuery {    insights(portfolioGroupCode: "ds_nestedgrp") {        topBottomPerformers(dateRange: QTD, limit: 5) {            top {                difference                portfolioName                portfolioCode            }            bottom {                difference                portfolioName                portfolioCode            }        }    }}

(7) query getTopBottomPerformersQuery {     insights(portfolioGroupCode: "ds_nestedgrp") {         topBottomPerformers(dateRange: YTD, limit: 5) {             top {                 difference                 portfolioName                 portfolioCode             }             bottom {                 difference                 portfolioName                 portfolioCode             }         }     } }


(8) query getTopBottomPerformersQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: ITD, limit: 5) {              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(9) query getTopBottomPerformersQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: OneYear, limit: 5) {              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(10) query getTopBottomPerformersQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: ThreeYears, limit: 5) {              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(11) query getTopBottomPerformersQuery {     insights(portfolioGroupCode: "ds_nestedgrp") {         topBottomPerformers(dateRange: FiveYears, limit: 5) {             top {                 difference                 portfolioName                 portfolioCode             }             bottom {                 difference                 portfolioName                 portfolioCode             }         }     } }

(12) query getActivityDatesQuery {      asOfDates(entity: ACTIVITY) {          etlStartTime          periodDates {              mtd{                  fromDate                  toDate                }              qtd{                  fromDate                  toDate                }              ytd {                  fromDate                  toDate                }          }      }  }

(13) query getExposureComparisonHeaderInfo {   portfolioGroups(code: "ds_nestedgrp") {       lastLoadedDate       portfolios {           code       }   } }


(14) query ActivitySummary {     insights(portfolioGroupCode: "ds_nestedgrp") {         ...summaryActivityFragment     } } fragment summaryActivityFragment on Reports {     summaryActivity(dateRange: YTD) {     beginningMarketValue     cashMarketValue     contributions     dividentsInterest     endingMarketValue     expenses     fees     feesAndExpenses     gainAndLoss     netFlows     realizedGL     unrealizedGL     withdrawals     changeMarketValuePercent } }

(15) query getExposureComparisonDatesQuery {     asOfDates(entity: EXPOSURE) {         periodDates {             mtd {                 fromDate               }             qtd {                 fromDate               }             ytd {                 fromDate               }         }         date     } }

(16) query getPortfolioGroups {   portfolioGroups {       code       name   }   }

(17) query getPropertyMetadata {       propertyMetadata(entity: INVESTMENT) {           code           name       }   }

(18) query getPortfolioGroups {      portfolioGroups {          code          name      }  }


(19) query getExposureComparisonHeaderInfo {       portfolioGroups(code: "ds_nestedgrp") {           lastLoadedDate           portfolios {               code           }       }   }

(20) query getPerformanceAnalysisCardQuery {      insights(portfolioGroupCode: "ds_nestedgrp") {          topBottomPerformers(dateRange: YTD, limit: 5) {              lastLoadedDate              top {                  difference                  portfolioName                  portfolioCode              }              bottom {                  difference                  portfolioName                  portfolioCode              }          }      }  }

(21) query getActivityDatesQuery {       asOfDates(entity: ACTIVITY) {           etlStartTime           periodDates {               mtd{                   fromDate                   toDate                 }               qtd{                   fromDate                   toDate                 }               ytd {                   fromDate                   toDate                 }           }       }   }

(22) query getExposureComparisonLastLoadedDate {asOfDates(entity: EXPOSURE) {    etlStartTime} }

(23) query getExposureComparisonHeaderInfo {        portfolioGroups(code: "ds_nestedgrp") {            lastLoadedDate            portfolios {                code            }        }    }

(24) query getAllActiveReturnDeviations($portfolioGroupCode: String!) {        insights(portfolioGroupCode: $portfolioGroupCode) {        mtd: activeReturnDeviations(period: MTD) {                    ...ActiveChartSchema                },qtd: activeReturnDeviations(period: QTD) {                    ...ActiveChartSchema                },ytd: activeReturnDeviations(period: YTD) {                    ...ActiveChartSchema                },sinceInception: activeReturnDeviations(period: ITD) {                    ...ActiveChartSchema                },oneYear: activeReturnDeviations(period: OneYear) {                    ...ActiveChartSchema                },threeYears: activeReturnDeviations(period: ThreeYears) {                    ...ActiveChartSchema                },fiveYears: activeReturnDeviations(period: FiveYears) {                    ...ActiveChartSchema                }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }    

variables:  {"portfolioGroupCode":"ds_nestedgrp"}

(25) query getExposureComparisonCardQuery ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"ds_nestedgrp","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":4,"dateRange":"YTD","aggregations":[{"method":"sum","field":"market_value"}]}

(26) query getActiveReturnDeviations($portfolioGroupCode: String!, $period: PerformanceDateRangeInput!) {       insights(portfolioGroupCode: $portfolioGroupCode) {         activeReturnDeviations(period: $period) {           ...ActiveChartSchema         }       }     }     fragment ActiveChartSchema on ActiveReturnDeviationReport {   deviations {       deviation   }   mean   standardDeviation   portfolioCount   sigmaIntervals {     totalMarketValue     portfolioCount     proportionOfAUM     sigmaFrom     sigmaTo   } }
"variables":{"portfolioGroupCode":"ds_nestedgrp","period":"YTD"}

(27) query ActivitySummary {       insights(portfolioGroupCode: "ds_nestedgrp") {           ...summaryActivityFragment       }   }   fragment summaryActivityFragment on Reports {       summaryActivity(dateRange: YTD) {       beginningMarketValue       cashMarketValue       contributions       dividentsInterest       endingMarketValue       expenses       fees       feesAndExpenses       gainAndLoss       netFlows       realizedGL       unrealizedGL       withdrawals       changeMarketValuePercent   }   }


2 Second load Dashboard
(1) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(2) query getPropertyMetadata {        propertyMetadata(entity: INVESTMENT) {            code            name        }    }

(3) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(4) query getExposureComparisonHeaderInfo {        portfolioGroups(code: "ds_nestedgrp") {            lastLoadedDate            portfolios {                code            }        }    }

(5) query getPerformanceAnalysisCardQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: YTD, limit: 5) {                lastLoadedDate                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(6) query getActivityDatesQuery {        asOfDates(entity: ACTIVITY) {            etlStartTime            periodDates {                mtd{                    fromDate                    toDate                  }                qtd{                    fromDate                    toDate                  }                ytd {                    fromDate                    toDate                  }            }        }    }

(7) query getExposureComparisonCardQuery ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"ds_nestedgrp","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":4,"dateRange":"YTD","aggregations":[{"method":"sum","field":"market_value"}]}


(8) query getActiveReturnDeviations($portfolioGroupCode: String!, $period: PerformanceDateRangeInput!) {        insights(portfolioGroupCode: $portfolioGroupCode) {          activeReturnDeviations(period: $period) {            ...ActiveChartSchema          }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }
"variables":{"portfolioGroupCode":"ds_nestedgrp","period":"YTD"}

(9) query ActivitySummary {        insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }


3 Exposure Comparison

(1) query getExposureComparisonLastLoadedDate {      asOfDates(entity: EXPOSURE) {          etlStartTime      }  }

(2) query getExposureComparisonHeaderInfo {     portfolioGroups(code: "ds_nestedgrp") {         lastLoadedDate         portfolios {             code         }     } }

(3) query getPortfolioGroups {     portfolioGroups {         code         name     } }

(4) query getPortfolioGroups {     portfolioGroups {         code         name     } }

(5) query getGroupByOptions {      groupByOptions {        l1        l2        l3        l4        name      }    }

(6) query getExposureTopBottomChanges ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {   insights(portfolioGroupCode: $code) {     topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {       top {         marketValueChange         weightChange         aumPercent         aumChangePercent         properties(codes: $propertyCodes) {           code           name           value           type         }       }       bottom {         marketValueChange         weightChange         aumPercent         aumChangePercent         properties(codes: $propertyCodes) {           code           name           value           type         }       }     }   } }
"variables":{"code":"ds_nestedgrp","dateRange":"YTD","groupBy":"ANALYST","propertyCodes":"ANALYST","limit":3,"aggregations":[{"method":"sum","field":"market_value"}]}


(7) query getExposureComparisonPositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) {   asOfDates(entity: EXPOSURE) {       periodDates {           ytd {                fromDate                toDate            }       }   }   insights(portfolioGroupCode: $code) {       positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {         asOfDate         marketValue         portfolioCode         portfolioName         price         quantity         weight         properties(codes: $propertyCodes) {           code           name           type           value           valueCode         }       }     }   }
"variables":{"code":"ds_nestedgrp","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}


4 Performance Analysis
(1) query getPerformanceLastLoadedDate {       asOfDates(entity: PERFORMANCE) {           etlStartTime       }   }

(2) query getExposureComparisonHeaderInfo {       portfolioGroups(code: "ds_nestedgrp") {           lastLoadedDate           portfolios {               code           }       }   }

(3) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: MTD, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(4) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: QTD, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(5) query getTopBottomPerformersQuery {       insights(portfolioGroupCode: "ds_nestedgrp") {           topBottomPerformers(dateRange: YTD, limit: 5) {               top {                   difference                   portfolioName                   portfolioCode               }               bottom {                   difference                   portfolioName                   portfolioCode               }           }       }   }

(6) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: ITD, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(7) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: OneYear, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(8) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: ThreeYears, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(9) query getTopBottomPerformersQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            topBottomPerformers(dateRange: FiveYears, limit: 5) {                top {                    difference                    portfolioName                    portfolioCode                }                bottom {                    difference                    portfolioName                    portfolioCode                }            }        }    }

(10) query getPerformanceAnalysisGridQuery {        insights(portfolioGroupCode: "ds_nestedgrp") {            performance {                marketValue                portfolioName                portfolioCode                properties(codes: ["Benchmark", "ANALYST"]) {                    code                    value                    type                }                mtd {        difference        index        portfolio    }qtd {        difference        index        portfolio    }ytd {        difference        index        portfolio    }sinceInception {        difference        index        portfolio    }oneYear {        difference        index        portfolio    }threeYears {        difference        index        portfolio    }fiveYears {        difference        index        portfolio    }            }        }    }

(11) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(12) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(13) query getPropertyMetadata {        propertyMetadata(entity: PORTFOLIO) {            code            name        }    }

(14) query getAllActiveReturnDeviations($portfolioGroupCode: String!) {        insights(portfolioGroupCode: $portfolioGroupCode) {        mtd: activeReturnDeviations(period: MTD) {                    ...ActiveChartSchema                },qtd: activeReturnDeviations(period: QTD) {                    ...ActiveChartSchema                },ytd: activeReturnDeviations(period: YTD) {                    ...ActiveChartSchema                },sinceInception: activeReturnDeviations(period: ITD) {                    ...ActiveChartSchema                },oneYear: activeReturnDeviations(period: OneYear) {                    ...ActiveChartSchema                },threeYears: activeReturnDeviations(period: ThreeYears) {                    ...ActiveChartSchema                },fiveYears: activeReturnDeviations(period: FiveYears) {                    ...ActiveChartSchema                }        }      }      fragment ActiveChartSchema on ActiveReturnDeviationReport {    deviations {        deviation    }    mean    standardDeviation    portfolioCount    sigmaIntervals {      totalMarketValue      portfolioCount      proportionOfAUM      sigmaFrom      sigmaTo    }  }
"variables":{"portfolioGroupCode":"ds_nestedgrp"}

(15) query getPerformanceAnalysisGridQuery {   insights(portfolioGroupCode: "ds_nestedgrp") {       performance {           marketValue           portfolioName           portfolioCode           properties(codes: ["Benchmark", "ANALYST"]) {               code               value               type           }           mtd {   difference   index   portfolio    }qtd {   difference   index   portfolio    }ytd {   difference   index   portfolio    }sinceInception {   difference   index   portfolio    }oneYear {   difference   index   portfolio    }threeYears {   difference   index   portfolio    }fiveYears {   difference   index   portfolio    }       }   }    }

5 Activity

(1) query getActivityDatesQuery {        asOfDates(entity: ACTIVITY) {            etlStartTime            periodDates {                mtd{                    fromDate                    toDate                  }                qtd{                    fromDate                    toDate                  }                ytd {                    fromDate                    toDate                  }            }        }    }

(2) query getExposureComparisonHeaderInfo {        portfolioGroups(code: "ds_nestedgrp") {            lastLoadedDate            portfolios {                code            }        }    }

(3) query ActivitySummary {        insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(4) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(5) query getPortfolioGroups {        portfolioGroups {            code            name        }    }

(6) query getPropertyMetadata {        propertyMetadata(entity: PORTFOLIO) {            code            name        }    }

(7) query getActivity {        asOfDates(entity: ACTIVITY) {        periodDates {            ytd {                 fromDate                 toDate             }        }    }        insights(portfolioGroupCode: "ds_nestedgrp") {            ...summaryActivityFragment            activities(dateRange: YTD, grouping: { aggregate: { value: sum }, groupby: "ANALYST"}) {beginningMarketValue endingMarketValue changeMarketValuePercent cashMarketValue                feesAndExpenses {value  fees {value percent} expenses {value}}                gainAndLoss {percent value dividentsInterest {value} realizedGL {value} unrealizedGL {value}}netFlows {value percent contributions {value} withdrawals {value}}                properties(codes: ["ANALYST"]) {                    code                    value                    type                }            }           }    }        fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }


6 Household Details

(1) query getHouseholdLastLoadedDate {  asOfDates(entity: PORTFOLIO_GROUP) {    etlStartTime  }}

(2) query getAllHouseholdGroupsQuery {  portfolioGroups(groupTypes: HOUSEHOLD) {    code    name  }}

(3) query getGroupByOptions {      groupByOptions {        l1        l2        l3        l4        name      }    }

(4) query getPropertyMetadata {      propertyMetadata(entity: PORTFOLIO) {          code          name      }  }

(5) query getPropertyMetadata {      propertyMetadata(entity: PORTFOLIO) {          code          name      }  }

(6) query getPropertyMetadata {      propertyMetadata(entity: INVESTMENT) {          code          name      }  }

(7) query getHouseholdHeaderData($code: String!, $dateRange: ActivityDateRangeInput!) {    insights(portfolioGroupCode: $code) {      summaryActivity(dateRange: $dateRange) {        cashMarketValue      }    }    portfolioGroups(code: $code) {      portfolios {        code      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD"}

(8) query ActivitySummary {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(9) query getHouseholdTopBottomHoldings ($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }        bottom {          marketValueChange          weightChange          aumPercent          aumChangePercent          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD","groupBy":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}],"limit":3,"propertyCodes":"ANALYST"}

(10) query getExposureComparisonPositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) {    asOfDates(entity: EXPOSURE) {        periodDates {            ytd {                 fromDate                 toDate             }        }    }    insights(portfolioGroupCode: $code) {        positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {          asOfDate          marketValue          portfolioCode          portfolioName          price          quantity          weight          properties(codes: $propertyCodes) {            code            name            type            value            valueCode          }        }      }    }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}

(11) query getPerformanceAnalysisGridQuery {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            performance {                marketValue                portfolioName                portfolioCode                properties(codes: ["Benchmark", "ANALYST"]) {                    code                    value                    type                }                mtd {        difference        index        portfolio    }qtd {        difference        index        portfolio    }ytd {        difference        index        portfolio    }sinceInception {        difference        index        portfolio    }oneYear {        difference        index        portfolio    }threeYears {        difference        index        portfolio    }fiveYears {        difference        index        portfolio    }            }        }    }

(12) query getActivity {        asOfDates(entity: ACTIVITY) {        periodDates {            ytd {                 fromDate                 toDate             }        }    }        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            ...summaryActivityFragment            activities(dateRange: YTD, grouping: { aggregate: { value: sum }, groupby: "ANALYST"}) {beginningMarketValue endingMarketValue changeMarketValuePercent cashMarketValue                feesAndExpenses {value  fees {value percent} expenses {value}}                gainAndLoss {percent value dividentsInterest {value} realizedGL {value} unrealizedGL {value}}netFlows {value percent contributions {value} withdrawals {value}}                properties(codes: ["ANALYST"]) {                    code                    value                    type                }            }           }    }        fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(13) query getHouseholdExposureTileData($code: String!, $dateRange: PositionDateRangeInput!, $aggregations: [PositionAggregationsInput!]!, $groupBy: [String!]!, $limit: Int, $propertyCodes: [String!]) {    insights(portfolioGroupCode: $code) {      topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {        top {          aumChangePercent          aumPercent          marketValueChange          properties(codes: $propertyCodes) {            code            name            value            type          }        }      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp","groupBy":"ANALYST","dateRange":"YTD","limit":8,"aggregations":[{"method":"sum","field":"market_value"}],"propertyCodes":"ANALYST"}

(14) query getActivityDatesQuery {        asOfDates(entity: ACTIVITY) {            etlStartTime            periodDates {                mtd{                    fromDate                    toDate                  }                qtd{                    fromDate                    toDate                  }                ytd {                    fromDate                    toDate                  }            }        }    }


(15) query getHouseholdPerformanceTileData($code: String) {    portfolioGroups(code: $code) {      portfolios {        name        code        performance {          ytd {            difference          }          mtd {            difference          }          qtd {            difference          }        }      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp"}

(16) query ActivitySummary {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(17) query getHouseholdSummaryTileData($code:String!, $dateRange: PortfolioActivityDateRangeInput!) {    portfolioGroups(code: $code) {      portfolios {        code        name        taxStatus        summaryActivity(dateRange: $dateRange) {          cashMarketValue          endingMarketValue          realizedGL        }        performance {          marketValue        }      }    }  }
"variables":{"code":"auto_BrokerDupNameGrp","dateRange":"YTD"}

(18) query getActivityDatesQuery {        asOfDates(entity: ACTIVITY) {            etlStartTime            periodDates {                mtd{                    fromDate                    toDate                  }                qtd{                    fromDate                    toDate                  }                ytd {                    fromDate                    toDate                  }            }        }    }

(19) query ActivitySummary {        insights(portfolioGroupCode: "auto_BrokerDupNameGrp") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Reports {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

7 Portfolio Details

(1) query getPortfolioDetailInfo {        portfolios(code: "ds_multicash") {            name          }    }

(2) query getPortfolioDetailLastLoadedDate {        asOfDates(entity: PORTFOLIO) {            etlStartTime        }    }

(3) query getGroupByOptions {        groupByOptions {          l1          l2          l3          l4          name        }      }

(4) query GetTMV {        portfolios(code: "ds_multicash") {          summaryActivity(dateRange: MTD) {            endingMarketValue          }        }      }

(5) query getRGL {        portfolios(code: "ds_multicash") {          summaryActivity(dateRange: YTD) {            realizedGL          }        }      }

(6) query getPropertyMetadata {        propertyMetadata(entity: INVESTMENT) {            code            name        }    }

(7) query GetTotalCash {        portfolios(code: "ds_multicash") {          summaryActivity(dateRange: MTD) {            cashMarketValue            endingMarketValue          }        }      }

(8) query getURGL {        portfolios(code: "ds_multicash") {          summaryActivity(dateRange: ITD) {            unrealizedGL          }        }      }

(9) query GetAttributes {      portfolios(code: "ds_multicash") {        openDate        closingMethod        status        taxStatus        properties(codes: "INVESTMENT_GOAL") {          code          value          type        }      }  }

(10) query GetPerformanceGraph {        portfolios(code: "ds_multicash") {          performance {            mtd {              portfolio              index              difference            }            qtd {              portfolio              index              difference            }            ytd {              portfolio              index              difference            }            oneYear {              portfolio              index              difference            }            threeYears {              portfolio              index              difference            }            fiveYears {              portfolio              index              difference            }            sinceInception {              portfolio              index              difference            }          }        }      }

(11) query getActivityDatesQuery {        asOfDates(entity: ACTIVITY) {            etlStartTime            periodDates {                mtd{                    fromDate                    toDate                  }                qtd{                    fromDate                    toDate                  }                ytd {                    fromDate                    toDate                  }            }        }    }

(12) query getPortfolioExposurePositions($code: String!, $dateRange: PositionDateRangeInput, $filters: [GenericFilterInput!], $grouping: PositionGroupingInput, $propertyCodes: [String!]) {    asOfDates(entity: EXPOSURE) {        periodDates {            ytd {                 fromDate                 toDate             }        }    }    portfolios(code: $code) {        positions(dateRange: $dateRange, filters: $filters, grouping: $grouping) {          asOfDate          marketValue          portfolioCode          portfolioName          price          quantity          weight          properties(codes: $propertyCodes) {            code            name            type            value            valueCode          }        }      }    }
"variables":{"code":"ds_multicash","dateRange":"YTD","grouping":{"groupby":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}]},"propertyCodes":["ANALYST","INVESTMENT"],"filters":[]}

(13) query getExposureHeader($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {        portfolios(code: $code) {          topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {                bottom {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }                top {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }              }        }    }
"variables":{"code":"ds_multicash","dateRange":"YTD","groupBy":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}],"limit":3,"propertyCodes":"ANALYST"}

(14) query getExposureComparison($code: String!, $dateRange: PositionDateRangeInput!, $aggregations: [PositionAggregationsInput!]!, $groupBy: [String!]!, $limit: Int, $propertyCodes: [String!]) {      portfolios(code: $code) {        topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {          top {            aumChangePercent            aumPercent            marketValueChange            properties(codes: $propertyCodes) {              code              name              value              type            }          }        }      }    }
"variables":{"code":"ds_multicash","groupBy":"ANALYST","dateRange":"YTD","limit":8,"aggregations":[{"method":"sum","field":"market_value"}]}

(15) query ActivitySummaryPortDetails {        portfolios(code: "ds_multicash") {            ...summaryActivityFragment        }    }    fragment summaryActivityFragment on Portfolio {        summaryActivity(dateRange: YTD) {        beginningMarketValue        cashMarketValue        contributions        dividentsInterest        endingMarketValue        expenses        fees        feesAndExpenses        gainAndLoss        netFlows        realizedGL        unrealizedGL        withdrawals        changeMarketValuePercent    }    }

(16) query getExposureHeader($code: String!, $dateRange: PositionDateRangeInput!, $groupBy: [String!]!, $aggregations: [PositionAggregationsInput!]!, $limit: Int, $propertyCodes: [String!]) {        portfolios(code: $code) {          topBottomHoldings(dateRange: $dateRange, grouping: {aggregations: $aggregations, groupby: $groupBy}, limit: $limit) {                bottom {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }                top {                  properties(codes: $propertyCodes) {                    code                    name                    value                    type                  }                  marketValueChange                  aumChangePercent                }              }        }    }
"variables":{"code":"ds_multicash","dateRange":"YTD","groupBy":"ANALYST","aggregations":[{"method":"sum","field":"market_value"}],"limit":3,"propertyCodes":"ANALYST"}
