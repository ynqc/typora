query 1 = Query.from(Table).select(Column).filter().filterGroup().orderby().limit().offset.groupby().orderby().aggregation().filter().JOIN()
query 2 = Query.from(query1).select(Column).filter().filterGroup()

（1）TODO需要验证

（2）sql A & ((B | C ) & D | (E & F))

（3）to_json() //log



| Class           | Method         | Use                                                          | Type                            |
| --------------- | -------------- | ------------------------------------------------------------ | ------------------------------- |
| Query           | .tables        | [Table, Table]                                               | List                            |
|                 | from           | Query.from('activity')                                       | Tables/QUERY                    |
|                 | select         | .select('id', 'tenant_id', 'portfolio_code'). // 现在可以不要 | COLUMN                          |
|                 |                | activity_table = Table('activity', ‘’a’’)<br /><br />column(“tenanted_id ”, ‘tenantId’)<br />br />activity_table_2 = Table('activity', ‘’a’’)<br />q = Query.from_(activity_table).select(activity_table.tenant_id, activity_table_2.portfolio_code) |                                 |
|                 |                |                                                              |                                 |
|                 | filter // 不要 | .filter(column=activity_table.tenant_id, value= 201, operator= >).filter().fiter() | Column>= 201                    |
|                 |                | .filter(Column(‘tenant_id’) > 201).filter().fiter()          |                                 |
|                 |                | .filter(‘tenant_id’ == 201).filter().fiter()                 |                                 |
|                 |                | Filter(A).and.filter()                                       |                                 |
|                 | filter         | filterItem -> (column=activity_table.tenant_id, value= 201, operator= >, andOr= ‘and’).filter().filter() | {Filter -> and/or + filterItem} |
|                 |                | .filter({}, {})                                              |                                 |
|                 | filterItem     | // TODO ()                                                   |                                 |
|                 |                |                                                              |                                 |
| orderby         | orderby        | .orderby([orderby,orderby])                                  | Column                          |
|                 |                | .orderby(“as_of_date”, order=Order.desc).orderby()           |                                 |
|                 |                |                                                              |                                 |
|                 |                |                                                              |                                 |
|                 | groupby        | .groupby(““as_of_date”)                                      | Column                          |
|                 |                | .groupby(““as_of_date”).aggregation([aggregation, aggregation]) | Column                          |
| aggregation     |                | column=”“as_of_date”, method=“sum” // 需要验证groupby        |                                 |
| FilterItem 参考 | Join           | .join(join_table, type=  JoinType.left).                     | TABLE/  QUERY                   |
|                 |                | .join.on(left.portfolio_code, right.portfolio_code).on(left.te, right.te).on() | Column=Column                   |
|                 | Limit          | limit = 0                                                    | Number                          |
|                 | offset         | offset = 1                                                   | Number                          |
| // TODO         | .Unions        | Query                                                        |                                 |
|                 |                |                                                              |                                 |
|                 | .povit         | Case/  when/ then                                            |                                 |
| Table           |                | Table('activity', “A”)                                       | TABLE/ Table.COLUMN             |
|                 | .columns       |                                                              |                                 |
| Column          |                | Column('portfolio_code',   ‘portfolioCode’)                  |                                 |
|                 |                | [Column, Column]                                             | List                            |
|                 |                | Column('portfolio_code')                                     |                                 |
|                 |                | Column('portfolio_code', type=“time/date/bool”)              |                                 |
|                 |                | Column(ColumnA- ColumnB, ‘C’)                                |                                 |
| Query Function  |                |                                                              |                                 |
|                 | get_sql        |                                                              |                                 |
|                 | get_columns    |                                                              |                                 |
|                 | to_json(TODO)  | // log                                                       | Dict                            |



