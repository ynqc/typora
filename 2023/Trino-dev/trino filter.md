|          | EQUAL | NOT_EQUAL | GREATER_THAN | LESS_THAN | LESS_THAN_OR_EQUAL | GREATER_THAN_OR_EQUAL | CONTAIN | IS_NULL |
| -------- | ----- | :-------: | :----------: | --------- | ------------------ | --------------------- | ------- | ------- |
| FLOAT    | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ×       | ✓       |
| TEXT     | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ✓       | ✓       |
| DATETIME | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ==×==   | ✓       |
| BOOLEAN  | ✓     |     ✓     |    ==×==     | ==×==     | ==×==              | ==×==                 | ==×==   | ✓       |

graphql  原来各类型数据支持filter operator 情况

|          | EQUAL | NOT_EQUAL | GREATER_THAN | LESS_THAN | LESS_THAN_OR_EQUAL | GREATER_THAN_OR_EQUAL | CONTAIN | IS_NULL |
| -------- | ----- | :-------: | :----------: | --------- | ------------------ | --------------------- | ------- | ------- |
| FLOAT    | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ×       | ✓       |
| TEXT     | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ✓       | ✓       |
| DATETIME | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ✓       | ✓       |
| BOOLEAN  | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ✓       | ✓       |

graphql  trino refactor 后各类型数据支持filter operator 情况

|          | EQUAL | NOT_EQUAL | GREATER_THAN | LESS_THAN | LESS_THAN_OR_EQUAL | GREATER_THAN_OR_EQUAL | CONTAIN | IS_NULL |
| -------- | ----- | :-------: | :----------: | --------- | ------------------ | --------------------- | ------- | ------- |
| FLOAT    | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ×       | ✓       |
| TEXT     | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ✓       | ✓       |
| DATETIME | ✓     |     ✓     |      ✓       | ✓         | ✓                  | ✓                     | ==×==   | ✓       |
| BOOLEAN  | ✓     |     ✓     |    ==×==     | ==×==     | ==×==              | ==×==                 | ==×==   | ✓       |

