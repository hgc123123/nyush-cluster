# Querying Storage Quotas

!!! info "Outdated"

    This document is only valid for the old, third-generation file system and will be removed soon.

As described elsewhere, all data in your user and group volumes is subject to quotas.
This page quickly shows how to query for the current usage of data volume and file counts for your user, group, and projects.

## Query for User Data and File Usage

The file `/etc/bashrc.gpfs-quota` contains some Bash functions that you can use for querying the quota usage.
This file is automatically sourced in all of your Bash sessions.

For querying your user's data and file usage, enter the following command:

```
# mmlsquota -u $USER --block-size=1G
```

You will get a report as follows.

```
                         Block Limits                                               |     File Limits
Filesystem Fileset    type         blocks      quota      limit   in_doubt    grace |    files   quota    limit in_doubt    grace  Remarks
gpfs       home       USR          1.713G        50G        60G          0     none |    16999       0        0        0     none 
gpfs       scratch    USR          1.247T         5T         6T          0     none |     2977       0        0        0     none
```
