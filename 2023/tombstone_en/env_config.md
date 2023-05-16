1. dev 201

 (1) Config

- cassandra  5 pods,  every pod cup 8, memory 20G

- graphql  cup 4, memeory 20G

- trino  4 workers， coordinator cup 2, memory 4G，every worker cup 2， memory 4G


 (2) Namespace: graphql-trino-refactor

 (3) graphql
			non tombstone http://10.2.96.132:31426/graphql  (db schema: feature-aoc-30265-esg)

​    tombstone http://10.2.96.132:32295/ (db schema: feature-aoc-32599-tombstone)

 (4) table size

| Table                             | feature-aoc-30265-esg | feature_aoc_32599_tombstone |
| --------------------------------- | --------------------- | --------------------------- |
| portfolio                         | 12628                 | 12628                       |
| activity                          | 567                   | 532                         |
| exposure_test                     | 16781                 | 15356                       |
| portfolio_property_by_tenant      | 5                     | 5                           |
| portfolio_property_by_portfolio   | 122908                | 122893                      |
| performance_analysis              | 80                    | 75                          |
| portfolio_by_group                | 14547                 | 14517                       |
| exposure_esg_pivot                | 13310                 | 7631                        |
| investment_esg_metadata           | 14                    | 14                          |
| investment_property_by_investment | 74257                 | 74244                       |
| portfolio_group_ui                | 30                    | 0                           |

(5) use portfolio group: ds_nestedgrp



2. Ssnc JIC

(1) Config

- cassandra  6 pods,  every pod cup 8, memory 20G
- graphql  cup 4, memeory 20G
- trino  4 workers， coordinator cup 2, memory 4G，every worker cup 2， memory 4G

(2) Namespace: graphql-trino-refactor(trino), feature_aoc_32599_tombstone,  feature_aoc_31900_performance

(3) graphql
			non tombstone https://feature-aoc-31900-performance-graphql.ssnc-corp.cloud/graphql  (db schema: feature_aoc_31900_performance)

​    tombstone https://feature-aoc-32599-tombstone-graphql.ssnc-corp.cloud/graphql   (db schema: feature_aoc_32599_tombstone)

(4) table size

| Table                             | feature_aoc_31900_performance | feature_aoc_32599_tombstone |
| --------------------------------- | ----------------------------- | --------------------------- |
| portfolio                         | 23130                         | 23130                       |
| activity                          | 401961                        | 267974                      |
| exposure_test                     | 15214307                      | 14577835                    |
| portfolio_property_by_tenant      | 5                             | 5                           |
| portfolio_property_by_portfolio   | 124300                        | 124300                      |
| performance_analysis              | 57270                         | 38180                       |
| portfolio_by_group                | 807984                        | 807984                      |
| exposure_esg_pivot                | 11457865                      | 11715066                    |
| investment_esg_metadata           | 14                            | 14                          |
| investment_property_by_investment | 156546                        | 173940                      |
| portfolio_group_ui                | 2                             | 0                           |

(5) use portfolio group:  Group_72619   - 20064 /  Group_83901 - 3





