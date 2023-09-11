# Wisckey
WiscKey: Separating Keys from Values in SSD-conscious Storage

wisckey是一种基于LSM树的持久键值存储，基于LevelDB，论文的核心思路是将Key和Value分开以减少I/O的放大，同时充分利用了SSD 的顺序和随机访问特征。

LSM树比起其他index structures（比如B树）的优点是LSM树维护写入的顺序访问模式，B树上的small updates可能涉及到很多随机写入，因此在传统的HDD或者现有的SSD上效率都不是很高。

基于 LSM 的技术的成功与其在经典硬盘驱动器 (HDD) 上的使用密切相关。在 HDD 中，随机 I/O 比顺序 I/O 慢 100 倍以上 [43]；因此，执行额外的顺序读取和写入以连续对键进行排序并启用高效查找代表了一个很好的权衡。

然而，存储格局正在迅速变化，现代固态存储设备 (SSD) 在许多重要用例中正在取代 HDD。与HDD相比，SSD在性能和可靠性特性上有着根本的不同；在考虑键值存储系统设计时，我们认为以下三个差异至关重要。

1. SSD上的随机性能和顺序性能的差异并不像HDD那么大，因此执行大量顺序I/O以减少后续随机I/O的LSM树可能会不必要地浪费带宽。

2. SSD的内部并行度很高，SSD上的LSM必须精心设计才能利用这种并行性。

3. SSD可能会因为重复写入而导致磨损，而LSM树中的写放大会显著缩短SSD的寿命。


分离KV也引起了一些问题

1. Value不再有序，因此scan(range query)的性能可能会被影响

2. 分离KV需要做garbage collect，回收那些invalid values占的空间

3. KV的分离导致了crash consistency 的挑战，WiscKey利用文件系统的特性appends never result in garbage data on a crash，提供与现代LSMtree相同的一致性策略。


