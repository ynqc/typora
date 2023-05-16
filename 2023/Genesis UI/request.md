页面可缓存

1 合并AsOfDate， 并缓存结果

2 合并propertyMetadata请求并缓存



Portfolio Analysis

| S/N  | ui name                                        | graphql schema                | graphql   field          | number of times | parameters                                              |                                   |      |
| ---- | ---------------------------------------------- | ----------------------------- | ------------------------ | --------------- | ------------------------------------------------------- | --------------------------------- | ---- |
| /    | **Dashboard(27) - First query**                |                               |                          |                 |                                                         |                                   |      |
| 1.1  | getAllETLAsOfDate                              | asOfDates                     | date/etlStartTime        | 5               | PORTFOLIO/PORTFOLIO_GROUP/EXPOSURE/PERFORMANCE/ACTIVITY | 1，2，3，12，15， 21，22 合并请求 | 1    |
| 1.2  | getExposureComparisonMultidatePresetDatesQuery | asOfDates                     | periodDates              | 1               | EXPOSURE                                                | 删除，用1缓存结果                 |      |
| 1.3  | getPerformanceLastLoadedDate                   | asOfDate                      | etlStartTime             | 1               | PERFORMANCE                                             | 删除，用1缓存结果                 |      |
| 1.4  | getExposureComparisonHeaderInfo                | portfolioGroups->portfolios   | code                     | 1               |                                                         | 删除，用1缓存结果                 |      |
| 1.5  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | MTD                                                     | 建议可按需                        | 2    |
| 1.6  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | QTD                                                     | 建议可按需                        |      |
| 1.7  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | YTD                                                     | 建议可按需                        |      |
| 1.8  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | ITD                                                     | 建议可按需                        |      |
| 1.9  | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | OneYear                                                 | 建议可按需                        |      |
| 1.10 | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | ThreeYears                                              | 建议可按需                        |      |
| 1.11 | getTopBottomPerformersQuery                    | topBottomPerformers           | top/bottom               | 1               | FiveYearsasOfDates                                      | 建议可按需                        |      |
| 1.12 | getActivityDatesQuery                          | asOfDates                     | periodDates/etlStartTime | 1               | ACTIVITY                                                | 删除，用1缓存结果                 |      |
| 1.13 | getExposureComparisonHeaderInfo                | portfolioGroups->portfolios   | code                     | 1               |                                                         | 删除， 与4重复                    |      |
| 1.14 | ActivitySummary                                | summaryActivity               | ….                       | 1               |                                                         |                                   | 3    |
| 1.15 | getExposureComparisonDatesQuery                | asOfDates                     | periodDates              | 1               | EXPOSURE                                                | 删除，用1缓存结果                 |      |
| 1.16 | getPortfolioGroups                             | portfolioGroups               | code/name                | 1               |                                                         | 16，于23合并                      | 4    |
| 1.17 | getPropertyMetadata                            | propertyMetadata              | code/name                | 1               | INVESTMENT                                              | 用缓存                            | 5    |
| 1.18 | getPortfolioGroups                             | portfolioGroups               | code/name                | 1               |                                                         | 删除，于16重复                    |      |
| 1.19 | getExposureComparisonHeaderInfo                | portfolioGroups-> portfolios  | code                     | 1               |                                                         | 于23重复，删除，用16              |      |
| 1.20 | getPerformanceAnalysisCardQuery                | topBottomPerformers           | top/bottom               | 1               | YTD                                                     | 删除，重复                        |      |
| 1.21 | getActivityDatesQuery                          | asOfDates                     | periodDates/etlStartTime | 1               | ACTIVITY                                                | 删除， 用1                        |      |
| 1.22 | getExposureComparisonLastLoadedDate            | asOfDates                     | etlStartTime             | 1               | EXPOSURE                                                | 删除， 用1                        |      |
| 1.23 | getExposureComparisonHeaderInfo                | portfolioGroups -> portfolios | code                     | 1               |                                                         | 删除，用16                        |      |
| 1.24 | getAllActiveReturnDeviations                   | activeReturnDeviations        | …                        | 7               | MTD/QTD/YTD/ITD/OneYear/ThreeYears/FiveYears            | 于26重复                          | 6    |
| 1.25 | getExposureComparisonCardQuery                 | topBottomHoldings             | top/bottom               | 1               |                                                         |                                   | 7    |
| 1.26 | getActiveReturnDeviations                      | activeReturnDeviations        |                          | 1               | YTD                                                     | 删除，用24                        |      |
| 1.27 | ActivitySummary                                | summaryActivity               | …                        | 1               | YTD                                                     |                                   | 8    |

