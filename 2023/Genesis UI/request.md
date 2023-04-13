Portfolio Analysis

| S/N  | ui name                                        | graphql schema                | graphql   field          | number of times | parameters                                              |
| ---- | ---------------------------------------------- | ----------------------------- | ------------------------ | --------------- | ------------------------------------------------------- |
| /    | **Dashboard(27) - First query**                |                               |                          |                 |                                                         |
| 1.1  | getAllETLAsOfDate                              | asOfDates                     | date/etlStartTime        | 5               | PORTFOLIO/PORTFOLIO_GROUP/EXPOSURE/PERFORMANCE/ACTIVITY |
| 1.2  | getExposureComparisonMultidatePresetDatesQuery | asOfDates                     | periodDates              | 1               | EXPOSURE                                                |
| 1.3  | getPerformanceLastLoadedDate                   | asOfDate                      | etlStartTime             | 1               | PERFORMANCE                                             |
| 1.4  | getExposureComparisonHeaderInfo                | portfolioGroups->portfolios   | code                     | 1               |                                                         |
| 1.5  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | MTD                                                     |
| 1.6  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | QTD                                                     |
| 1.7  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | YTD                                                     |
| 1.8  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | ITD                                                     |
| 1.9  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | OneYear                                                 |
| 1.10 | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | ThreeYears                                              |
| 1.11 | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | FiveYearsasOfDates                                      |
| 1.12 | getActivityDatesQuery                          | asOfDates                     | periodDates/etlStartTime | 1               | ACTIVITY                                                |
| 1.13 | getExposureComparisonHeaderInfo                | portfolioGroups->portfolios   | code                     | 1               |                                                         |
| 1.14 | ActivitySummary                                | summaryActivity               | ….                       | 1               |                                                         |
| 1.15 | getExposureComparisonDatesQuery                | asOfDates                     | periodDates              | 1               | EXPOSURE                                                |
| 1.16 | getPortfolioGroups                             | portfolioGroups               | code/name                | 1               |                                                         |
| 1.17 | getPropertyMetadata                            | propertyMetadata              | code/name                | 1               | INVESTMENT                                              |
| 1.18 | getPortfolioGroups                             | portfolioGroups               | code/name                | 1               |                                                         |
| 1.19 | getExposureComparisonHeaderInfo                | portfolioGroups-> portfolios  | code                     | 1               |                                                         |
| 1.20 | getPerformanceAnalysisCardQuery                | topBottomPerformers           | top/bottom               | 1               | YTD                                                     |
| 1.21 | getActivityDatesQuery                          | asOfDates                     | periodDates/etlStartTime | 1               | ACTIVITY                                                |
| 1.22 | getExposureComparisonLastLoadedDate            | asOfDates                     | etlStartTime             | 1               | EXPOSURE                                                |
| 1.23 | getExposureComparisonHeaderInfo                | portfolioGroups -> portfolios | code                     | 1               |                                                         |
| 1.24 | getAllActiveReturnDeviations                   | activeReturnDeviations        | …                        | 7               | MTD/QTD/YTD/ITD/OneYear/ThreeYears/FiveYears            |
| 1.25 | getExposureComparisonCardQuery                 | topBottomHoldings             | top/bottom               | 1               |                                                         |
| 1.26 | getActiveReturnDeviations                      | activeReturnDeviations        |                          | 1               | YTD                                                     |
| 1.27 | ActivitySummary                                | summaryActivity               | …                        | 1               | YTD                                                     |

| S/N  | ui name                         | graphql schema              | graphql   field | number of times | parameters |
| ---- | ------------------------------- | --------------------------- | --------------- | --------------- | ---------- |
| /    | **Dashboard(9) - Second query** |                             |                 |                 |            |
| 2.1  | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |            |
| 2.2  | getPropertyMetadata             | propertyMetadata            | code/name       | 1               | INVESTMENT |
| 2.3  | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |            |
| 2.4  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code            | 1               |            |
| 2.5  | getPerformanceAnalysisCardQuery | topBottomPerformers         | top/bottom      | 1               |            |
| 2.6  | getActivityDatesQuery           | asOfDates                   | periodDates     | 1               | ACTIVITY   |
| 2.7  | getExposureComparisonCardQuery  | topBottomHoldings           | top/bottom      |                 |            |
| 2.8  | getActiveReturnDeviations       | activeReturnDeviations      | …               | 1               | YTD        |
| 2.9  | ActivitySummary                 | summaryActivity             | …               | 1               |            |

