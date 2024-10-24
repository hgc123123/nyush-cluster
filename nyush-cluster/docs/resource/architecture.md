# Overview
**NYUSH HPC Cluster** consists of two clusters, which can be accessed via the campus network.

- nyushc
- pudong

**Total**

| Classes | Nodes | CPUs | GPUs  |
|:--------|:------|:-----|:-----:|
| `Total` | 134   | 6480 | 230   |

**Network**

| Cluster     | Pudong | NYUSHC  |
|:------------|:-------|:-------:|
| `Bandwidth` | 10GB/s | 100GB/s |

![](figures/hpc-architecture-topology.png){: style="width:100%;" .center}

## GPFS File System

The two clusters share the same parallel GPFS file system with 1.7PB, by DDN mounted at /gpfsnyu and /gpfsnyuc7 respectively.

![](figures/hpc-architecture.png){: style="width:100%;" .center}

## Network

The computing nodes in the nyushc cluster are connected by a `100 GB/s` RoCE network, and the bandwidth from the file system to the computing nodes is also `100 GB/s`.
In the pudong cluster, both bandwidths are `10GB/s` and `100GB/s`.

## Resources in the NYUSHC cluster

| Classes | Nodes | CPUs | GPUs  |
|:--------|:------|:-----|:-----:|
| `Total` | 52    | 3112 | 134   |

**Partitions**
```
$ sinfo --summarize
PARTITION AVAIL  TIMELIMIT   NODES(A/I/O/T) NODELIST
parallel     up 20-00:00:0        10/1/0/11 compute[134-144]
rooster      up   infinite          5/3/0/8 gpu[145-148,192-195]
hebb         up   infinite          1/0/0/1 gpu180
debug*       up 7-00:00:00        3/13/0/16 compute[118-133]
chem         up   infinite          2/0/0/2 gpu[181,185]
li           up   infinite          0/1/0/1 gpu182
aml          up   infinite          3/0/0/3 compute[183-184,189]
argon        up   infinite          3/0/0/3 argon[23-25]
netsys       up   infinite          0/1/0/1 gpu186
sfscai       up 2-00:00:00          3/3/0/6 gpu[187-188,190-191,196-197]
```

**CPUS**
```
# sinfo -o "%C"
CPUS(A/I/O/T)
1262/1842/8/3112
```

**GPUS**

```
# sinfo -o "%n %G" | grep gpu
gpu146 gpu:3090:8(S:0-1)
gpu147 gpu:3090:8(S:0-1)
gpu148 gpu:3090:8(S:0-1)
gpu145 gpu:3090:8(S:0-1)
gpu192 gpu:4090d:8(S:0-1)
gpu193 gpu:4090d:8(S:0-1)
gpu194 gpu:4090d:8(S:0-1)
gpu195 gpu:4090d:8(S:0-1)
gpu180 gpu:a30:3(S:0-1)
gpu185 gpu:4090:4
gpu181 gpu:3090:10(S:0-1)
gpu182 gpu:a800:1(S:0-1)
gpu186 gpu:4090:4(S:0)
gpu187 gpu:a800:8(S:0-1)
gpu188 gpu:a800:8(S:0-1)
gpu196 gpu:4090d:8(S:0-1)
gpu190 gpu:l20:8(S:0-1)
gpu191 gpu:h20:8(S:0-1)
gpu197 gpu:4090d:8(S:0-1)

# scontrol show nodes | grep "Gres=gpu" | awk -F'=' '{print $2}' | awk -F'(' '{print $1}' | awk -F':' '{sum += $NF} END {print sum}'
134
```

## Resources in the Pudong Cluster
| Classes | Nodes | CPUs | GPUs  |
|:--------|:------|:-----|:-----:|
| `Total` | 82    | 3368 | 96    |

**Partitions**
```
$ sinfo --summarize
PARTITION   AVAIL  TIMELIMIT   NODES(A/I/O/T) NODELIST
chen           up   infinite          0/5/0/5 chen[1-5]
argon          up   infinite       11/12/0/23 argon[1-22],argpu1
parallel       up 20-00:00:0       14/15/0/29 compute[24-52]
gpu            up 90-00:00:0          0/1/0/1 gpu7
hebb           up   infinite          0/1/0/1 gpu6
chem           up   infinite          0/2/0/2 gpu[8-9]
sci            up   infinite          2/0/0/2 gpu[10-11]
felix          up   infinite          0/3/0/3 felix[1-3]
aquila         up 7-00:00:00        1/14/0/15 agpu[1-9],aquila[1-6]
hetao          up   infinite          0/1/0/1 hetao1
```

**CPUS**
```
# sinfo -o "%C"
CPUS(Alloc/Idle/Other/Total)
804/2504/60/3368
```

**GPUS**

```
# sinfo -o "%n %G" | grep gpu
argpu1 gpu:3080Ti:10(S:0-1)
gpu7 gpu:2080Ti:8(S:0-1)
gpu6 gpu:P100:2(S:0-1)
gpu8 gpu:3080:4(S:0-1)
gpu9 gpu:3080:4(S:0-1)
gpu10 gpu:3080Ti:8(S:0-1)
gpu11 gpu:3080Ti:8(S:0-1)
agpu7 gpu:3090:8(S:0-1)
agpu1 gpu:1080Ti:2(S:0)
agpu2 gpu:1080Ti:4(S:0-1)
agpu3 gpu:1080Ti:4(S:0-1)
agpu4 gpu:2080Ti:8(S:0-1)
agpu5 gpu:2080Ti:8(S:0-1)
agpu6 gpu:2080Ti:8(S:0-1)
agpu8 gpu:a10:3(S:0-1)
agpu9 gpu:3080ti:6

# scontrol show nodes | grep "Gres=gpu" | awk -F'=' '{print $2}' | awk -F'(' '{print $1}' | awk -F':' '{sum += $NF} END {print sum}'
96
```

