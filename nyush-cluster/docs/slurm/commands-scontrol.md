# Slurm Command: `scontrol`

The `scontrol` allows to query detailed information from the scheduler and perform manipulation.
Object manipulation is less important for normal users.

!!! info "Representative Example"

    ```bash
   [hpc@hpclogin ~]$ scontrol show job 482955
    JobId=482955 JobName=n2bbaa
       UserId=userid(userid) GroupId=groupId(groupId) MCS_label=N/A
       Priority=1052 Nice=0 Account=jz2 QOS=normal
       [...] 

    [hpc@hpclogin ~]$ scontrol show node compute[118-122]
    NodeName=compute118 Arch=x86_64 CoresPerSocket=32 
       CPUAlloc=0 CPUEfctv=64 CPUTot=64 CPULoad=0.00
       AvailableFeatures=cpu,g6338
       ActiveFeatures=cpu,g6338
       [...]

    [hpc@hpclogin ~]$ scontrol show partition debug
    PartitionName=debug
       AllowGroups=ALL AllowAccounts=ALL AllowQos=normal
       AllocNodes=ALL Default=YES QoS=N/A
       DefaultTime=00:30:00 DisableRootJobs=NO ExclusiveUser=NO GraceTime=0 Hidden=NO
       [...]
   ```

This command allows to query all information for an object from Slurm, e.g., jobs, nodes, or partitions.
The command also accepts ranges of jobs and hosts.
It is most useful to get the information of one or a few objects from the scheduler.

!!! info "Slurm Documentation: scontrol"

    Please also see the official [Slurm documentation on scontrol](https://slurm.schedmd.com/scontrol.html).

## Important Sub commands

- `scontrol show job`
    -- Show details on jobs.
- `scontrol show partition`
    -- Show details on partitions.
- `scontrol show node`
    -- Show details on nodes.
- `scontrol help`
    -- Show help.
- `scontrol`
    -- Start an interactive scontrol shell / REPL (read-eval-print loop).

## Notes

- `scontrol` can only work on jobs that are pending (in the queue), running, or in "completing' state.
- For jobs that have finished, you have to use Slurm's accounting features, e.g., with the [`sacct`](commands-sacct.md) command.
