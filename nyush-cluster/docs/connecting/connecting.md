# Connecting to the HPC cluster
The HPC cluster is only available via the campus networks.
VPN access requires additional measures which are described in [Connecting from External Networks](./from-external.md).

There are two primary methods for interacting with NYU Shanghai HPC:

1. Through the “Ondemand” web portal.
2. Via SSH and Slurm.

This part of the documentation only described direct console access via SSH.
For information regarding the web portal, please read [OnDemand Portal](../software/ondemand-overview.md).
In case you're not familiar with SSH, you should probably start via the web portal or (if you are determined to learn) read through our [SSH basics](ssh-basics.md) page.

## In brief
Follow these steps to connect to NYU Shanghai HPC via the command line:

1. **Register an account** via [submiting forum](https://nyu.service-now.com/sp?id=sc_cat_item&sys_id=b0fc230be498d6408b4d97a033492665). :memo: 
2. Connect to one of the two login nodes.
    
    ```bash
    # NYUSHC Cluster
    $ ssh NetID@hpclogin.shanghai.nyu.edu
    ```

    !!! hint
        `hpclogin.shanghai.nyu.edu` is the login node. 
         Please do not perform big file transfers or an `sshfs` mount via the login nodes. 

    Please also read [Advanced SSH](./advanced-ssh/overview.md) for more custom scenarios how to connect to NYU Shanghai HPC.
    If you are using a Windows PC to access NYU Shanghai HPC, please read [Connecting via SSH on Windows](./connecting-windows.md)

3. Allocate resources on a computation node using [Slurm](../slurm/overview.md). Do not compute on the login node!

    ```bash
    # Start interactive shell on computation node
    $ srun --pty bash -i
    ```

4. Bonus: [Connect from external networks :flying_saucer:](./from-external.md).

!!! tip "tl;dr"

    - Web Access: https://ood.shanghai.nyu.edu
    - SSH-Based Access:

        ```bash
        # Interactive login (choose one)
        ssh NetID@hpclogin.shanghai.nyu.edu
        srun --pty bash -i
        ```

## What is my username?
Your username for accessing the cluster are your NetID:

## How can I connect from the outside?
Please read [Connecting from External Networks](./from-external.md)

## I have problems connecting
Please read [Debugging Connection Problems](./connection-problems.md)
