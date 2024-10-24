# Next Maintenance Window

This page documents the current and known upcoming maintenance windows.

## Update software for the HPC switch, November 28-30, 2024

All COMPUTE nodes and STORAGE resources won't be reachable!
  
> All nodes will be running in RESERVATION mode. This means you are still able to schedule new jobs on these nodes if their potential/allowed runtime does not extend into the maintenance window (Noveber 28, 29 and 23, all-day). For example, if you submit a job that can run up to 7 days after Noveber 28 then the job will remain in "pending/PD" state giving the explanation of "all nodes being reserved or unavailable".

Issues of today's maintenance:

- Mounting of storage to `/tmp` on login nodes

**IMPORTANT**

- **All running mutilple-node jobs will die**