| S/N  | ui name                             | graphql schema              | graphql   field          | number of times | parameters |
| ---- | ----------------------------------- | --------------------------- | ------------------------ | --------------- | ---------- |
| /    | **Exposure Comparison(7)**          |                             |                          |                 |            |
| 3.1  | getExposureComparisonLastLoadedDate | asOfDates                   | etlStartTime             | 1               | EXPOSURE   |
| 3.2  | getExposureComparisonHeaderInfo     | portfolioGroups->portfolios | code                     | 1               |            |
| 3.3  | getPortfolioGroups                  | portfolioGroups             | code/name                | 1               |            |
| 3.4  | getPortfolioGroups                  | portfolioGroups             | code                     | 1               |            |
| 3.5  | getGroupByOptions                   | groupByOptions              | L1/l2/l3/l4/name         | 1               |            |
| 3.6  | getExposureTopBottomChanges         | topBottomHoldings           | top/bottom               |                 |            |
| 3.7  | getExposureComparisonPositions      | asOfDates/positions         | periodDates/group+filter |                 |            |

| S/N  | ui name                         | graphql schema              | graphql   field | number of times | parameters                                   |
| ---- | ------------------------------- | --------------------------- | --------------- | --------------- | -------------------------------------------- |
| /    | **Performance Analysis(15)**    |                             |                 |                 |                                              |
| 4.1  | getPerformanceLastLoadedDate    | asOfDates                   | etlStartTime    | 1               | PERFORMANCE                                  |
| 4.2  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code            | 1               |                                              |
| 4.3  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | MTD                                          |
| 4.4  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | QTD                                          |
| 4.5  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | YTD                                          |
| 4.6  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | ITD                                          |
| 4.7  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | OneYear                                      |
| 4.8  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | ThreeYears                                   |
| 4.9  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | FiveYears                                    |
| 4.10 | getPerformanceAnalysisGridQuery | performance                 | …               | 1               |                                              |
| 4.11 | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |                                              |
| 4.12 | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |                                              |
| 4.13 | getPropertyMetadata             | propertyMetadata            | Code            |                 | PORTFOLIO                                    |
| 4.14 | getAllActiveReturnDeviations    | activeReturnDeviations      | …               | 7               | MTD/QTD/YTD/ITD/OneYear/ThreeYears/FiveYears |
| 4.15 | getPerformanceAnalysisGridQuery | performance                 | …               | 1               |                                              |

| S/N  | ui name                         | graphql schema              | graphql   field          | number of times | parameters   |
| ---- | ------------------------------- | --------------------------- | ------------------------ | --------------- | ------------ |
| /    | **Activity(7)**                 |                             |                          |                 |              |
| 5.1  | getActivityDatesQuery           | asOfDates                   | etlStartTime/periodDates | 1               | ACTIVITY     |
| 5.2  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code                     | 1               |              |
| 5.3  | ActivitySummary                 | summaryActivity             | …                        | 1               | YTD          |
| 5.4  | getPortfolioGroups              | portfolioGroups             | code/name                | 1               |              |
| 5.5  | getPortfolioGroups              | portfolioGroups             | code/name                | 1               |              |
| 5.6  | getPropertyMetadata             | propertyMetadata            | code/name                |                 | PORTFOLIO    |
| 5.7  | getActivity                     | asOfDates/activities        | periodDates/grouping     |                 | ACTIVITY/YTD |

