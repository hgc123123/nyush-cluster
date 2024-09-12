# Slurm Command: `sinfo`

The `sinfo` command allows you to query the current cluster status.

!!! info "Representative Example"

    ```bash
    [gh2440@hpclogin ~]$ sinfo 
    PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
    parallel     up 20-00:00:0      1    mix compute141
    parallel     up 20-00:00:0      7  alloc compute[134-140]
    parallel     up 20-00:00:0      3   idle compute[142-144]
    rooster      up   infinite      4   idle gpu[145-148]
    hebb         up   infinite      1   idle gpu180
    debug*       up 7-00:00:00      1   resv compute133
    debug*       up 7-00:00:00      4    mix compute[119,121,130,132]
    debug*       up 7-00:00:00      2  alloc compute[120,131]
    debug*       up 7-00:00:00      9   idle compute[118,122-129]
    chem         up   infinite      2   idle gpu[181,185]
    li           up   infinite      1   idle gpu182
    aml          up   infinite      1    mix compute183
    aml          up   infinite      2   idle compute[184,189]
    netsys       up   infinite      1   idle gpu186
    sfscai       up 2-00:00:00      2    mix gpu[188,190]
    sfscai       up 2-00:00:00      1   idle gpu187
    [gh2440@hpclogin ~]$ sinfo --summarize
    PARTITION AVAIL  TIMELIMIT   NODES(A/I/O/T) NODELIST
    parallel     up 20-00:00:0         8/3/0/11 compute[134-144]
    rooster      up   infinite          0/4/0/4 gpu[145-148]
    hebb         up   infinite          0/1/0/1 gpu180
    debug*       up 7-00:00:00        6/10/0/16 compute[118-133]
    chem         up   infinite          0/2/0/2 gpu[181,185]
    li           up   infinite          0/1/0/1 gpu182
    aml          up   infinite          1/2/0/3 compute[183-184,189]
    netsys       up   infinite          0/1/0/1 gpu186
    sfscai       up 2-00:00:00          2/1/0/3 gpu[187-188,190]
    ```

This command will summaries the state of nodes by different criteria (e.g., by partition or globally).

!!! info "Slurm Documentation: sinfo"

    Please also see the official [Slurm documentation on srun](https://slurm.schedmd.com/sinfo.html).

## Important Arguments

Also see all important arguments of the [`sinfo`](commands-sinfo.md) command.

- `--summarize`
    -- Summarize the node state by partition.
- `--nodes`
    -- Select the nodes to show the status for, e.g., display the status of all GPU nodes with `sinfo -n med030[1-4]`.

## Node States

The most important node states are:

- `down` -- node is marked as offline
- `draining` -- node will not accept any more jobs but has jobs running on it
- `drained` -- node will not accept any more jobs and has no jobs running on it, but is not offline yet
- `idle` -- node is ready to run jobs
- `allocated` -- node is fully allocated (e.g., CPU, RAM, or GPU limit has been reached)
- `mixed` -- node is running jobs but there is space for more

## Notes

- Also see the [Slurm Format Strings](format-strings.md) section.