| S/N  | ui name                         | graphql schema              | graphql   field | number of times | parameters |               |      |
| ---- | ------------------------------- | --------------------------- | --------------- | --------------- | ---------- | ------------- | ---- |
| /    | **Dashboard(9) - Second query** |                             |                 |                 |            |               |      |
| 2.1  | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |            | 1与4合并      | 1    |
| 2.2  | getPropertyMetadata             | propertyMetadata            | code/name       | 1               | INVESTMENT | 用缓存        |      |
| 2.3  | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |            | 删除，与1重复 |      |
| 2.4  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code            | 1               |            | 删除，与1合并 |      |
| 2.5  | getPerformanceAnalysisCardQuery | topBottomPerformers         | top/bottom      | 1               |            |               | 2    |
| 2.6  | getActivityDatesQuery           | asOfDates                   | periodDates     | 1               | ACTIVITY   | 删除， 用缓存 |      |
| 2.7  | getExposureComparisonCardQuery  | topBottomHoldings           | top/bottom      |                 |            |               | 3    |
| 2.8  | getActiveReturnDeviations       | activeReturnDeviations      | …               | 1               | YTD        |               | 4    |
| 2.9  | ActivitySummary                 | summaryActivity             | …               | 1               |            |               | 5    |

| S/N  | ui name                             | graphql schema              | graphql   field          | number of times | parameters |               |      |
| ---- | ----------------------------------- | --------------------------- | ------------------------ | --------------- | ---------- | ------------- | ---- |
| /    | **Exposure Comparison(7)**          |                             |                          |                 |            |               |      |
| 3.1  | getExposureComparisonLastLoadedDate | asOfDates                   | etlStartTime             | 1               | EXPOSURE   | 用缓存        |      |
| 3.2  | getExposureComparisonHeaderInfo     | portfolioGroups->portfolios | code                     | 1               |            | 2，3合并      | 1    |
| 3.3  | getPortfolioGroups                  | portfolioGroups             | code/name                | 1               |            | 删除，与2合并 |      |
| 3.4  | getPortfolioGroups                  | portfolioGroups             | code                     | 1               |            | 删除，与2冲   |      |
| 3.5  | getGroupByOptions                   | groupByOptions              | L1/l2/l3/l4/name         | 1               |            |               | 2    |
| 3.6  | getExposureTopBottomChanges         | topBottomHoldings           | top/bottom               |                 |            |               | 3    |
| 3.7  | getExposureComparisonPositions      | asOfDates/positions         | periodDates/group+filter |                 |            | 删除asOfDates | 4    |

| S/N  | ui name                         | graphql schema              | graphql   field | number of times | parameters                                   |                |      |
| ---- | ------------------------------- | --------------------------- | --------------- | --------------- | -------------------------------------------- | -------------- | ---- |
| /    | **Performance Analysis(15)**    |                             |                 |                 |                                              |                |      |
| 4.1  | getPerformanceLastLoadedDate    | asOfDates                   | etlStartTime    | 1               | PERFORMANCE                                  | 用缓存         |      |
| 4.2  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code            | 1               |                                              | 2和11 合并     | 1    |
| 4.3  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | MTD                                          | 建议可按需     | 2    |
| 4.4  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | QTD                                          | 建议可按需     |      |
| 4.5  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | YTD                                          | 建议可按需     |      |
| 4.6  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | ITD                                          | 建议可按需     |      |
| 4.7  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | OneYear                                      | 建议可按需     |      |
| 4.8  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | ThreeYears                                   | 建议可按需     |      |
| 4.9  | getTopBottomPerformersQuery     | topBottomPerformers         | …               | 1               | FiveYears                                    | 建议可按需     |      |
| 4.10 | getPerformanceAnalysisGridQuery | performance                 | …               | 1               |                                              | 与15重复       | 3    |
| 4.11 | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |                                              | 删除， 用2     |      |
| 4.12 | getPortfolioGroups              | portfolioGroups             | code/name       | 1               |                                              | 删除，与11重复 |      |
| 4.13 | getPropertyMetadata             | propertyMetadata            | Code            |                 | PORTFOLIO                                    | 用缓存         |      |
| 4.14 | getAllActiveReturnDeviations    | activeReturnDeviations      | …               | 7               | MTD/QTD/YTD/ITD/OneYear/ThreeYears/FiveYears |                | 4    |
| 4.15 | getPerformanceAnalysisGridQuery | performance                 | …               | 1               |                                              | 删除与10重复   |      |

