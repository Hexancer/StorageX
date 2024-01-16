# ADOC
### Write-Stall现象
虽然基于LSM的KV系统可以提供更高的吞吐，但是当这些系统面临高写入压力的负载时，会频繁的出现write-stall现象。

写停顿现象有两个特征：
1. 从HDD到PM持久内存，写停顿发生在各种类型的设备上。
2. 写停顿是设备依赖的，写停顿的持续时间和性能下降率由设备类型和写入密度等因素决定。

论文提出了Automatic Data Overflow Control ADOC,通过协调LSM-KV系统各个组件之间的数据流动来减少写停顿。

先前的bLSM、SILK等在特定的配置上减小写停顿，分析难以一般化。还有一些研究将写停顿归于资源耗尽，但可以发现写停顿在硬件资源充足的情况下也会发生。这就表明写停顿不仅发生在被动阻塞，当KV系统尝试避免更进一步的性能损失时，也会主动停顿。比如LevelDB和RocksDB为避免Disk Overflow，使用的主动停顿策略，Disk Overflow是指当flush或者compaction的作业跟不上写入速率的情况。

写停顿的原因应是Disk Overflow 的泛化，称为data overflow，是指LSM-KV系统的一个或者多个组件因为流向某个组件的数据流激增而迅速膨胀。

论文把data overflow根据形成原因分成三种：
1.

