# Slurm Cheat Sheet

This page contains assorted Slurm commands and Bash snippets that should be helpful.

**`man` pages!**

```bash
$ man sinfo
$ man scontrol
$ man squeue
# etc...
```

**interactive sessions**

```bash
[hpc@hpclogin ~]$ srun --pty bash
[hpc@compute132 ~]$ echo "hello world"
hello world
[hpc@compute132 ~]$ exit
exit
[hpc@hpclogin ~]$ 
```

**batch submission**

```bash
[hpc@hpclogin ~]$ sbatch job_script.slurm 
Submitted batch job 483131
[hpc@hpclogin ~]$ squeue -u hpc
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            483131  parallel     test      hpc  R       0:06      1 compute141
```

**listing nodes**

```bash
$ sinfo -N
[hpc@hpclogin ~]$ sinfo -N
NODELIST    NODES PARTITION STATE 
compute118      1    debug* mix   
compute119      1    debug* alloc 
compute120      1    debug* idle  
compute121      1    debug* mix   
compute122      1    debug* idle  
compute134      1  parallel alloc 
compute135      1  parallel alloc 
compute136      1  parallel alloc 
compute137      1  parallel alloc 
compute138      1  parallel alloc 
compute139      1  parallel alloc 
gpu145          1   rooster idle  
gpu146          1   rooster idle  
gpu147          1   rooster idle  
gpu180          1      hebb idle  
gpu181          1      chem idle  
gpu182          1        li idle  
gpu185          1      chem idle  
gpu186          1    netsys idle  
gpu187          1    sfscai mix   
gpu190          1    sfscai mix
[....]

[hpc@hpclogin ~]$ scontrol show nodes
[hpc@hpclogin ~]$ scontrol show node compute118
NodeName=compute118 Arch=x86_64 CoresPerSocket=32 
   CPUAlloc=32 CPUEfctv=64 CPUTot=64 CPULoad=33.19
   AvailableFeatures=cpu,g6338
   ActiveFeatures=cpu,g6338
[...]

[hpc@hpclogin ~]$ scontrol show node gpu187
NodeName=gpu187 Arch=x86_64 CoresPerSocket=32 
   CPUAlloc=13 CPUEfctv=64 CPUTot=64 CPULoad=6.22
   AvailableFeatures=cpu,g6338,a800
   ActiveFeatures=cpu,g6338,a800
   Gres=gpu:a800:8(S:0-1)
   NodeAddr=10.214.97.187 NodeHostName=gpu187 Version=22.05.6
   OS=Linux 4.18.0-372.9.1.el8.x86_64 #1 SMP Fri Apr 15 22:12:19 EDT 2022 
   RealMemory=515372 AllocMem=299008 FreeMem=112490 Sockets=2 Boards=1
   State=MIXED ThreadsPerCore=1 TmpDisk=0 Weight=400 Owner=N/A MCS_label=N/A
   Partitions=sfscai 
   BootTime=2023-11-01T11:38:40 SlurmdStartTime=2024-08-06T09:17:19
   LastBusyTime=2024-09-10T06:36:37
   CfgTRES=cpu=64,mem=515372M,billing=64,gres/gpu=8
   AllocTRES=cpu=13,mem=292G,gres/gpu=5
   CapWatts=n/a
   CurrentWatts=0 AveWatts=0
   ExtSensorsJoules=n/s ExtSensorsWatts=0 ExtSensorsTemp=n/s
```

**queue states**

```bash
[hpc@hpclogin ~]$ squeue -u hpc
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
[....]

[hpc@hpclogin ~]$ squeue -u hpc
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
            483131  parallel     test      hpc  R       4:10      1 compute141
```

**node resources**

```bash
$ sinfo -o "%20N  %10c  %10m  %25f  %10G "
```

**additional resources such as GPUs**

```bash
$ sinfo -o "%N %G"
```

**listing job details**