| S/N  | ui name                         | graphql schema              | graphql   field          | number of times | parameters   |                      |      |
| ---- | ------------------------------- | --------------------------- | ------------------------ | --------------- | ------------ | -------------------- | ---- |
| /    | **Activity(7)**                 |                             |                          |                 |              |                      |      |
| 5.1  | getActivityDatesQuery           | asOfDates                   | etlStartTime/periodDates | 1               | ACTIVITY     | 用缓存               |      |
| 5.2  | getExposureComparisonHeaderInfo | portfolioGroups->portfolios | code                     | 1               |              | 与4合并              | 1    |
| 5.3  | ActivitySummary                 | summaryActivity             | …                        | 1               | YTD          |                      | 2    |
| 5.4  | getPortfolioGroups              | portfolioGroups             | code/name                | 1               |              | 4，5重复，删除， 用2 |      |
| 5.5  | getPortfolioGroups              | portfolioGroups             | code/name                | 1               |              | 4，5重复，删除， 用2 |      |
| 5.6  | getPropertyMetadata             | propertyMetadata            | code/name                |                 | PORTFOLIO    |                      | 3    |
| 5.7  | getActivity                     | asOfDates/activities        | periodDates/grouping     |                 | ACTIVITY/YTD | 删除asOfDates        | 4    |

| S/N  | ui name                         | graphql schema                                           | graphql   field           | number of times | parameters         |                                              |      |
| ---- | ------------------------------- | -------------------------------------------------------- | ------------------------- | --------------- | ------------------ | -------------------------------------------- | ---- |
| /    | **Household Details(19)**       |                                                          |                           |                 |                    |                                              |      |
| 6.1  | getHouseholdLastLoadedDate      | asOfDates                                                | etlStartTime              | 1               | PORTFOLIO_GROUP    | 用缓存                                       |      |
| 6.2  | getAllHouseholdGroupsQuery      | portfolioGroups                                          | code/name                 | 1               | HOUSEHOLD          |                                              | 1    |
| 6.3  | getGroupByOptions               | groupByOptions                                           | L1/l2/l3/l4/name          | 1               |                    |                                              | 2    |
| 6.4  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          | 4，5，6重复用缓存，                          |      |
| 6.5  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          | 删除                                         |      |
| 6.6  | getPropertyMetadata             | propertyMetadata                                         | code/name                 | 1               | PORTFOLIO          | 删除                                         |      |
| 6.7  | getHouseholdHeaderData          | summaryActivity/portfolioGroups->portfolios              | cashMarketValue/code      | 1               |                    | summaryActivity删除，用16，portfolios与2合并 |      |
| 6.8  | ActivitySummary                 | summaryActivity                                          | …                         | 1               | YTD                | 删除，用16                                   |      |
| 6.9  | getHouseholdTopBottomHoldings   | topBottomHoldings                                        | top/bottom                | 1               | limit=3            | limit 8                                      | 3    |
| 6.10 | getExposureComparisonPositions  | asOfDates/positions                                      | periodDates/filter+ group | 1               |                    | asOfDates删除                                | 4    |
| 6.11 | getPerformanceAnalysisGridQuery | performance                                              | …                         | 1               |                    |                                              | 5    |
| 6.12 | getActivitygetActivity          | asOfDates/summaryActivity/activities                     | …                         | 1               | ACTIVITY/YTD/group | 只留activities                               | 6    |
| 6.13 | getHouseholdExposureTileData    | topBottomHoldings                                        | top                       | 1               | limit=8            | 删除，用9                                    |      |
| 6.14 | getActivityDatesQuery           | asOfDates                                                | etlStartTime/periodDates  | 1               | ACTIVITY           | 用缓存                                       |      |
| 6.15 | getHouseholdPerformanceTileData | portfolioGroups->portfolios                              | code/name/performance     | 1               |                    | 只performance                                | 7    |
| 6.16 | ActivitySummary                 | summaryActivity                                          | …                         | 1               | YTD                | 与19重复                                     | 8    |
| 6.17 | getHouseholdSummaryTileData     | portfolioGroups->portfolios->summaryActivity/performance | …                         | 1               |                    | 删除， 与11，16重复                          |      |
| 6.18 | getActivityDatesQuery           | asOfDates                                                | etlStartTime/periodDates  | 1               | ACTIVITY           | 用缓存                                       |      |
| 6.19 | ActivitySummary                 | summaryActivity                                          | …                         | 1               |                    | 删除，用16                                   |      |

