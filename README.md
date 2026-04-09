# Reproducing Ditto: An Elastic and Adaptive Memory-Disaggregated Caching System

This repository contains the reproduction of **Ditto** (SOSP 2023), a caching system that separates memory storage from compute using RDMA. Instead of keeping the cache on the same machine as the application, Ditto stores cached data on dedicated memory nodes and accesses it directly over the network using RDMA hardware - completely bypassing the memory node's CPU. This allows Ditto to serve millions of requests per second with very low latency, while competing systems quickly hit a performance wall as client load increases.

The reproduction was carried out on a **10-node CloudLab cluster** (r650 nodes with Mellanox ConnectX-6 100 Gbps NICs), following the setup instructions and scripts from the original Ditto artifact repository exactly.

---

## What We Reproduced

We reproduced the following aspects of the original paper:

- **Motivation experiment** - showing how conventional disaggregated caches collapse under concurrent load due to CPU bottlenecks in the eviction path
- **Throughput scalability** - comparing Ditto against CM-LRU, CM-LFU, and Redis as the number of CPU cores increases
- **Throughput-latency curves** - across YCSB-A, B, C workloads and against Shard-LRU
- **Adaptive eviction policy** - evaluating hit rates under mixed workloads, varying application mixes, and time-varying workloads using Twitter production traces
- **Elastic scaling** — comparing how Ditto and Redis handle adding CPU and memory resources at runtime

---

## Results

All **17 figures** from our reproduction can be found in:

```
results/figures/
```

The raw data used to generate those figures is available in:

```
results/result_data/
```

---

## How to Run

Please refer to the original [Ditto repository](https://github.com/dmemsys/Ditto) for the full setup guide. In short, you will need:

- A CloudLab account with access to r650 nodes
- Ubuntu 18.04 with Mellanox OFED 4.9-5.1.0.0
- The dependencies installed via `scripts/setup-env.sh`

Once the cluster is set up, the kick-the-tires script is a good starting point:

```bash
cd experiments/scripts
python kick-the-tires.py 256 ycsbc
```

For the full experiment suite, see `experiments/scripts/README.md` in the Ditto repository.

---

## Reference

Jiacheng Shen, Pengfei Zuo, Xuchuan Luo, Tianyi Yang, Yuxin Su, Yangfan Zhou, and Michael R. Lyu.
*Ditto: An Elastic and Adaptive Memory-Disaggregated Caching System.*
SOSP 2023.