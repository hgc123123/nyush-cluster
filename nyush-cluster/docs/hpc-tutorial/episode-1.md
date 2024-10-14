# First Steps: Episode 2

|Episode|Topic|
|---|---|
| 0 | [How can I install the tools?](episode-0.md) |
| **1** | **How can I distribute my jobs on the cluster (Slurm)?** |

Welcome to the second episode of our tutorial series!

Once you are logged in to the cluster, you have the possibility to distribute your jobs to all the nodes that are available.
But how can you do this easily?
The key command to this magic is `sbatch`.
This tutorial will show you how you can use this efficiently.

## The `sbatch` Command

So what is `sbatch` doing for you?

You use the `sbatch` command in front of the script you actually want to run.
`sbatch` then puts your job into the job queue.
The job scheduler looks at the current status of the whole system and will assign the first job in the queue to a node that is free in terms of computational load.
If all machines are busy, yours will wait.
But your job will sooner or later get assigned to a free node.

We strongly recommend using this process for starting your computationally intensive tasks because you will get the best performance for your job and the
whole system won't be disturbed by jobs that are locally blocking nodes.
Thus, everybody using the cluster benefits.

You may have noticed that you run `sbatch` with a script, not with regular commands.
The reason is that `sbatch` only accepts bash scripts.
If you give `sbatch` a normal shell command or binary, it won't work.
This means that we have to put the command(s) we want to use in a bash script.

The content of the example file for CPU jobs:

```bash
#!/bin/bash

# Set a name for the job (-J or --job-name).
#SBATCH --job-name=tutorial

# Set the file to write the stdout and stderr to.
#SBATCH --output=logs/%x-%j.out
#SBATCH --error=logs/%x-%j.err

# Set the number of ntasks.
#SBATCH --ntasks-per-node=8

# Set the number of cores allocated to each task (-c or --cpus-per-task).
#SBATCH --cpus-per-task=1

# Force allocation of the eight tasks on ONE node.
#SBATCH --nodes=1

# Set the total memory. Units can be given in T|G|M|K.
#SBATCH --mem=16G

# Optionally, set the partition to be used (-p or --partition).
#SBATCH --partition=debug

# Set the expected running time of your job (-t or --time).
# Formats are MM:SS, HH:MM:SS, Days-HH, Days-HH:MM, Days-HH:MM:SS
#SBATCH --time=06:00:00

ulimit -l unlimited
module load  amber/22-oneapi-2022.0.1
mpirun -np 8   pmemd.MPI -O -i mdin -o mdout -p prmtop -c inpcrd
```

The content of the example file for GPU jobs:

```bash
#!/bin/bash
#SBATCH --job-name=tutorial

#SBATCH --output=logs/%x-%j.out
#SBATCH --error=logs/%x-%j.err

#SBATCH --nodes=1
#SBATCH --ntasks-per-node=6
#SBATCH --cpus-per-task=1
#SBATCH --mem=12G

# Set the number of GPUs.
#SBATCH --gres=gpu:1

#SBATCH --partition=sfscai  //The partition is owned by the  AI center.
#SBATCH --time=06:00:00

ulimit -l unlimited
module load  singularity amber/22-openmpi-4.0.3-cuda-11.4.3
pmemd.cuda -O -i mdin -o mdout -p prmtop -c inpcrd
```

The lines starting with `#SBATCH` are actually setting parameters for a `sbatch` command, so `#SBATCH --job-name=tutorial` is equal to `sbatch --job-name=tutorial`.
Slurm will create a log file with a file name composed of the job name (`%x`) and the job ID (`%j`), e.g. `logs/tutorial-XXXX.log`. It will not automatically create the `logs` directory, we need to do this manually first. Here, we emphasize the importance of the log files! They are the first place to look if anything goes wrong.

To start now with our tutorial, create a new tutorial directory with a log directory, e.g.,

```terminal
(first-steps) $ mkdir -p /gpfsnyu/home/*NetID*/project/logs
(first-steps) $ cd /gpfsnyu/home/*NetID*/project
```

and copy the wrapper script to this directory:

```terminal
(first-steps) $ cp /gpfsnyu/spack/share/submit_job.slurm .
```

Let's run it:

```terminal
(first-steps) $ sbatch submit_job.slurm
```

And wait for the response which will tell you that your job was submitted and which job id number it was assigned. Note that `sbatch` only tells you that the job has started, but nothing about finishing. You won't get any response at the terminal when the job finishes. It will take approximately 20 minutes to finish the job.

## Monitoring Jobs

You'll probably want to see how your job is doing. You can get a list of your jobs using:

```terminal
(first-steps) $ squeue --me
```

Identify your job by the `<JOBID>` (1st column) or the name of the script (3rd column).
The most likely states you will see (5th column of the table):

* `PD` pending, waiting to be submitted
* `R` running
* disappeared, either because of an error or because it finished

Get more information about your jobs by either passing the job id:

```terminal
(first-steps) $ sstat <JOBID>
```

And of course, watch what the logs are telling you:

```terminal
(first-steps) $ tail -f logs/tutorial-<JOBID>.log
```

There will be no notification when your job is done, so it is best to watch the `squeue --me` command.
To watch the `sbatch` command there is a linux command `watch` that you give a command to execute every few seconds.
This is useful for looking for changes in the output of a command. The seconds between two executions can be set with the `-n` option.
:warning: It is best to use `-n 60` to minimize unnecessary load on the file system:

```terminal
(first-steps) $ watch -n 60 squeue --me
```
If for some reason your job is hanging, you can delete your job using `scancel` with your job-ID:
```terminal
(first-steps) $ scancel <job-ID>
```