| S/N  | ui name                          | graphql schema                  | graphql   field                                       | number of times | parameters            |               |      |
| ---- | -------------------------------- | ------------------------------- | ----------------------------------------------------- | --------------- | --------------------- | ------------- | ---- |
| /    | **Portfolio Details(16)**        |                                 |                                                       |                 |                       |               |      |
| 7.1  | getPortfolioDetailInfo           | portfolios                      | name                                                  | 1               |                       |               | 5    |
| 7.2  | getPortfolioDetailLastLoadedDate | asOfDates                       | etlStartTime                                          | 1               | PORTFOLIO             | 用缓存        |      |
| 7.3  | getGroupByOptions                | groupByOptions                  | L1/l2/l3/l4/name                                      | 1               |                       |               | 6    |
| 7.4  | GetTMV                           | portfolios->summaryActivity     | endingMarketValue                                     | 1               | MTD                   | 可按需        | 7    |
| 7.5  | getRGL                           | portfolios->summaryActivity     | realizedGL                                            | 1               | YTD                   | 可按需        |      |
| 7.6  | getPropertyMetadata              | propertyMetadata                | code/name                                             | 1               | INVESTMENT            | 用缓存        |      |
| 7.7  | GetTotalCash                     | portfolios->summaryActivity     | cashMarketValue /          endingMarketValue          | 1               | MTD                   | 可按需        |      |
| 7.8  | getURGL                          | portfolios->summaryActivity     | unrealizedGL                                          | 1               | ITD                   | 可按需        |      |
| 7.9  | GetAttributes                    | portfolios                      | openDate      closingMethod  /status        taxStatus | 1               |                       | 与1合并       |      |
| 7.10 | GetPerformanceGraph              | portfolios->performance         | …                                                     | 1               |                       |               | 3    |
| 7.11 | getActivityDatesQuery            | asOfDates                       | etlStartTime/periodDates                              | 1               | ACTIVITY              | 用缓存        |      |
| 7.12 | getPortfolioExposurePositions    | asOfDates/portfolios->positions | periodDates/…                                         | 1               | EXPOSURE/filter+group | 删除asOfDates | 4    |
| 7.13 | getExposureHeader                | portfolios->topBottomHoldings   | top/bottom                                            | 1               | Limit 3               | limit=8       | 1    |
| 7.14 | getExposureComparison            | portfolios->topBottomHoldings   | top                                                   | 1               | Limit 8               | 删除，用13    |      |
| 7.15 | ActivitySummaryPortDetails       | portfolios->summaryActivity     | …                                                     | 1               | YTD                   |               | 2    |
| 7.16 | getExposureHeader                | portfolios->topBottomHoldings   | top/bottom                                            | 1               | Limit 3               | 删除用13      |      |