# Introducation

Amber is a collection of programs that implement biomolecular simulations at the atomic level, primarily through molecular dynamics.

## Version
| Cluster | Version | Module                             |
|:--------|:-------:|:-----------------------------------|
| nyushc  | 22      | amber/22-oneapi-2022.0.1           |
| nyushc  | 22      | amber/22-cuda-11.4.2               |
| nyushc  | 22      | amber/22-openmpi-4.0.3-cuda-11.4.3 |

## Test Data
```
mkdir ~/amber
cd ~/amber
cp -r /gpfsnyu/spack/opt/spack/linux-rhel8-icelake/contribute/amber22/data/* ./
```

## 1.Submit CPU jobs

**amber_cpu.slurm**
```
#!/bin/bash
#SBATCH --job-name=Amber_mpi
#SBATCH --partition=parallel
#SBATCH -N 1
#SBATCH --ntasks-per-node=64
#SBATCH --time=10:00:00
#SBATCH --output=%j.out
#SBATCH --error=%j.err
ulimit -l unlimited
module load  amber/22-oneapi-2022.0.1
mpirun pmemd.MPI -O -i mdin -o mdout -p prmtop -c inpcrd
```

```
sbatch amber_cpu.slurm
```

## 2.Submit GPU jobs

**amber_gpu.slurm**
```
#!/bin/bash
#SBATCH --job-name=Amber_mpi
#SBATCH --partition=chem
#SBATCH -N 1
#SBATCH --ntasks-per-node=6
#SBATCH --gres=gpu:1
#SBATCH --time=10:00:00
#SBATCH --output=%j.out
#SBATCH --error=%j.err
ulimit -l unlimited
module load  singularity amber/22-cuda-11.4.2
pmemd.cuda -O -i mdin -o mdout -p prmtop -c inpcrd
```

```
sbatch amber_gpu.slurm
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