```
[hpc@hpclogin ~]$ scontrol show job 483133
JobId=483133 JobName=test
   UserId=hpc(111222117) GroupId=hpc(111222117) MCS_label=N/A
   Priority=5248 Nice=0 Account=hpc QOS=normal
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:00:08 TimeLimit=00:30:00 TimeMin=N/A
   SubmitTime=2024-09-11T10:15:15 EligibleTime=2024-09-11T10:15:15
   AccrueTime=2024-09-11T10:15:15
   StartTime=2024-09-11T10:15:15 EndTime=2024-09-11T10:45:15 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2024-09-11T10:15:15 Scheduler=Main
   Partition=parallel AllocNode:Sid=hpclogin:2249687
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=compute141
   BatchHost=compute141
   NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
   TRES=cpu=1,mem=2G,node=1,billing=1
   Socks/Node=* NtasksPerN:B:S:C=1:0:*:* CoreSpec=*
   MinCPUsNode=1 MinMemoryCPU=2G MinTmpDiskNode=0
   Features=(null) DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/gpfsnyu/home/hpc/run.slurm
   WorkDir=/gpfsnyu/home/hpc
   StdErr=/gpfsnyu/home/hpc/483133.err
   StdIn=/dev/null
   StdOut=/gpfsnyu/home/hpc/483133.out
   Power=
```


```bash
[hpc@hpclogin ~]$ squeue 
             JOBID PARTITION     NAME   USER ST       TIME  NODES NODELIST(REASON)
            482984       aml       PT   jh99  R   17:30:02      1 compute183
            483130     debug session_   dw90  R       8:35      1 compute131
            483024     debug     bbaa   zg70  R   13:39:44      1 compute130
            483021     debug     baab   zg70  R   13:52:27      1 compute118
            482998     debug     abba   zg70  R   16:42:43      1 compute121
            482997     debug     aabb   zg70  R   16:45:55      1 compute119
            482996     debug   f2baba   zg70  R   16:51:22      1 compute119
            482955     debug   n2bbaa   zg70  R   18:29:09      1 compute132
            482954     debug   n2abba   zg70  R   18:32:13      1 compute130
            483133  parallel     test   gh40  R       1:27      1 compute141
            483135  parallel NdU6b100   jm05  R       0:13      1 compute141
            480230  parallel Moababsc   wr72  R 12-18:36:08      1 compute140
            480229  parallel Moaabbsc   wr72  R 12-18:38:11      1 compute139
            480228  parallel Wbbaasca   wr72  R 12-18:41:24      1 compute138
            480227  parallel Wbabasca   wr72  R 12-18:42:47      1 compute137
            480226  parallel Wbaabsca   wr72  R 12-18:44:45      1 compute136
            480225  parallel Wababsca   wr72  R 12-18:46:28      1 compute135
            480224  parallel Waabbsca   wr72  R 12-18:48:31      1 compute134
            480231  parallel Moababsc   wr72  R 12-18:30:26      1 compute141
            483027    sfscai prodigy-  js156  R   13:23:38      1 gpu190
            483124    sfscai      rec  yl179  R      25:46      1 gpu187
            483123    sfscai       rd  yl179  R      26:52      1 gpu190
            483119    sfscai     cold  yl179  R      46:58      1 gpu190
            483117    sfscai     cold  yl179  R    1:18:48      1 gpu187
            482628    sfscai  GT-S.sh   ys10  R 1-22:50:19      1 gpu188
            482907    sfscai      mag   qz86  R   23:21:23      1 gpu190
            483111    sfscai   POP909  jl187  R    2:05:18      1 gpu190
            482894    sfscai     bash   bc88  R 1-02:21:24      2 gpu[187-188]
            483134    sfscai     bash   yg09  R       0:48      1 gpu187
            483049    sfscai     bash   zz17  R   11:34:46      1 gpu190
            483057    sfscai ctpph_ad   zz17  R   10:32:32      1 gpu187
            482970    sfscai ctp_ph_a   zz17  R   17:45:53      1 gpu188
```

