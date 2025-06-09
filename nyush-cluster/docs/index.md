# Home
Welcome to the user documentation of the NYU Shanghai high-performance computing (HPC) cluster, also called NYUSH HPC.
This documentation is maintained by NYUSH IT and the user community.
It is a living document that you can update and add to.
See [How-To: Contribute to this Document](how-to/misc/contribute.md) for details.

:arrow_left: The global table of contents is on the left, the one of the current page is on the right. :arrow_right:

!!! tip "Important links"

    <!-- - [How to create HPC account](https://support.nyu.edu/esc?id=sc_cat_item&table=sc_cat_item&sys_id=7b9e64ce1bd912108ef92f81b24bcb2e&searchTerm=HPC) -->
    - <a href="https://workflow.shanghai.nyu.edu"  style="color:purple; font-weight:bold;">How to create HPC account</a> &nbsp;&nbsp; <b> Press</b>: My Process -> IT Request -> HPC Account  <br>
    - [Performance and workload monitoring](https://ood.shanghai.nyu.edu/grafana/d/4247817/jobinfo?orgId=1)

!!! note "Shared Models"
    Models are available in the <b>"/gpfsnyu/spack/share/modles"</b> directory.<br>
    There is no need to download them yourself to avoid wasting storage space.

## Getting Started
Read the following set of pages (in order) to learn how to get access and connect to the cluster.

1. [Connecting](connecting/connecting.md)
2. [Slurm](slurm/overview.md)
3. [HPC Tutorial](hpc-tutorial/episode-0.md)

!!! note "Acknowledging NYUSH HPC Usage"
    Acknowledge usage of the cluster in your manuscript as *"Computation has been performed on the HPC for Pudong/NYUSHC cluster of the NYU Shanghai"*.
    Please add your publications using the cluster to [this list](misc/publication-list.md).

## News & Maintenance Announcements
- :maple_leaf: September 30th 2024: Add new nodes purchased by AI center.

See [Maintenance](admin/maintenance.md) for a detailed list of current, planned, and previous maintenance and update work.

## Policies

1. The default validity period of an account is one year. The system will send a reminder within one month before expiration. Users must reapply; otherwise, the account will be disabled and the data will be deleted.

2. If a user leaves NYU Shanghai for more than six months, their account will be deleted and the data will not be retained. The system will notify the user within one month to remove their data.

3. The default storage quota for users is 50 GB in the home directory and 5 TB in the scratch directory. If additional storage is required, users must pay extra: ¥5 per GB per month in the home directory, and ¥1 per GB per month in the scratch directory.<br>
Files in the /gpfsnyu/scratch directory that have not been accessed for 2 months will be moved to /gpfsnyu/scratch/cleanup/60days-files/. A deletion notice will be sent within one week. If you still need the data, please notify shanghai.it.help@nyu.edu.<br>
Note that this mechanism affects your quota: for example, if 1 TB of data is moved to /gpfsnyu/scratch/cleanup/60days-files/ and you choose to restore it, your effective scratch quota will be reduced to 5 TB – 1 TB = 4 TB.

4. The CPU core limit on the public cluster is 500 cores. If you wish to use more, you can apply by paying an additional fee, which is ¥0.06 per core per hour.

5. Running jobs on the login node is prohibited. IT reserves the right to terminate such jobs and suspend the user's account for one month.

6. The file system throughput is limited to 10 GB/s. IT reserves the right to terminate any job that exceeds this limit and suspend the user's account for one month.


## Documentation Structure
The documentation is structured as follows:

- **Administrative** information about administrative processes such as how to get access, register users, work groups, and projects.
- **Connecting** technical help for connecting to the cluster.
- **Storage** describes how and where files are stored.
- **HPC tutorial** a first demo project for getting you started quickly.
- **Slurm** technical help for using the Slurm scheduler.
- **Miscellaneous** contains a growing list of pages that don't fit anywhere else.
