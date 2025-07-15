# Introducation

From the official [website](https://sharc-md.org/):

The SHARC molecular dynamics (MD) program suite is an ab initio MD software package developed to study the excited-state dynamics of molecules.

## Version
| Cluster | Version | Module                         |
|:--------|:-------:|:-------------------------------|
| nyushc  | 4.0     | sharc4/4.0-oneapi-2022.0.1     |

## Test Data
```
mkdir ~/sharc4
cd ~/sharc4
cp -r /gpfsnyu/spack/share/data/ORCA_curvature ./
```

## Submit SHARC4-ORCA jobs

**sharc4-orca.slurm**
```
#!/bin/bash
#SBATCH --job-name=sharc4
#SBATCH --partition=parallel
#SBATCH -N 1
#SBATCH --ntasks-per-node=1
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load  sharc4/4.0-oneapi-2022.0.1 orca
cd ~/sharc4/ORCA_curvature
export SCRADIR=/gpfsnyu/scratch/**your own netid**/scradir
./run.sh
```

```
sbatch sharc4-orca.slurm
```

## Result
**Location**
```
/gpfsnyu/scratch/**your own netid**/scradir/WORK/master_1
```

```
cd /gpfsnyu/scratch/**your own netid**/scradir/WORK/master_1

[hpc@hpclogin1 master_1]$ tail -n 10 ORCA.log 
Timings for individual modules:

Sum of individual times          ...        5.113 sec (=   0.085 min)
Startup calculation              ...        0.338 sec (=   0.006 min)   6.6 %
SCF iterations                   ...        1.579 sec (=   0.026 min)  30.9 %
Property integrals               ...        0.112 sec (=   0.002 min)   2.2 %
Property calculations            ...        0.073 sec (=   0.001 min)   1.4 %
CIS module                       ...        3.011 sec (=   0.050 min)  58.9 %
                             ****ORCA TERMINATED NORMALLY****
TOTAL RUN TIME: 0 days 0 hours 0 minutes 5 seconds 266 msec
```
