# Introducation

HPL is a software package that solves a (random) dense linear system in double precision (64 bits) arithmetic on distributed-memory computers. 
It can thus be regarded as a portable as well as freely available implementation of the High Performance Computing Linpack Benchmark.

## Version
| Cluster | Version   | Module          |
|:--------|:---------:|:----------------|
| nyushc  | 2022.0.1  | oneapi/2022.0.1 |

## Test Data
```
mkdir ~/HPL && cd ~/HPL
module load oneapi
cp -r $MKLROOT/benchmarks/mp_linpack ./
cd mp_linpack/
```

## Content of mp_linpack
```
[hpc@hpclogin test]# tree mp_linpack/
mp_linpack/
├── build.sh
├── COPYRIGHT
├── HPL.dat
├── HPL_main.c
├── libhpl_intel64.a
├── readme.txt
├── runme_intel64_dynamic
├── runme_intel64_prv
└── xhpl_intel64_dynamic

0 directories, 9 files
```

## Parameters

### Parameters in HPL.dat

**Ns**
```
[0.85,0.9]*sqrt(mem*1024*1024*1024*nodes/8)
```

**P & Q***
```
P*Q=Total procs
P<=Q
```

### Parameters in runme_intel64_dynamic
```
# Set total number of MPI processes for the HPL (should be equal to PxQ).
export MPI_PROC_NUM=6

# Set the MPI per node to each node.
# MPI_PER_NODE is same as -perhost or -ppn paramaters in mpirun/mpiexec
export MPI_PER_NODE=2
```

### Script to Change Parameters
```
sed -i -e 's/.*Ns.*/373504 396032 404992\ Ns/' HPL.dat
sed -i -e 's/.*(N).*/3\            # of problems sizes (N)/' HPL.dat
sed -i -e 's/.*\ Ps.*/2\ Ps/' HPL.dat
sed -i -e 's/.*\ Qs.*/3\ Qs/' HPL.dat
sed -i 's/MPI_PROC_NUM=2/MPI_PROC_NUM=6/' runme_intel64_dynamic
```

## Test HPL on 3 Nodes

**hpl3nodes.slurm**

```
#!/bin/bash

#SBATCH --job-name=hpl3node
#SBATCH --partition=argon
#SBATCH --output=%j.out
#SBATCH --error=%j.err
#SBATCH -n 6
#SBATCH --ntasks-per-node=2
#SBATCH --cpus-per-task=32
#SBATCH --mem=503G
#SBATCH --exclusive

ulimit -s unlimited
ulimit -l unlimited

module load oneapi

./runme_intel64_dynamic
```

```
sbatch hpc3nodes.slurm
```

## Result
```
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      373504   192     2     3            2866.12            1.21200e+04
HPL_pdgesv() start time Wed Oct 23 15:11:00 2024

HPL_pdgesv() end time   Wed Oct 23 15:58:46 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   3.08828052e-03 ...... PASSED
```

```
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      373504   256     2     3            2834.45            1.22554e+04
HPL_pdgesv() start time Wed Oct 23 16:00:03 2024

HPL_pdgesv() end time   Wed Oct 23 16:47:18 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   3.30804262e-03 ...... PASSED
```

```
===============================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      396032   192     2     3            3430.64            1.20706e+04
HPL_pdgesv() start time Wed Oct 23 16:48:39 2024

HPL_pdgesv() end time   Wed Oct 23 17:45:50 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   2.94238774e-03 ...... PASSED
```

```
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      396032   256     2     3            3384.76            1.22342e+04
HPL_pdgesv() start time Wed Oct 23 17:47:17 2024

HPL_pdgesv() end time   Wed Oct 23 18:43:42 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   2.90265108e-03 ...... PASSED
```

```
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      404992   192     2     3            3652.15            1.21255e+04
HPL_pdgesv() start time Wed Oct 23 18:45:11 2024

HPL_pdgesv() end time   Wed Oct 23 19:46:03 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   3.21215278e-03 ...... PASSED
```

```
================================================================================
T/V                N    NB     P     Q               Time                 Gflops
--------------------------------------------------------------------------------
WC00C2R2      404992   256     2     3            3608.68            1.22716e+04
HPL_pdgesv() start time Wed Oct 23 19:47:33 2024

HPL_pdgesv() end time   Wed Oct 23 20:47:42 2024

--------------------------------------------------------------------------------
||Ax-b||_oo/(eps*(||A||_oo*||x||_oo+||b||_oo)*N)=   3.54650347e-03 ...... PASSED
================================================================================

Finished      6 tests with the following results:
              6 tests completed and passed residual checks,
              0 tests completed and failed residual checks,
              0 tests skipped because of illegal input values.
--------------------------------------------------------------------------------

End of Tests.
================================================================================
```

## Efficiency

**Theoretical peak**
```
= nodes*cores_per_node*(Main frequency)* \
  (number of floating-point operations per clock cycle)
```

**Efficency**
```
=1.22716e+04/(3*64*32*2.3)=86.8%
```


