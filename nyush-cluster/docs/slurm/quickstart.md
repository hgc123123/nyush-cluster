# Slurm Quickstart

**Create an interactive bash session** (`srun` will run bash in real-time, `--pty` connects its `stdout` and `stderr` to your current session).

```bash
[hpc@hpclogin ~]$ srun --pty bash -i
[hpc@compute132 ~]$ echo "hello world"
hello world
[hpc@compute132 ~]$ exit
exit
[hpc@hpclogin ~]$
```

Note you probably want to **longer running time for your interactive jobs**.
This way, your jobs can run for up to 20 days.
This will make your job be routed automatically into the `parallel` partition as it is the only one that can fit your job.

```bash
[hpc@hpclogin ~]$ srun --pty --time 20-00 -p parallel  bash -i
[hpc@compute141 ~]$
```

**Allocate 4 cores (default is 1 core), and a total of 4GB of RAM on one node** (alternatively use `--mem-per-cpu` to set RAM per CPU); `sbatch` accepts the same argument.

```bash
[hpc@hpclogin ~]$ srun --cpus-per-task=4 --nodes=1 --mem=4G --pty bash
[hpc@hpclogin ~]$ export | grep SLURM_CPUS_ON_NODE
4
[hpc@hpclogin ~]$ your-parallel-script --threads 4
```

**Create an interactive R session on the cluster**. 

```bash
[hpc@hpclogin ~]$ module load r
[hpc@hpclogin ~]$ srun --pty R

R version 4.2.2 (2022-10-31) -- "Innocent and Trusting"
[...]
Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

[Previously saved workspace restored]

> Sys.info()["nodename"]
    nodename 
"compute132" 
> q()
Save workspace image? [y/n/c]: y
[hpc@hpclogin ~]$ 
```

