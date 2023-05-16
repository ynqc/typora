以上都是在没有cache的时候得到的结果

201 数据上

1 并发相同个数请求（168） 201 table

​          a. none tombstone 更稳定，tombstone不稳定，但是开始快，后来慢

​          b. none tombstone 平均吞吐量是3.45，  tombstone 平均吞吐量3.23， 比 none tombstone 慢6.8% （none tombstone - tombstone）/ tombstone

2 并发相同时间60s

​		a. tombstone 比 none tomestone 并发请求个数少，少了91.79%

​        b. tombstone 比 none tomestone 吞吐量少，少了77.27%

​       c. tombstone 比 none tomestone 平均请求时间慢，慢了了76.27%

3 单发
		       a.  tombstone 平均响应时间 < non tombstone, 快了42.09%



JIC 数据上

1 并发相同个数请求

 a.  tombstone 平均响应时间 < non tombstone, 慢了26.57%

2 并发相同时间60s

（1）并发用户越多，tombstone 与 none tomestone 相比性能越差 

5 user

​		a. tombstone 比 none tomestone 并发请求个数少，少了50%

​        b. tombstone 比 none tomestone 吞吐量少，少了49.47%

​       c. tombstone 比 none tomestone 平均请求时间慢，慢了了33.16%

10 user

​		a. tombstone 比 none tomestone 并发请求个数少，少了50%

​        b. tombstone 比 none tomestone 吞吐量少，少了48.81%

​       c. tombstone 比 none tomestone 平均请求时间慢，慢了了32.84%

20 user

​		a. tombstone 比 none tomestone 并发请求个数少，少了100%

​        b. tombstone 比 none tomestone 吞吐量少，少了60.86%

​       c. tombstone 比 none tomestone 平均请求时间慢，慢了了39.60%

3 单发一个filter请求

  a.  tombstone 平均响应时间 < non tombstone（39.71% - 6.81%）

  b. 但是portfolio group的不同，portfolio 个数不同，导致不同请求tombstone慢的程度不同