```bash
[hpc@hpclogin ~]$ squeue -o "%.10i %9P %20j %10u %.2t %.10M %.6D %10R %b"
     JOBID PARTITION NAME               USER       ST       TIME  NODES NODELIST(R TRES_PER_NODE
    482984 aml       PT                   jh99      R   17:31:36      1 compute183 N/A
    483130 debug     session_desktop_6o   dw90      R      10:09      1 compute131 N/A
    483024 debug     bbaa                 zg70      R   13:41:18      1 compute130 N/A
    483021 debug     baab                 zg70      R   13:54:01      1 compute118 N/A
    482998 debug     abba                 zg70      R   16:44:17      1 compute121 N/A
    482997 debug     aabb                 zg70      R   16:47:29      1 compute119 N/A
    482996 debug     f2baba               zg70      R   16:52:56      1 compute119 N/A
    482955 debug     n2bbaa               zg70      R   18:30:43      1 compute132 N/A
    482954 debug     n2abba               zg70      R   18:33:47      1 compute130 N/A
    483133 parallel  test                 gh40      R       3:01      1 compute141 N/A
    483135 parallel  NdU6b100             jm05      R       1:47      1 compute141 N/A
    480230 parallel  Moababscan           wr72      R 12-18:37:42      1 compute140 N/A
    480229 parallel  Moaabbscan           wr72      R 12-18:39:45      1 compute139 N/A
    480228 parallel  Wbbaascan            wr72      R 12-18:42:58      1 compute138 N/A
    480227 parallel  Wbabascan            wr72      R 12-18:44:21      1 compute137 N/A
    480226 parallel  Wbaabscan            wr72      R 12-18:46:19      1 compute136 N/A
    480225 parallel  Wababscan            wr72      R 12-18:48:02      1 compute135 N/A
    480224 parallel  Waabbscan            wr72      R 12-18:50:05      1 compute134 N/A
    480231 parallel  Moababscan           wr72      R 12-18:32:00      1 compute141 N/A
    483027 sfscai    prodigy-evaluation-1 js556     R   13:25:12      1 gpu190     gres:gpu:1
    483124 sfscai    rec                  yl379     R      27:20      1 gpu187     gres:gpu:1
    483123 sfscai    rd                   yl379     R      28:26      1 gpu190     gres:gpu:1
    483119 sfscai    cold                 yl379     R      48:32      1 gpu190     gres:gpu:1
    483117 sfscai    cold                 yl379     R    1:20:22      1 gpu187     gres:gpu:1
    482628 sfscai    GT-S.sh              ys10      R 1-22:51:53      1 gpu188     gres:gpu:1
    482907 sfscai    mag                  qz86      R   23:22:57      1 gpu190     gres:gpu:2
    483111 sfscai    POP909               jl087     R    2:06:52      1 gpu190     gres:gpu:1
    482894 sfscai    bash                 bc88      R 1-02:22:58      2 gpu[187-18 gres:gpu:2
    483134 sfscai    bash                 yg09      R       2:22      1 gpu187     gres:gpu:1
    483049 sfscai    bash                 zz17      R   11:36:20      1 gpu190     gres:gpu:1
    483057 sfscai    ctpph_adp            zz17      R   10:34:06      1 gpu187     gres:gpu:1
    482970 sfscai    ctp_ph_adp           zz17      R   17:47:27      1 gpu188     gres:gpu:1
```


```bash
[hpc@hpclogin ~]$ sinfo 
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
parallel     up 20-00:00:0      1    mix compute141
parallel     up 20-00:00:0      7  alloc compute[134-140]
parallel     up 20-00:00:0      3   idle compute[142-144]
rooster      up   infinite      4   idle gpu[145-148]
hebb         up   infinite      1   idle gpu180
debug*       up 7-00:00:00      1   resv compute133
debug*       up 7-00:00:00      3    mix compute[118,121,132]
debug*       up 7-00:00:00      3  alloc compute[119,130-131]
debug*       up 7-00:00:00      9   idle compute[120,122-129]
chem         up   infinite      2   idle gpu[181,185]
li           up   infinite      1   idle gpu182
aml          up   infinite      1    mix compute183
aml          up   infinite      2   idle compute[184,189]
netsys       up   infinite      1   idle gpu186
sfscai       up 2-00:00:00      3    mix gpu[187-188,190]
```
