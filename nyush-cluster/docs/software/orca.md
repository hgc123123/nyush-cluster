# Introducation

From the official [website](https://www.faccts.de/orca/):

ORCA is a powerful and versatile quantum chemistry software package, primarily developed by the group of Prof. Frank Neese. It is free for academic use.

## Version
| Cluster | Version | Module               |
|:--------|:-------:|:---------------------|
| nyushc  | 6.0.0   | orca/6.0.0           |

## Test Data
```
mkdir ~/orca
cd ~/orca
cp -r /gpfsnyu/spack/share/data/ORCA/* ./
```

## 1.Running a calculation in parallel

For instance, a calculation using four processors requires:

**orca.slurm**
```
#!/bin/bash
#SBATCH --job-name=orca
#SBATCH --partition=debug
#SBATCH -N 1
#SBATCH --ntasks-per-node=4
#SBATCH --output=%j.out
#SBATCH --error=%j.err
ulimit -l unlimited
module load  orca/6.0.0
cd ~/orca
orca=$(which orca)
$orca test.inp > test.log
```

PAL4 `!HF DEF2-SVP PAL4`  in the input file represents 4 cpu cores.

## Total CPU Time

| Cores    | 1 core         | 4 cores      | 8 cores       |
|:---------|:--------------:|:------------:|:--------------|
| **Time** | 14sec 793msec  | 6sec 206msec | 5sec 380msec  |

