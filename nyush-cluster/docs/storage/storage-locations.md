# Storage and Volumes: Locations
This document describes the iteration of the file system structure on the NYUSH HPC cluster.

## Organizational Entities
There are the following two entities on the cluster:

1. **Users** *(real people)*
2. **Groups** *(Arbeitsgruppen)* with one leader and an optional delegate

Each user and group can have storage folders in different locations.

## Data Types and Storage Tiers
Files stored on the HPC fall into one of two categories:

1. **Home** folders store programs, scripts, and user config i.&nbsp;e. long-lived and very important files. 
Loss of this data requires to redo manual work (like programming).

2. **Scratch** folders store data of potentially large size which has a medium life time and is important.
Examples are raw sequencing data and intermediate results that are to be kept (e.&nbsp;g. sorted and indexed BAM files).

Storage comes in two types which differ in their I/O speed, total capacity, and cost. They are called Tier 1 and Tier 2 and sometimes hot storage and warm storage. In the HPC filesystem they are mounted in /gpfsnyu/home and /gpfsnyu/scratch.

- Tier 1 storage is fast, relatively small, expensive, and optimized for performance.
- Tier 2 storage is slow, big, cheap, and built for keeping large files for longer times.

Storage quotas are imposed in these locations to restrict the maximum size of folders.
Amount and utilization of quotas is communicated via the [HPC Access](https://ood.shanghai.nyu.edu/grafana) web portal.

### Home Directories
Location: `/gpfsnyu/home/`

Only users have home directories on Tier 1 storage.
This is the starting point when starting a new shell or SSH session.
Important config files are stored here as well as analysis scripts and small user files.
Home folders have a [strict storage quota](./home-quota.md) of 50 GB.

### Scratch Space
Location: `/gpfsnyu/scratch`

This is where big files go when they are not in active use.
Users are allocated 10 TB of Tier 2 storage by default.
File quotas here can be significantly larger as space is much cheaper and more abundant than on Tier 1.


### Overview

| Tier | Function        | Path                                           | Default Quota |
|:-----|:----------------|:-----------------------------------------------|--------------:|
|    1 | User home       | `/gpfsnyu/home/users/<user>`                   | 50 GB         |
|    2 | scratch         | `/gpfsnyu/scratch/users/<user>`                | 10 TB         |

## Snapshots and Mirroring
Snapshots are incremental copies of the state of the data at a particular point in time. 
They provide safety against various "Ops, did I just delete that?" scenarios, meaning they can be used to recover lost or damaged files.
Depending on the location and Tier, GPFS creates snapshots in different frequencies and retention plans.

| Location                 | Path                         | Retention policy                | Mirrored |
|:-------------------------|:-----------------------------|:--------------------------------|---------:|
| User homes               | `/gpfsnyu/home/users/`       | Hourly for 48 h, daily for 14 d | yes      |

