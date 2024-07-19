title: Monitor your Jobs(qstat, qdel, qalter)
## Get information about the state of the scheduler

When deciding which jobs to run, the scheduler takes the following details into account:

- are there available resources
- how recently has this user run jobs successfully
- how many resources has this user used recently
- how long is the job's Walltime
- how long has the job been in the queue

You can get an overview of the compute nodes and a list of all the jobs running on each node using `pstat`

``` bash
[z1234567@katana2 src]$ pstat
k001  normal-mrcbio           free          12/44   200/1007gb  314911*12
k002  normal-mrcbio           free          40/44    56/ 377gb  314954*40
k003  normal-mrcbio           free          40/44   375/ 377gb  314081*40
k004  normal-mrcbio           free          40/44    62/ 377gb  314471*40
k005  normal-ccrc             free           0/32     0/ 187gb
k006  normal-physics          job-busy      32/32   180/ 187gb  282533*32
k007  normal-physics          job-busy      32/32   180/ 187gb  284666*32
k008  normal-physics          free           0/32     0/ 187gb
k009  normal-physics          job-busy      32/32   124/ 187gb  314652*32
k010  normal-physics          free           0/32     0/ 187gb      
```

To get information about a particular node, you can use `pbsnodes` but on its own it is a firehose. Using it with a particular node name is more effective:

``` bash
[z1234567@katana2 src]$ pbsnodes k254
k254
    Mom = k254
    ntype = PBS
    state = job-busy
    pcpus = 32
    jobs = 313284.kman.restech.unsw.edu.au/0, 313284.kman.restech.unsw.edu.au/1, 313284.kman.restech.unsw.edu.au/2 
    resources_available.arch = linux
    resources_available.cpuflags = avx,avx2,avx512bw,avx512cd,avx512dq,avx512f,avx512vl
    resources_available.cputype = skylake-avx512
    resources_available.host = k254
    resources_available.mem = 196396032kb
    resources_available.ncpus = 32
    resources_available.node_weight = 1
    resources_available.normal-all = Yes
    resources_available.normal-qmchda = Yes
    resources_available.normal-qmchda-maths_business-maths = Yes
    resources_available.normal-qmchda-maths_business-maths-general = Yes
    resources_available.vmem = 198426624kb
    resources_available.vnode = k254
    resources_available.vntype = compute
    resources_assigned.accelerator_memory = 0kb
    resources_assigned.hbmem = 0kb
    resources_assigned.mem = 50331648kb
    resources_assigned.naccelerators = 0
    resources_assigned.ncpus = 32
    resources_assigned.ngpus = 0
    resources_assigned.vmem = 0kb
    resv_enable = True
    sharing = default_shared
    last_state_change_time = Thu Apr 30 08:06:23 2020
    last_used_time = Thu Apr 30 07:08:25 2020
```

## Managing Jobs on Katana

Once you have jobs running, you will want visibility of the system so that you can manage them - delete jobs, change jobs, check that jobs are still running.

There are a couple of easy to use commands that help with this process.

