

Summary

（1） The throughput of none tombstone is more stable, while tombstone is relatively unstable (throughput: tombstone -6-2/s, none tombstone -2-3/s)

（2）The more concurrent the throughput, the average response time tombstone<none tombstone（39.71% - 6.81%）

（3）Different requests result in more complex SQL, with none tombstone throughput > tombstone (JIC data: none tombstone -0.81-60/min, tombstone -0.77-37/min)



Comparison between tombstone and none tombstone summary

（1） The throughput of none tombstone is more stable, while tombstone is relatively unstable (throughput: tombstone -6-2/s, none tombstone -2-3/s)

（2）The more concurrent the throughput, the average response time tombstone  > none tombstone（39.71% - 6.81%）

（3）Different requests result in more complex SQL, with none tombstone throughput > tombstone (JIC data: none tombstone -0.81-60/min, tombstone -0.77-37/min)
