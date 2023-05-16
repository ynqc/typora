|      | table                                  | partition key                                                | clustering key                                | Use  |
| ---- | -------------------------------------- | ------------------------------------------------------------ | --------------------------------------------- | ---- |
| 1    | activity                               | tenant_id, period_range_type                                 | portfolio_code, as_of_date                    | Y    |
| 2    | activity_group_by_property             | tenant_id, portfolio_group_code, period_range_type, portfolio_property_name | as_of_date                                    | N    |
| 3    | etl_audit                              | tenant_id                                                    | ui_feature                                    | Y    |
| 4    | exposure_esg_grouped_pivot             | tenant_id, as_of_date                                        | portfolio_code                                | Y    |
| 5    | exposure_esg_pivot                     | tenant_id, as_of_date                                        | portfolio_code, investment_id                 | Y    |
| 6    | exposure_grouped                       | tenant_id, as_of_date, portfolio_code                        | /                                             | N    |
| 7    | exposure_test                          | tenant_id, as_of_date                                        | portfolio_code, is_short_asset, investment_id | Y    |
| 8    | investment_esg_metadata                | esg_code                                                     | esg_name                                      | Y    |
| 9    | investment_property_by_investment      | tenant_id                                                    | investment_id, investment_property_name       | Y    |
| 10   | investment_property_by_tenant          | tenant_id                                                    | investment_property_name                      | Y    |
| 11   | investment_property_group_by_tenant    | tenant_id                                                    | name                                          | Y    |
| 12   | performance_analysis                   | tenant_id                                                    | portfolio_code, as_of_date                    | Y    |
| 13   | performance_analysis_group_by_property | tenant_id, portfolio_group_code, portfolio_property_name     | as_of_date                                    | N    |
| 14   | portfolio                              | tenant_id                                                    | portfolio_code                                | Y    |
| 15   | portfolio_by_code                      | tenant_id, portfolio_code                                    | /                                             | Y    |
| 16   | portfolio_by_group                     | tenant_id, portfolio_group_code                              | portfolio_code                                | Y    |
| 17   | portfolio_by_portfolio_property        | tenant_id, portfolio_group_code, portfolio_property_name     | portfolio_code                                | N    |
| 18   | portfolio_group                        | tenant_id                                                    | portfolio_group_code                          | N    |
| 19   | portfolio_group_by_code                | tenant_id, portfolio_group_code                              | /                                             | Y    |
| 20   | portfolio_group_by_type                | tenant_id, portfolio_group_type                              | portfolio_group_code                          | N    |
| 21   | portfolio_group_ui                     | tenant_id                                                    | portfolio_group_code                          | Y    |
| 22   | portfolio_property_by_portfolio        | tenant_id                                                    | portfolio_code, portfolio_property_name       | Y    |
| 23   | portfolio_property_by_tenant           | tenant_id                                                    | portfolio_property_name                       | Y    |
| 24   | standard_deviation                     | tenant_id, portfolio_group_code                              | portfolio_code, as_of_date                    | N    |

