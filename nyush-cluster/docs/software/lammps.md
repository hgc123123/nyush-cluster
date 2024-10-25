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
#SBATCH --ntasks-per-node=64
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load lammps/29Aug2024-oneapi-2022.0.1

ulimit -l unlimited
mpirun -np 64 lmp -in in.lj

#Using MPI+OpenMP
#export OMP_NUM_THREADS=32 
#mpirun -np 2 lmp -sf omp -pk omp 32 -in in.lj
```

```
sbatch lammps_cpu.slurm
```

## 2.Submit GPU jobs

**lammps_gpu.slurm**
```
#!/bin/bash

#SBATCH --job-name=Lammps_GPU
#SBATCH --partition=sfscai
#SBATCH -N 1
#SBATCH --ntasks-per-node=64
#SBATCH --gres=gpu:1
#SBATCH --output=%j.out
#SBATCH --error=%j.err
module load lammps/29Aug2024-oneapi-2022.0.1-cuda-12.1.1

ulimit -l unlimited
mpirun -np 64 lmp -sf gpu -pk gpu 1 -in in.lj
```

```
sbatch lammps_gpu.slurm
```

## Total Time (CPU vs GPU)

**CPU (module load lammps/29Aug2024-oneapi-2022.0.1)**

| Cores    | 64 (64*MPI) | 128 (128*MPI) | 192 (192*MPI) |
|:---------|:------------|:--------------|---------------|
| **Time** | 0:03:43     | 00:01:19      | 00:00:58      |

**CPU - MPI+OpenMP (module load lammps/29Aug2024-oneapi-2022.0.1)**

| Cores    | 64 (32MPI+2OpenMP) |
|:---------|:-------------------|
| **Time** | 00:03:22           |

**GPU (module load lammps/29Aug2024-oneapi-2022.0.1-cuda-12.1.1)**

| GPUs     | 1 (64*MPI+2*4090D) |
|:---------|:-------------------|
| **Time** | 00:01:05           |
