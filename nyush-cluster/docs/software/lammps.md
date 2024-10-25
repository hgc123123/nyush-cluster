# Introducation

LAMMPS stands for Large-scale Atomic/Molecular Massively Parallel Simulator.

LAMMPS is a classical molecular dynamics simulation code focusing on materials modeling. It was designed to run efficiently on parallel computers and to be easy to extend and modify. 

## Version
| Cluster | Version   | Module                                       |
|:--------|:---------:|:---------------------------------------------|
| nyushc  | 29Aug2024 | lammps/29Aug2024-oneapi-2022.0.1-cuda-12.1.1 |
| nyushc  | 29Aug2024 | lammps/29Aug2024-oneapi-2022.0.1             |
| nyushc  | 29Aug2024 | lammps/20230802-oneapi-2023.1.0              |

## Test Data
```
mkdir ~/lammps
cd ~/lammps
cp -r /gpfsnyu/spack/opt/spack/linux-rhel8-icelake/contribute/lammps/data/* ./
```

## 1.Submit CPU jobs

**lammps_cpu.slurm**
```
#!/bin/bash

#SBATCH --job-name=Lammps_MPI
#SBATCH --partition=debug
#SBATCH -N 1
#SBATCH --ntasks-per-node=4
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load lammps/29Aug2024-oneapi-2022.0.1

ulimit -l unlimited
mpirun -np 4 lmp -in in.lj

#Using MPI+OpenMP
#export OMP_NUM_THREADS=2 
#mpirun -np 2 lmp -sf omp -pk omp 2 -in in.lj
```

```
sbatch lammps_cpu.slurm
```

## 2.Submit GPU jobs

**lammps_gpu.slurm**
```
#!/bin/bash

#SBATCH --job-name=Lammps_MPI
#SBATCH --partition=debug
#SBATCH -N 1
#SBATCH --ntasks-per-node=4
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load lammps/29Aug2024-oneapi-2022.0.1

ulimit -l unlimited
mpirun -np 4 lmp -sf gpu -pk gpu 1 -in in.lj
```

```
sbatch lammps_gpu.slurm
```

## Total CPU Time (CPU vs GPU)

**CPU (module load amber/22-oneapi-2022.0.1)**

| Cores    | 64            |
|:---------|:--------------|
| **Time** | 576.74 seconds|

**GPU (module load amber/22-cuda-11.4.2)**

| GPUs     | 1 RTX 3090    |
|:---------|:--------------|
| **Time** | 76.70 seconds |
