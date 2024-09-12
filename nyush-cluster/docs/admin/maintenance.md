# Next Maintenance Window

This page documents the current and known upcoming maintenance windows.

## Login, Compute and Storage Maintenance, December 13-14, 2022

All informationand updates regarding maintenance will be circulated on our forum https://hpc.

## Login, Compute and Storage Maintenance, March 22-23, 2022

All COMPUTE nodes and STORAGE resources won't be reachable!
  
> All nodes will be running in RESERVATION mode. This means you are still able to schedule new jobs on these nodes if their potential/allowed runtime does not extend into the maintenance window (Tuesday and Wednesday, March 22 and 23, all-day). For example, if you submit a job that can run up to 7 days after March 15 then the job will remain in "pending/PD" state giving the explanation of "all nodes being reserved or unavailable".

Issues of today's maintenance:

- Mounting of storage to `/tmp` on login nodes
- Changing mount options of the root partition on the compute nodes
- Upgrading all nodes kernels and further packages
    - This implies an upgrade of CUDA, Singularity, and further packages
- Cold reboot ("power off, power on") of storage system
- Exchanging `cephfs-2` switches (Tier 2 storage, not relevant for most users)

**IMPORTANT**

- **All nodes will reboot**
- **All running jobs will die**
- **All sessions on login nodes will die**

