# K/V with LSM tree's Papers
记录正在读或者已经读过的Key Value键值存储系统的文章


## Why K/V?
With the exponential growth of data volume,traditional relational database meets challenges in scalability in dealing with extremely large-scale data.

As an alternative, key-value(KV) store is widely used as the fundamental storage infrastructure in many applications.

## Why LSM-tree?
According to the used index structures, KV stores can be categorized into **hash index** based design,**B-tree** based design and **LSM-tree** based design.
Because hash index based design requires large memory and can not well support range query,and B-tree based design involves an abundance of random writes, so most modern KV stores use LSM-tree, e.g.,LevelDB(Google),RocksDB(Facebook),Dynamo(Amazon),Cassandra(Apache).

## What's LSM-tree?
Log-Structured Merge-Tree

An LSM-tree based KV store is typically composed of two components.
One resides in memory to cache KV pairs, and it includes a MemTable and an Immutable MemTable. The other is stored insecondary storage, which is divide into multiple levels consisting of multiple SSTables.

Each SSTable contains a set of sorted KV pairs and necessary metadata.When a level reachese its size limit its SSTables will be compacted into the next levle via compaction, which first reads out the SSTables in the two levels, then performs a merge sort, and finally writes back the new SSTables into the next level.
**(This is the reason that compaction induces severe write amplification.)**

When we lookup a KV pair,KV store needs to check multiple SSTables from the lowest level to the highest level until the key is found or all level have been checked.**(This ie the reason that LSM-tree based KV store suffer from read amplication)**

Furthermore, it is required to read multiple metadata blocks to really check whether a KV pair exists in one SSTable.



