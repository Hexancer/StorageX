# Diffkv
#### Differentiated Key-Value Storage Management for Balanced I/O Performance

*version-0.1*

> DiffKV, a novel LSM-tree KV store that aims for balanced performance in writes, reads, and scans.

##### Intro

Key-value storage three main operations

- writes, insert KV pairs

- reads, retrieve the value of a single key

- scans, retrieve the values over a key range

Efficiency of sequential I/Os && Data ordering for fast scans ---> ***Log-Structured-Merge-tree***

##### Motivation