<!---
The following is a set of "content tabs" (see https://squidfunk.github.io/mkdocs-material/reference/content-tabs)
HTML heading tags are used instead of '###' otherwise the right sidebar index breaks
-->
!!! note "Job Commands"
    === "qstat"
        <h3>Show all jobs on the system</h3>
        `qstat` gives very long output. Consider piping to `less`

        ``` bash

            [z1234567@katana2 ~]$ qstat | less
            Job id            Name             User              Time Use S Queue
            ----------------  ---------------- ----------------  -------- - -----
            245821.kman       s-m20-i20-200h   z1234567                 0 Q medicine200
            280163.kman       Magcomp25A2      z1234567          3876:18: R mech700
            282533.kman       Proj_MF_Nu1      z1234567          3280:08: R cosmo200
            284666.kman       Proj_BR_Nu1      z1234567          3279:27: R cosmo200
            308559.kman       JASASec55        z1234567          191:21:3 R maths200
            309615.kman       2020-04-06.BUSC  z1234567          185:00:5 R babs200
            310623.kman       Miaocyclegan     z1234567          188:06:3 R simigpu200
            ...
        ```

        <h3>List just my jobs</h3>

        You can use either your **ZID** or the [Environment Variable](../../help_support/glossary#environment-variable) `$USER`

        ``` bash
            [z2134567@katana2 src]$ qstat -u $USER
            kman.restech.unsw.edu.au: 
                                                                        Req'd  Req'd   Elap
            Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
            --------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
            315230.kman.res z2134567 general1 job.pbs       --    1   1    1gb 01:00 Q   -- 
        ```

        If you add the `-s` flag, you will get slightly more status information.

        ``` bash
        [z1234567@katana2 src]$ qstat -su z1234567

        kman.restech.unsw.edu.au: 
                                                                    Req'd  Req'd   Elap
        Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
        --------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
        315230.kman.res z1234567 general1 job.pbs     61915   1   1    1gb 01:00 R 00:03
        Job run at Fri May 01 at 14:28 on (k019:mem=1048576kb:ncpus=1:ngpus=0)
        315233.kman.res z1234567 general1 job.pbs       --    1   1    1gb 01:00 Q   --
            -- 
        ```

        <h3>List information about a particular job</h3>
        ``` bash
        [z1234567@katana2 src]$ qstat -f 315236                                                                                                                                       
        Job Id: 315236.kman.restech.unsw.edu.au                                                                                                                                       
            Job_Name = job.pbs                                                                                                                                                        
            Job_Owner = z1234567@katana2
            job_state = Q
            queue = general12
            server = kman.gen
            Checkpoint = u
            ctime = Fri May  1 14:41:00 2020
            Error_Path = katana2:/home/z1234567/src/job.pbs.e315236
            group_list = GENERAL
            Hold_Types = n
            Join_Path = n
            Keep_Files = n
            Mail_Points = a
            mtime = Fri May  1 14:41:00 2020
            Output_Path = katana2:/home/z1234567/src/job.pbs.o315236
            Priority = 0
            qtime = Fri May  1 14:41:00 2020
            Rerunable = True
            Resource_List.ib = no
            Resource_List.mem = 1gb
            Resource_List.ncpus = 1
            Resource_List.ngpus = 0
            Resource_List.nodect = 1
            Resource_List.place = pack
            Resource_List.select = 1:mem=1gb:ncpus=1
            Resource_List.walltime = 01:00:00
            substate = 10
            Variable_List = PBS_O_HOME=/home/z1234567,PBS_O_LANG=en_AU.UTF-8,
                PBS_O_LOGNAME=z1234567,
                PBS_O_PATH=/home/z1234567/bin:/usr/lib64/qt-3.3/bin:/usr/lib64/ccache:
                /usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/pbs/bin,PBS_O_M
                AIL=/var/spool/mail/z1234567,PBS_O_SHELL=/bin/bash,PBS_O_WORKDIR=/home
                /z1234567/src,PBS_O_SYSTEM=Linux,PBS_O_QUEUE=submission,PBS_O_HOST=kat
                ana2
            etime = Fri May  1 14:41:00 2020
            eligible_time = 00:00:00
            Submit_arguments = -W group_list=GENERAL -N job.pbs job.pbs.JAZDNgL
            project = _pbs_project_default
        ```
    === "qdel"
        Remove a job from the queue or kill it if it's started. To remove an array job, you must include the square braces and they will need to be escaped. In that situation you use `qdel 12345\[\]`. Uses the `$JOBID` 

        ``` bash
        [z1234567@katana2 src]$ qdel 315252
        ```


    === "qalter"
        Once a job has been submitted, it can be altered. However, once a job begins execution, the only values that can be modified are `cputime`, `walltime`, and `run_count`. These can only be reduced.

        Users can only lower resource requests on queued jobs. If you need to increase resources, contact a systems administrator. In this example you will see the resources change - but not the `Submit_arguments`

        ``` bash
        [z1234567@katana2 src]$ qsub -l select=1:ncpus=2:mem=128mb job.pbs
        315259.kman.restech.unsw.edu.au
        [z1234567@katana2 src]$ qstat -f 315259
        Job Id: 315259.kman.restech.unsw.edu.au
            ...
            Resource_List.mem = 128mb
            Resource_List.ncpus = 2
            ...
            Submit_arguments = -W group_list=GENERAL -N job.pbs -l select=1:ncpus=2:mem=128mb job.pbs.YOOu3lB
            project = _pbs_project_default
            
        [z1234567@katana2 src]$ qalter -l select=1:ncpus=4:mem=512mb 315259; qstat -f 315259
        Job Id: 315259.kman.restech.unsw.edu.au
            ...
            Resource_List.mem = 512mb
            Resource_List.ncpus = 4
            ...
            Submit_arguments = -W group_list=GENERAL -N job.pbs -l select=1:ncpus=2:mem=128mb job.pbs.YOOu3lB
            project = _pbs_project_default
        ```

## Job Stats

As soon as your job finishes, PBS produces job statistics along with a summary of your job. This summary appears as follows (replace ```4638435.kman.restech.unsw.edu.au.OU``` for your output file; the steps for retrieving the file name are outlined below):
```bash
z123456@katana2:~ $ cat 4638435.kman.restech.unsw.edu.au.OU

================================================================================
                     Resource Usage on 27/07/2023 15:43:37

Job Id: 4638435
Queue: CSE
Walltime: 00:00:03  (requested 01:00:00)
Job execution was successful. Exit Status 0.

--------------------------------------------------------------------------------
|              |             CPUs              |            Memory             |
--------------------------------------------------------------------------------
| Node         | Requested   Used   Efficiency | Requested   Used   Efficiency |
| k080         |     1        0.0      0.0%    |   1.0gb    0.01gb     1.0%    |
--------------------------------------------------------------------------------
```
This text is appended at the end of your output file. If you haven't specified an output file, one will be generated for you and placed in the folder where you submit your job.

If you're unsure about the location of your job statistics file, you can run the following command (replace 4682962 with your job ID):
```bash
z123456@katana2:~ $ qstat -xf 4682962
```
then search for Output_Path. For example, in below example, the absolute path of the output file is ```/home/z123456/output_file.txt```. Interactive jobs do no show a file name, for those cases the output file name look as ```4638435.kman.restech.unsw.edu.au.OU``` and is placed on the folder where you submitted the job:
```bash
z3536424@katana2:~/hacky $ qstat -xf 4686875
Job Id: 4686875.kman.restech.unsw.edu.au
    Job_Name = hacky
    Job_Owner = z3536424@katana2
    resources_used.cpupercent = 0
    resources_used.cput = 00:00:00
    resources_used.mem = 6896kb
    resources_used.ncpus = 1
    resources_used.vmem = 6896kb
    resources_used.walltime = 00:00:09
    job_state = F
    queue = cse12
    server = kman
    Checkpoint = u
    ctime = Thu Aug 10 16:10:39 2023
    Error_Path = /dev/pts/0
    exec_host = k242/3
    exec_vnode = (k242:ncpus=1:mem=1048576kb:ngpus=0)
    Hold_Types = n
    interactive = True
    Join_Path = n
    Keep_Files = n
    Mail_Points = a
    mtime = Thu Aug 10 16:11:09 2023
    Output_Path = /home/z123456/output_file.txt
    Priority = 0
    qtime = Thu Aug 10 16:10:39 2023
    Rerunable = False
    Resource_List.ib = no
    Resource_List.mem = 1gb
    Resource_List.ncpus = 1
    Resource_List.ngpus = 0
    Resource_List.nodect = 1
    Resource_List.place = group=cse12
    Resource_List.select = ncpus=1:mem=1gb
    Resource_List.walltime = 01:00:00
    stime = Thu Aug 10 16:10:55 2023
    obittime = Thu Aug 10 16:11:09 2023
    session_id = 2621006
    jobdir = /home/z3536424
    substate = 92
    Variable_List = PBS_O_HOME=/home/z3536424,PBS_O_LANG=en_AU.UTF-8,
	PBS_O_LOGNAME=z3536424,
	PBS_O_PATH=/home/z3536424/.local/bin:/usr/share/Modules/bin:/usr/lib64
	/ccache:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/opt/pbs/bin,
	PBS_O_MAIL=/var/spool/mail/z3536424,PBS_O_SHELL=/bin/bash,
	PBS_O_WORKDIR=/home/z3536424/hacky,PBS_O_SYSTEM=Linux,
	PBS_O_QUEUE=cse12,PBS_O_HOST=katana2
    comment = Job run at Thu Aug 10 at 16:10 on (k242:ncpus=1:mem=1048576kb:ngp
	us=0) and finished
    etime = Thu Aug 10 16:10:39 2023
    run_count = 1
    eligible_time = 00:00:19
    Exit_status = 0
    Submit_arguments = -N hacky -I
    history_timestamp = 1691647869
    project = _pbs_project_default
    Submit_Host = katana2
```

