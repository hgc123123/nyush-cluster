# Introducation

The OSU micro-benchmarks feature a series of MPI benchmarks that measure the performances of various MPI operations:

* Point-to-Point MPI Benchmarks: Latency, multi-threaded latency, multi-pair latency, multiple bandwidth / message rate test bandwidth, bidirectional bandwidth
Collective MPI Benchmarks: Collective latency tests for various MPI collective operations such as MPI_Allgather, MPI_Alltoall, MPI_Allreduce, MPI_Barrier, MPI_Bcast, MPI_Gather, MPI_Reduce, MPI_Reduce_Scatter, MPI_Scatter and vector collectives.
* One-sided MPI Benchmarks: one-sided put latency (active/passive), one-sided put bandwidth (active/passive), one-sided put bidirectional bandwidth, one-sided get latency (active/passive), one-sided get bandwidth (active/passive), one-sided accumulate latency (active/passive), compare and swap latency (passive), and fetch and operate (passive) for MVAPICH2 (MPI-2 and MPI-3).

## Version
| Cluster | Version   | Module                                       |
|:--------|:---------:|:---------------------------------------------|
| nyushc  | 7.1.1     | osu-micro-benchmarks/7.1-1-oneapi-2023.1.0   |


## 1.Bidirectional Bandwidth

**osu_bibw.slurm**
```
#!/bin/bash                                              
#SBATCH --job-name=osu_argon[23-24]
#SBATCH --partition=argon
#SBATCH -n 2                              # There are two processes
#SBATCH --ntasks-per-node=1               # one process per node
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --exclusive
#SBATCH --nodelist=argon[23,24]

module load osu-micro-benchmarks/7.1-1-oneapi-2023.1.0
ulimit -l unlimited
mpirun osu_bibw
```

```
sbatch osu_bibw.slurm
```

**Result**
```
# OSU MPI Bi-Directional Bandwidth Test v7.1
# Size      Bandwidth (MB/s)
# Datatype: MPI_CHAR.
1                       6.10
2                      12.01
4                      24.21
8                      48.42
16                     96.09
32                    181.81
64                    345.81
128                   668.20
256                  1132.48
512                  2139.29
1024                 4087.70
2048                 7168.90
4096                13482.60
8192                17868.14
16384               19940.29
32768               29086.70
65536               32105.18
131072              34041.75
262144              18963.82
524288              20195.27
1048576             22703.34
2097152             28583.81
4194304             18822.70
```

## 2.Latency

**osu_latency.slurm**
```
#!/bin/bash
#SBATCH --job-name=osu_argon[23-24]
#SBATCH --partition=argon
#SBATCH -n 2                              # There are two processes
#SBATCH --ntasks-per-node=1               # one process per node
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH --exclusive
#SBATCH --nodelist=argon[23,24]

module load osu-micro-benchmarks/7.1-1-oneapi-2023.1.0
ulimit -l unlimited
mpirun osu_latency
```

```
sbatch osu_latency.slurm
```

**Result**
```
# OSU MPI Latency Test v7.1
# Size          Latency (us)
# Datatype: MPI_CHAR.
1                       3.16
2                       3.14
4                       3.14
8                       3.14
16                      3.14
32                      3.20
64                      3.34
128                     3.40
256                     3.80
512                     3.84
1024                    3.97
2048                    4.50
4096                    4.81
8192                    5.73
16384                   7.30
32768                   8.84
65536                  10.74
131072                 16.47
262144                 24.43
524288                 38.77
1048576                68.24
2097152               125.95
4194304               241.71
```
