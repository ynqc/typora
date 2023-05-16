结论

（1） none tombstone 吞吐量 更稳定，tombstone相对不稳定(tombstone - 6-2/s, none tombstone - 2-3/s)

（2）并发 越多 吞吐量，平均响应时间 tombstone  > none tombstone, （39.71% - 6.81%）

  (3) 不同请求，sql 越复杂， none tombstone  吞吐量 > tombstone(  none tombstone-0.81-60/min, tombstone- 0.77-37/min))



