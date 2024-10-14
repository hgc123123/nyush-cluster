# Transfer Files and Directories
For storage mounted directly on the HPC cluster, files can be transferred to and from the cluster using regular file copying, e.g. `cp` and `rsync`. 

For non-mounted storage, files may be transferred to and from the cluster via secure copying, e.g. `scp` and `sftp`, which can be utilized via `rsync`. 

For file downloads from online resources, tools such as `curl`, `wget`, and `rsync` may be used.

!!! note "Important"
    `Compute nodes` have only HTTP/HTTPS internet access via a proxy server, i.e. they cannot be used for file transfers other than between mount points. **File transfers can be done via the [login nodes](../connecting/connecting.md).**

!!! note
    TIPS: Try to use scp -c netid@hpclogin.shanghai.nyu.edu ... to speed up the transfer rates. There can be a fair bit of CPU overhead when SCP encrypts the data before transferring it - this option makes it use a faster encryption cipher.

## From your local machine to the HPC cluster
Copy a single file on your local file system to your home directory 
To copy a single file to your home directory (~/) on the cluster, use
    
    ```bash
    $ scp one_file.tsv netid@hpclogin.shanghai.nyu.edu:~/
    ```
Copy one or more files to a folder on the cluster
To copy multiple files to the HPC cluster so they appear directly under ~/study/files/, use

    ```bash
    $ scp *.txt *.R ../some/path/another_file.tsv netid@hpclogin.shanghai.nyu.edu:~/study/files/
    ```
Recursively copy a directory to a folder on the cluster 
To copy all content of directory dataset/ to the HPC cluster so that it appears as ~/study/dataset/, use

    ```bash
    $ scp -r dataset/ netid@hpclogin.shanghai.nyu.edu:~/study/
    ```

## From the HPC cluster to your local machine
Copy a single file from your home directory to your local machine 
To copy a single file in your home directory to the working directory of your local machine, use

    ```bash
    {local}$ cd /path/project
    {local}$ scp netid@hpclogin.shanghai.nyu.edu:~/study/file.csv .
    ```

!!! note 
    Don’t forget that period (.) at the end - it indicates copy [the file] "to the current directory".

Copy one or more files from the cluster 
To copy multiple files from `~/study/files/` on the cluster to `~/study/` on your local machine, do:

    ```bash
    {local}$ cd /path/project
    {local}$ scp netid@hpclogin.shanghai.nyu.edu:~/study/files/*.txt netid@hpclogin.shanghai.nyu.edu:~/study/files/*.R ~/study/
    ```

Recursively copy a folder from the cluster 
To copy all content of directory dataset/ on the cluster so that it appears as dataset/ in your local working directory, use


    ```bash
    {local}$ cd /path/project
    {local}$ scp netid@hpclogin.shanghai.nyu.edu:~/dataset/ .
    ```
!!! note 
    Don’t forget that period (.) at the end - it indicates copy [the folder] “to the current directory”.
