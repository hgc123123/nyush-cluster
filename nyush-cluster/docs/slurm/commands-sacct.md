# Slurm Command: `sacct`

Perform queries to the Slurm accounting information.

!!! info "Representative Example"

    ```bash
    [gh2440@hpclogin ~]$ sacct -j 483188
    JobID           JobName  Partition    Account  AllocCPUS      State ExitCode 
    ------------ ---------- ---------- ---------- ---------- ---------- -------- 
    483188               PT        aml     jh3899          9    RUNNING      0:0 
    483188.batch      batch                jh3899          9    RUNNING      0:0 
    483188.exte+     extern                jh3899          9    RUNNING      0:0 
    483188.0         matlab                jh3899          9    RUNNING      0:0
    ```

The `sacct` command displays information from the Slurm accounting service.
The Slurm scheduler only knows about active or completing (very recently active) jobs.
The accouting system also knows about currently running jobs so it is the more robust way to query information about jobs.
However, not all information is available to the accouting system, so `scontrol show job` and `squeue` provide more information about current and pending jbos.

!!! info "Slurm Documentation: sacct"

    Please also see the official [Slurm documentation on sacct](https://slurm.schedmd.com/sacct.html).

## Important Arguments

Also see all important arguments of the [`sbatch`](commands-sbatch.md) command.

- `--jobs`
  -- The job(s) to query for.
- `--format`
  -- Define attributes to retrieve.
- `--long`
  -- Get a lot of information from the database, consider to pipe into `| less -S`.

## Notes

- If you need to get information about a job regardless of it being in the past, present, or future execution, use `sacct` over `scontrol` and `squeue`.
