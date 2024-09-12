# How-To: Connect to GPU Nodes

Connecting to a node with GPUs is easy.
You request one or more GPU cores by adding a generic resources flag to your Slurm job submission via `srun` or `sbatch`.

- `--gres=gpu:tesla:COUNT` will request NVIDIA V100 cores.
- `--gres=gpu:COUNT` will request any available GPU cores.

Your job will be automatically placed in the Slurm `gpu` partition and allocated a number of `COUNT` GPUs.

!!! info
    Fair use rules apply.
    As GPU nodes are a limited resource, excessive use by single users is prohibited and can lead to mitigating actions.
    Be nice and cooperative with other users.

!!! warning "Interactive Use of GPU Nodes is Discouraged"

    While interactive computation on the GPU nodes is convenient, it makes it very easy to forget a job after your computation is complete and let it run idle.
    While your job is allocated, it blocks the **allocated** GPUs and other users cannot use them although you might not be actually using them.
    Please prefer batch jobs for your GPU jobs over interactive jobs.

    Furthermore, interactive GPU jobs are currently limited to 24 hours.
    We will monitor the situation and adjust that limit to optimize GPU usage and usability.

    Please also note that allocation of GPUs through Slurm is mandatory, in other words: Using GPUs via SSH sessions is prohibited.
    The scheduler is not aware of manually allocated GPUs and this interferes with other users' jobs.

## Usage example
### Preparation
We will setup a miniconda installation with `pytorch` testing the GPU.
If you already have this setup then you can skip this step

```bash
[hpc@hpclogin ~]$ module load miniconda3
[hpc@hpclogin ~]$ conda create -n gpu-test python=3.11
[hpc@hpclogin ~]$ source activate gpu-test
[hpc@hpclogin ~]$ pip3 install torch torchvision torchaudio
[hpc@hpclogin ~]$ python -c 'import torch; print(torch.cuda.is_available())'
False
```

The `False` shows that CUDA is not available on the node but that is to be expected.
We're only warming up!

### Allocating GPUs

Let us now allocate a GPU.
The Slurm schedule will properly allocate GPUs for you and setup the environment variable that tell CUDA which devices are available.
The following dry run shows these environment variables (and that they are not available on the login node).

```bash
[hpc@hpclogin ~]$ export | grep CUDA_VISIBLE_DEVICES
[hpc@hpclogin ~]$ srun -p sfscai -N 1 -n 2 --gres=gpu:1 --pty bash
[hpc@gpu190 ~]$ export | grep CUDA_VISIBLE_DEVICES
declare -x CUDA_VISIBLE_DEVICES="0"
[hpc@gpu190 ~]$ exit
exit
[hpc@hpclogin ~]$ srun -p sfscai -N 1 -n 2 --gres=gpu:2 --pty bash
[hpc@gpu190 ~]$ export | grep CUDA_VISIBLE_DEVICES
declare -x CUDA_VISIBLE_DEVICES="0,1"
```

As you see, you can also reserve multiple GPUs.
If we were to open two concurrent connections (e. g. in a `screen`) to the same node when allocating one GPU each, the allocated GPUs would be non-overlapping.
Note that any two jobs are isolated using Linux cgroups ("container" technology) so you cannot accidentally use a GPU of another job.

Now to the somewhat boring part where we show that CUDA actually works.

```bash
[hpc@gpu190 ~]$ module load cuda/12.1.0-gcc-8.5.0 
[hpc@gpu190 ~]$ nvcc --version
nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2023 NVIDIA Corporation
Built on Tue_Feb__7_19:32:13_PST_2023
Cuda compilation tools, release 12.1, V12.1.66
Build cuda_12.1.r12.1/compiler.32415258_0
[hpc@gpu190 ~]$ module load miniconda3
[hpc@gpu190 ~]$ source activate gpu-test
[hpc@gpu190 ~]$ python -c 'import torch; print(torch.cuda.is_available())'
True
```

!!! note
    If scheduling a GPU fails, consider explicitely requesting the GPU partion via
    `--partition gpu` (or `#SBATCH --partition gpu`).

    Also make sure to read the FAQ entry "[I have problems connecting to the GPU node! What's wrong?](../../help/faq.md#i-have-problems-connecting-to-the-gpu-node-whats-wrong)" if you encounter problems.

## Bonus #1: Who is using the GPUs?

Use `squeue` to find out about currently queued jobs (the `egrep` only keeps the header and entries in the `sfscai` partition).

```bash
[hpc@hpclogin ~]$ squeue | egrep -iw 'JOBID|sfscai'
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
                33    sfscai     bash holtgrem  R       2:26      1 gpu187
```

## Fair Share / Fair Use

**Note that allocating a GPU makes it unavailable for everyone else, so please behave nicely and be cooperative.**
If you see someone blocking the GPU nodes for a long time, first send a friendly email, most likely they blocked the node accidentally.
If you cannot resolve the issue (e. g. the user is not reachable) then please contact shanghai.it.help@nyu.edu.