| S/N  | ui name                         | graphql schema                                           | graphql   field           | number of times | parameters         |
| ---- | ------------------------------- | -------------------------------------------------------- | ------------------------- | --------------- | ------------------ |
| /    | **Household Details(19)**       |                                                          |                           |                 |                    |
| 6.1  | getHouseholdLastLoadedDate      | asOfDates                                                | etlStartTime              | 1               | PORTFOLIO_GROUP    |
| 6.2  | getAllHouseholdGroupsQuery      | portfolioGroups                                          | code/name                 | 1               | HOUSEHOLD          |
| 6.3  | getGroupByOptions               | groupByOptions                                           | L1/l2/l3/l4/name          | 1               |                    |
| 6.4  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          |
| 6.5  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          |
| 6.6  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          |
| 6.7  | getHouseholdHeaderData          | summaryActivity/portfolioGroups->portfolios              | cashMarketValue/code      | 1               |                    |
| 6.8  | ActivitySummary                 | summaryActivity                                          | …                         | 1               | YTD                |
| 6.9  | getHouseholdTopBottomHoldings   | topBottomHoldings                                        | top/bottom                | 1               |                    |
| 6.10 | getExposureComparisonPositions  | asOfDates/positions                                      | periodDates/filter+ group | 1               |                    |
| 6.11 | getPerformanceAnalysisGridQuery | performance                                              | …                         | 1               |                    |
| 6.12 | getActivitygetActivity          | asOfDates/summaryActivity/activities                     | …                         | 1               | ACTIVITY/YTD/group |
| 6.13 | getHouseholdExposureTileData    | topBottomHoldings                                        | top                       | 1               | limit=8            |
| 6.14 | getActivityDatesQuery           | asOfDates                                                | etlStartTime/periodDates  | 1               | ACTIVITY           |
| 6.15 | getHouseholdPerformanceTileData | portfolioGroups->portfolios                              | code/name/performance     | 1               |                    |
| 6.16 | ActivitySummary                 | summaryActivity                                          | …                         | 1               | YTD                |
| 6.17 | getHouseholdSummaryTileData     | portfolioGroups->portfolios->summaryActivity/performance | …                         | 1               |                    |
| 6.18 | getActivityDatesQuery           | asOfDates                                                | etlStartTime/periodDates  | 1               | ACTIVITY           |
| 6.19 | ActivitySummary                 | summaryActivity                                          | …                         | 1               |                    |

| S/N  | ui name                          | graphql schema                  | graphql   field                                       | number of times | parameters            |
| ---- | -------------------------------- | ------------------------------- | ----------------------------------------------------- | --------------- | --------------------- |
| /    | **Portfolio Details(16)**        |                                 |                                                       |                 |                       |
| 7.1  | getPortfolioDetailInfo           | portfolios                      | name                                                  | 1               |                       |
| 7.2  | getPortfolioDetailLastLoadedDate | asOfDates                       | etlStartTime                                          | 1               | PORTFOLIO             |
| 7.3  | getGroupByOptions                | groupByOptions                  | L1/l2/l3/l4/name                                      | 1               |                       |
| 7.4  | GetTMV                           | portfolios->summaryActivity     | endingMarketValue                                     | 1               | MTD                   |
| 7.5  | getRGL                           | portfolios->summaryActivity     | realizedGL                                            | 1               | YTD                   |
| 7.6  | getPropertyMetadata              | propertyMetadata                | code/name                                             | 1               | INVESTMENT            |
| 7.7  | GetTotalCash                     | portfolios->summaryActivity     | cashMarketValue /          endingMarketValue          | 1               | MTD                   |
| 7.8  | getURGL                          | portfolios->summaryActivity     | unrealizedGL                                          | 1               | ITD                   |
| 7.9  | GetAttributes                    | portfolios                      | openDate      closingMethod  /status        taxStatus | 1               |                       |
| 7.10 | GetPerformanceGraph              | portfolios->performance         | …                                                     | 1               |                       |
| 7.11 | getActivityDatesQuery            | asOfDates                       | etlStartTime/periodDates                              | 1               | ACTIVITY              |
| 7.12 | getPortfolioExposurePositions    | asOfDates/portfolios->positions | periodDates/…                                         | 1               | EXPOSURE/filter+group |
| 7.13 | getExposureHeader                | portfolios->topBottomHoldings   | top/bottom                                            | 1               | Limit 3               |
| 7.14 | getExposureComparison            | portfolios->topBottomHoldings   | top                                                   | 1               | Limit 8               |
| 7.15 | ActivitySummaryPortDetails       | portfolios->summaryActivity     | …                                                     | 1               | YTD                   |
| 7.16 | getExposureHeader                | portfolios->topBottomHoldings   | top/bottom                                            | 1               | Limit 3               |