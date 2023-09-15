其他相关主题的待读论文

ASPLOS 2020

##### Hailstorm: Disaggregated Compute and Storage for Distributed LSM-based Databases

论文doi: https://doi.org/10.1145/3373376.3378504

**Abstract**: Distributed LSM-based databases face throughput and latency issues due to load imbalance across instances and interference from background tasks such as flushing, compaction, and data migration. Hailstorm addresses these problems by deploying the database storage engines over a distributed filesystem that disaggregates storage from processing, enabling storage pooling and compaction offloading. Hailstorm pools storage devices within a rack, allowing each storage engine to fully utilize the aggregate rack storage capacity and bandwidth. Storage pooling successfully handles load imbalance without the need for resharding. Hailstorm offloads compaction tasks to remote nodes, distributing their impact, and improving overall system throughput and response time. We show that Hailstorm achieves load balance in many MongoDB deployments with skewed workloads, improving the average throughput by 60%, while decreasing tail latency by as much as 5×. In workloads with range queries, Hailstorm provides up to 22× throughput improvements. Hailstorm also enables cost savings of 47-56% in OLTP workloads.

----

ASPLOS 2023

##### Revisiting Log-Structured Merging for KV Stores in Hybrid Memory Systems

[项目地址](https://github.com/CGCL-codes/mioDB/blob/master/README.md)

论文doi： https://doi.org/10.1145/3575693.3575715

**Abstract**: We present MioDB, a novel LSM-tree based key-value (KV) store system designed to **fully exploit the advantages of byte-addressable non-volatile memories (NVMs)**. 

Our experimental studies reveal that the performance bottleneck of LSM-tree based KV stores using NVMs mainly stems from 
(1) costly data serialization/deserialization across memory and storage, and 

(2) unbalanced speed between memory-to-disk data flushing and on-disk data compaction. They may cause unpredictable performance degradation due to write stalls and write amplification. To address these problems, we advocate byte-addressable and persistent skip lists to replace the on-disk data structure of LSM-tree, and design four novel techniques to make the best use of fast NVMs. First, we propose one-piece flushing to minimize the cost of data serialization from DRAM to NVM. Second, we exploit an elastic NVM buffer with multiple levels and zero-copy compaction to eliminate write stalls and reduce write amplification. Third, we propose parallel compaction to orchestrate data flushing and compactions across all levels of LSM-trees. Finally, MioDB increases the depth of LSM-tree and exploits bloom filters to improve the read performance. Our extensive experimental studies demonstrate that MioDB achieves 17.1× and 21.7× lower 99.9th percentile latency, 8.3× and 2.5× higher random write throughput, and up to 5× and 4.9× lower write amplification compared with the state-of-the-art NoveLSM and MatrixKV, respectively.

---

EuroSys 2022

##### p<sup>2</sup>KVS: a portable 2-dimensional parallelizing framework to improve scalability of key-value stores on SSDs

论文doi: https://doi.org/10.1145/3492321.3519567

**Abstract**: Attempts to improve the performance of key-value stores (KVS) by replacing the slow Hard Disk Drives (HDDs) with much faster Solid-State Drives (SSDs) have consistently fallen short of the performance gains implied by the large speed gap between SSDs and HDDs, especially for small KV items. We experimentally and holistically explore the root causes of performance inefficiency of existing LSM-tree based KVSs running on powerful modern hardware with multicore processors and fast SSDs. Our findings reveal that the global write-ahead-logging (WAL) and index-updating (MemTable) can become bottlenecks that are as fundamental and severe as the commonly known LSM-tree compaction bottleneck, under both the single-threaded and multi-threaded execution environments.

To fully exploit the performance potentials of full-fledged KVS and the underlying high-performance hardware, we propose a portable 2-dimensional KVS parallelizing framework, referred to as p2KVS. In the horizontal inter-KVS-instance dimension, p2KVS partitions a global KV space into a set of independent subspaces, each of which is maintained by an LSM-tree instance and a dedicated worker thread pinned to a dedicated core, thus eliminating structural competition on shared data structures. In the vertical intra-KVS-instancedimension, p2KVS separates user threads from KVS-workers and presents a runtime queue-based opportunistic batch mechanism on each worker, thus boosting process efficiency. Since p2KVS is designed and implemented as a user-space request scheduler, viewing WAL, MemTables, and LSM-trees as black boxes, it is nonintrusive and highly portable. Under micro and macro-benchmarks, p2KVS is shown to gain up to 4.6× write and 5.4× read speedups over the state-of-the-art RocksDB.

##### Tebis: index shipping for efficient replication in LSM key-value stores

论文doi: https://doi.org/10.1145/3492321.3519572

**Abstract**: Key-value (KV) stores based on LSM tree have become a foundational layer in the storage stack of datacenters and cloud services. Current approaches for achieving reliability and availability favor reducing network traffic and send to replicas only new KV pairs. As a result, they perform costly compactions to reorganize data in both the primary and backup nodes, which increases device I/O traffic and CPU overhead, and eventually hurts overall system performance. In this paper we describe *Tebis*, an efficient LSM-based KV store that reduces I/O amplification and CPU overhead for maintaining the replica index. We use a primary-backup replication scheme that performs compactions only on the primary nodes and sends pre-built indexes to backup nodes, avoiding all compactions in backup nodes. Our approach includes an efficient mechanism to deal with pointer translation across nodes in the pre-built region index. Our results show that *Tebis* reduces pressure on backup nodes compared to performing full compactions: Throughput is increased by 1.1 -- 1.48×, CPU efficiency is increased by 1.06 -- 1.54×, and I/O amplification is reduced by 1.13 -- 1.81×, without increasing server to server network traffic excessively (by up to 1.09 -- 1.82×).

----

EuroSys 2023

##### All-Flash Array Key-Value Cache for Large Objects

论文doi: https://doi.org/10.1145/3552326.3567509

**Abstract**: We present BigKV, a key-value cache specifically designed for caching large objects in an all-flash array (AFA). The design of BigKV is centered around the unique property of a cache: since it contains a copy of the data, exact bookkeeping of what is in the cache is not critical for correctness. By ignoring hash collisions, approximating metadata information, and allowing data loss from failures, BigKV significantly increases the cache hit ratio and keeps more useful objects in the system. Experiments on a real AFA show that our design increases the throughput by 3.1× on average and reduces the average and tail latency by 57% and 81%, respectively.

##### FlowKV: A Semantic-Aware Store for Large-Scale State Management of Stream Processing Engines

论文doi: https://doi.org/10.1145/3552326.3567493

**Abstract**:  We propose FlowKV, a persistent store tailored for large-scale state management of streaming applications. Unlike existing KV stores, FlowKV leverages information from stream processing engines by taking a principled approach toward exploiting information about *how* and *when* the applications access data. FlowKV categorizes data access patterns of window operations according to how window boundaries are set and how tuples inside a window are aggregated, and deploys customized in-memory and on-disk data structures optimized for each pattern. In addition, FlowKV takes window metadata as explicit arguments of read and write methods to predict the moment when a window is read, and then loads the tuples of windows in batches from storage ahead of time. Using the NEXMark benchmark as workload, our experiments show that Apache Flink on FlowKV outperforms Flink on RocksDB or Faster with up to 4.12× throughput gain.

---

FAST2022

##### Closing the B+-tree vs. LSM-tree Write Amplification Gap on Modern Storage Hardware with Built-in Transparent Compression

**Abstract**: This paper studies how B+-tree could take full advantage of modern storage hardware with built-in transparent compression. Recent years witnessed significant interest in applying log-structured merge tree (LSM-tree) as an alternative to B+-tree, driven by the widely accepted belief that LSM-tree has distinct advantages in terms of storage cost and write amplification. This paper aims to revisit this belief upon the arrival of storage hardware with built-in transparent compression. Advanced storage appliances and emerging computational storage drives perform hardware-based lossless data compression, transparent to OS and user applications. Beyond straightforwardly reducing the storage cost gap between B+-tree and LSM-tree, such storage hardware creates new opportunities to re-think the implementation of B+-tree. This paper presents three simple design techniques that can leverage such modern storage hardware to significantly reduce the B+-tree write amplification. Experiments on a commercial storage drive with built-in transparent compression show that the proposed design techniques can reduce the B+-tree write amplification by over 10× . Compared with RocksDB (a key-value store built upon LSM-tree), the enhanced B+-tree implementation can achieve similar or even smaller write amplification.



**Removing Double-Logging with Passive Data Persistence in LSM-tree based Relational Databases**



**Abstract**: Storage engine is a crucial component in relational databases (RDBs). With the emergence of Internet services and applications, a recent technical trend is to deploy a Log-structured Merge Tree (LSM-tree) based storage engine. Although such an approach can achieve high performance and efficient storage space usage, it also brings a critical double-logging problem——In LSM-tree based RDBs, both the upper RDB layer and the lower storage engine layer implement redundant logging facilities, which perform synchronous and costly I/Os for data persistence. Unfortunately, such “double protection” does not provide extra benefits but only incurs heavy and unnecessary performance overhead.

In this paper, we propose a novel solution, called Passive Data Persistence Scheme (PASV), to address the double-logging problem in LSM-tree based RDBs. By completely removing Write-ahead Log (WAL) in the storage engine layer, we develop a set of mechanisms, including a passive memory buffer flushing policy, an epoch-based data persistence scheme, and an optimized partial data recovery process, to achieve reliable and low-cost data persistence during normal runs and also fast and efficient recovery upon system failures. We implement a fully functional, open-sourced prototype of PASV based on Facebook’s MyRocks. Evaluation results show that our solution can effectively improve system performance by increasing throughput by up to 49.9% and reducing latency by up to 89.3%, and it also saves disk I/Os by up to 42.9% and reduces recovery time by up to 4.8%.
