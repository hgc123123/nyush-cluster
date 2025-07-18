# Introducation

Gaussian is a widely used electronic structure program in computational chemistry. It predicts molecular properties and reactions using quantum mechanical methods. Key features include calculating energies, molecular structures, vibrational frequencies, and various molecular properties for diverse chemical systems.

## Version
| Cluster | Version | Module            |
|:--------|:-------:|:------------------|
| nyushc  | 09      | gaussian/09       |
| nyushc  | 16      | gaussian/16.b01   |

## Test Data
gaussian.com
```
%NProcShared=2
%Mem=5GB
%NoSave
%chk=g16.chk
# opt freq b3lyp/cc-pvdz

Title Card: Single Molecule Formamide

0 1
 C                  3.89594917   -4.10509404   -0.06119675
 O                  5.13960552   -4.14687675    0.12626969
 H                  3.41099598   -3.16562892   -0.22589552
 N                  3.10941607   -5.34695260   -0.05391726
 H                  2.79423275   -5.53644967    0.87600227
 H                  2.31994463   -5.24505745   -0.65918762
```

## Running a calculation using multiple cores

**gaussian.slurm**
```
#!/bin/bash
#SBATCH --job-name=gaussian
#SBATCH --partition=debug
#SBATCH -N 1
#SBATCH --ntasks-per-node=2
#SBATCH --output=%j.out
#SBATCH --error=%j.err
ulimit -l unlimited
module load  gaussian/09
g09 < gaussian.com > gaussian.log
```

## Total Time

```
Job cpu time = single-core time × number of cores
```

**g09**

| Cores                  | 1 core | 2 cores | 4 cores | 8 cores |
|:-----------------------|:------:|:-------:|:-------:|:--------|
| Job cpu time (seconds) | 17.9   | 18.7    | 20.6    |	25.4    |
| Elapsed time (seconds) | 17.9   |  9.5    |  5.3    |	 3.4    |

**g16**

| Cores                  | 1 core | 2 cores | 4 cores | 8 cores |
|:-----------------------|:------:|:-------:|:-------:|:--------|
| Job cpu time (seconds) | 17.7   | 18.4    | 20.4    | 25.0    |
| Elapsed time (seconds) | 17.9   |  9.3    |  5.3    |  3.4    |
