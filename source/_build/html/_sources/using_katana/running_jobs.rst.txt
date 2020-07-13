.. _running_jobs:

######################
Running Jobs on Katana
######################

**************
Brief Overview
**************

The :term:`Login Node` of a cluster is a shared resource for all users and is used for preparing, submitting and managing jobs. Never run any computationally intensive processes on the login nodes. Jobs are submitted from the login node, which delivers them to the :term:`Head Node` for job and resource management. Once the resources have been allocated and are available, the job will run on one or more of the compute nodes as requested. 

Different clusters use different tools to manage resources and schedule jobs - OpenPBS_ and SLURM_ are two popular systems. Katana, like NCI's Gadi, uses OpenPBS_ for this purpose.

Jobs are submitted using the :code:`qsub` command. There are two types of job that :code:`qsub` will accept: an :term:`Interactive Job` and a :term:`Batch Job`. Regardless of type, the resource manager will put your job in a :term:`Queue`.

An **interactive job** provides a shell session on a :term:`Compute Nodes`. You interact directly with the compute node running the software you need explicitly. Interactive jobs are useful for experimentation, debugging, and planning for **batch jobs**. 

In contrast, a :term:`Batch Job` is a scripted job that - after submission via :code:`qsub` - runs from start to finish without any user intervention. The vast majority of jobs on the cluster are batch jobs. This type of job is appropriate for production runs that will consume several hours or days. 

To submit a :term:`Batch Job` you will need to create a job script which specifies the resources that your job requires and calls your program. The general structure of `A Job Script`_ is shown below.

.. important::
    All jobs go into a :term:`Queue` while waiting for resources to become available. The length of time your jobs wait in a queue for resources depends on a number of factors.

The main resources available for use are Memory (RAM), :term:`CPU Core` (number of CPUs) and :term:`Walltime` (how long you want the CPUs for). These need to be considered carefully when writing your job script, since the decisions you make will impact which queue your jobs ends up on.

As you request more memory, the number of available queues goes down. The memory limits, in GB, at which the number of queues decreases are 124, 180, 248, 370, 750 and 1000.

Similarly, when considering the number of CPU cores, the available resources reduce at 16, 20, 24, 28, 32, 44 and 64 CPU cores.

Walltime provides the biggest constraint on your submitted jobs. The points at which resource availability is reduced are 12, 48, 100 and 200 hours

Jobs have the following restrictions:

-  Jobs of up to 12 hours can run anywhere on the cluster
-  Jobs of up to 100 hours can run on nodes belonging to your group and the general nodes
-  Jobs of up to 200 hours can only run on nodes belonging to your group

.. _interactive_job:
.. _interactive_session:

****************
Interactive Jobs
****************

An interactive job or interactive session is a session on a compute node with the required physical resources for the period of time requested. To request an interactive job, add the -I flag (capital i) to :code:`qsub`. Default sessions will have 1 CPU core, 1GB and 1 hour

For example, the following two commands. The first provides a default session, the second provides a session with two CPU core and 8GB memory for three hours. You can tell when an interactive job has started when you see the name of the server change from :code:`katana1` or :code:`katana2` to the name of the server your job is running on. In these cases it's :code:`k181` and :code:`k201` respectively.

.. code-block:: bash

    [z1234567@katana1 ~]$ qsub -I
    qsub: waiting for job 313704.kman.restech.unsw.edu.au to start
    qsub: job 313704.kman.restech.unsw.edu.au ready
    [z1234567@k181 ~]$ 

.. code-block:: bash

    [z1234567@katana2 ~]$ qsub -I -l select=1:ncpus=2:mem=8gb,walltime=3:00:00
    qsub: waiting for job 1234.kman.restech.unsw.edu.au to start
    qsub: job 1234.kman.restech.unsw.edu.au ready
    [z1234567@k201 ~]$ 

Jobs are constrained by the resources that are requested. In the previous example the first job - running on :code:`k181` - would be terminated after 1 hour or if a command within the session consumed more than 8GB memory. The job (and therefore the session) can also be terminated by the user with :code:`CTRL-D` or the :code:`logout` command.

Interactive jobs can be particularly useful while developing and testing code for a future batch job, or performing an interactive analysis that requires significant compute resources. Never attempt such tasks on the login node -- submit an interactive job instead.

.. _batch_jobs:

**********
Batch Jobs
**********

A batch job is a script that runs autonomously on a compute node. The script must contain the necessary sequence of commands to complete a task independently of any input from the user. This section contains information about how to create and submit a batch job on Katana.

Getting Started
===============

The following script simply executes a pre-compiled program ("myprogram") in the user's home directory:

.. code-block:: bash
    
    #!/bin/bash
 
    cd $HOME
 
    ./myprogram

This script can be submitted to the cluster with :code:`qsub` and it will become a job and be assigned to a queue. If the script is in a file called :code:`myjob.pbs` then the following command will submit the job with the default resource requirements (1 CPU core with 1GB of memory for 1 hour):

.. code-block:: bash

    [z1234567@katana1 ~]$ qsub myjob.pbs
    1237.kman.restech.unsw.edu.au

As with interactive jobs, the :code:`-l` (lowercase L) flag can be used to specify resource requirements for the job:

.. code-block:: bash

    [z1234567@katana ~]$ qsub -l select=1:ncpus=1:mem=4gb,walltime=12:00:00 myjob.pbs
    1238.kman.restech.unsw.edu.au

A Job Script
============

Job scripts offer a much more convenient method for invoking any of the options that can be passed to :code:`qsub` on the command-line. In a shell script, a line starting with # is a comment and will be ignored by the shell interpreter. However, in a job script, a line starting with #PBS can be used to pass options to the :code:`qsub` command.

Here is an overview of the different parts of a job script which we will examine further below. In the following sections we will add some code, explain what it does, then show some new code, and iterate up to something quite powerful.

For the previous example, the job script could be rewritten as:

.. code-block:: bash 

    #!/bin/bash
 
    #PBS -l select=1:ncpus=1:mem=4gb
    #PBS -l walltime=12:00:00
     
    cd $HOME
     
    ./myprogram

This structure is the most common that you will use. The top line must be :code:`#!/bin/bash` - we are running bash scripts, and this is required.
The following section - the lines starting with :code:`#PBS` - are where we will be configuring how the job will be run - here we are asking for resources.
The final section shows the commands that will be executed in the configured session.

The script can now be submitted with much less typing:

.. code-block:: bash

    [z1234567@katana ~]$ qsub myjob.pbs
    1239.kman.restech.unsw.edu.au

Unlike submission of an interactive job, which results in a session on a compute node ready to accept commands, the submission of a batch job returns the ID of the new job. This is confirmation that the job was submitted successfully. The job is now processed by the job scheduler and resource manager. Commands for checking the status of the job can be found in the section :ref:`Managing Jobs on Katana`.

If you wish to be notified by email when the job finishes then use the :code:`-M` flag to specify the email address and the :code:`-m` flag to declare which events cause a notification. Here we will get an email if the job aborts (:code:`-m a`) due to an error or ends (:code:`-m e`) naturally. 

.. code-block:: bash

    #PBS -M your.name.here@unsw.edu.au
    #PBS -m ae

The output that would normally go to screen and error messages of a batch job will be saved to file when your job ends. By default these files will be called :code:`JOB_NAME.oJOB_ID` and :code:`JOB_NAME.eJOB_ID`, and they will appear in the directory that was the current working directory when the job was submitted. In the above example, they would be :code:`myjob.o1239` and :code:`myjob.e1239`.  You can merge these into a single file with the :code:`-j oe` flag. The :code:`-o` flag allows you to rename the file.

.. code-block:: bash

    #PBS -j oe
    #PBS -o /home/z1234567/results/Output_Report

When a job starts, it needs to know where to save it's output and do it's work. This is called the *current working directory*. By default the job scheduler will make your *current working directory* your home directory (:code:`/home/z1234567`). This isn't likely or ideal and is important that each job sets its current working directory appropriately. There are a couple of ways to do this, the easiest is to set the *current working directory* to the directory you are in when you execute :code:`qsub` by using

.. code-block:: bash

    cd $PBS_O_WORKDIR

There is one last special variable you should know about, especially if you are working with large datasets. The storage on the compute node your job is running on will always be faster than the network drive.

If you use the storage close to the CPUs - in the server rather than on the shared drives, called :term:`Local Scratch` - you can often save hours of time reading and writing across the network. 

In order to do this, you can copy data to and from the local scratch, called :code:`$TMPDIR`:

.. code-block:: bash

    cp /home/z1234567/project/massivedata.tar.gz $TMPDIR
    tar xvf massivedata.tar.gz
    my_analysis.py massive_data
    cp -r $TMPDIR/my_output /home/z1234567


There are a lot of things that can be done with PBSPro, but you don't and won't need to know it all. These few basics will get you started. 

Here's the full script as we've described. You can copy this into a text editor and once you've changed our dummy values for yours, you only need to change the last line.

.. code-block:: bash

    #!/bin/bash
 
    #PBS -l select=1:ncpus=1:mem=4gb
    #PBS -l walltime=12:00:00
    #PBS -M your.name.here@unsw.edu.au
    #PBS -m ae
    #PBS -j oe
    #PBS -o /home/z1234567/results/Output_Report
     
    cd $PBS_O_WORKDIR
     
    ./myprogram


.. _array_jobs:

**********
Array Jobs
**********

One common use of computational clusters is to do the same thing multiple times - sometimes with slightly different input, sometimes to get averages from randomness within the process. This is made easier with array jobs.

An array job is a single job script that spawns many almost identical sub-jobs. The only difference between the sub-jobs is an environment variable :code:`$PBS_ARRAY_INDEX` whose value uniquely identifies an individual sub-job. A regular job becomes an array job when it uses the :code:`#PBS -J` flag. 

For example, the following script will spawn 100 sub-jobs. Each sub-job will require one CPU core, 1GB memory and 1 hour run-time, and it will execute the same application. However, a different input file will be passed to the application within each sub-job. The first sub-job will read input data from a file called :code:`1.dat`, the second sub-job will read input data from a file called :code:`2.dat` and so on. 

.. note::
    In this example we are using `brace expansion`_ - the {} characters around the bash variables - because they are needed for variables that change, like array indices. They aren't strictly necessary for :code:`$PBS_O_WORKDIR` but we include them to show consistency.

.. code-block:: bash

    #!/bin/bash
     
    #PBS -l select=1:ncpus=1:mem=1gb
    #PBS -l walltime=1:00:00
    #PBS -j oe
    #PBS -J 1-100
     
    cd ${PBS_O_WORKDIR}
     
    ./myprogram ${PBS_ARRAY_INDEX}.dat

There are some more examples of array jobs including how to group your computations in an array job on the `UNSW Github HPC examples <https://github.com/unsw-edu-au/Restech-HPC/tree/master/hpc-examples>`_ page.

**************************
Splitting large Batch Jobs
**************************

If your batch job can be split into multiple steps you may want to split one big job up into a number of smaller jobs. There are a number of reasons to spend the time to implement this.

1. If your large job runs for over 200 hours, it won't finish on Katana.
2. If your job has multiple steps which use different amounts of resources at each step. If you have a pipeline that takes 50 hours to run and needs 200GB of memory for an hour, but only 50GB the rest of the time, then the memory is sitting idle. 
3. Katana has prioritisations based on how many resources any one user uses. If you ask for 200GB of memory, this will be accounted for when working out your next job's priority.
4. There's no other way to say this, but because there are more resources for 12 hour jobs, seven or eight 12 hour jobs will often finish well before a single 100 hour job even starts. 


.. _state_of_pbs:

************************************************
Get information about the state of the scheduler
************************************************

When deciding which jobs to run, the scheduler takes the following details into account:

- are there available resources
- how recently has this user run jobs successfully
- how many resources has this user used recently
- how long is the job's Walltime
- how long has the job been in the queue

You can get an overview of the compute nodes and a list of all the jobs running on each node using :code:`pstat`

.. code-block:: bash

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


To get information about a particular node, you can use :code:`pbsnodes` but on it's own it is a firehose. Using it with a particular node name is more effective:

.. code-block:: bash

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


.. _managing_jobs:

***********************
Managing Jobs on Katana
***********************

Once you have jobs running, you will want visibility of the system so that you can manage them - delete jobs, change jobs, check that jobs are still running.

There are a couple of easy to use commands that help with this process.

qstat
=====

.. _more_info_from_pbs:

Show all jobs on the system
---------------------------

:code:`qstat` gives very long output. Consider piping to :code:`less`

.. code-block:: bash

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

List just my jobs
-----------------

You can use either your **ZID** or the :term:`Environment Variable` :code:`$USER`

.. code-block:: bash

    [z2134567@katana2 src]$ qstat -u $USER
    kman.restech.unsw.edu.au: 
                                                                Req'd  Req'd   Elap
    Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
    --------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
    315230.kman.res z2134567 general1 job.pbs       --    1   1    1gb 01:00 Q   -- 


If you add the :code:`-s` flag, you will get slightly more status information.

.. code-block:: bash

    [z1234567@katana2 src]$ qstat -su z1234567

    kman.restech.unsw.edu.au: 
                                                                Req'd  Req'd   Elap
    Job ID          Username Queue    Jobname    SessID NDS TSK Memory Time  S Time
    --------------- -------- -------- ---------- ------ --- --- ------ ----- - -----
    315230.kman.res z1234567 general1 job.pbs     61915   1   1    1gb 01:00 R 00:03
       Job run at Fri May 01 at 14:28 on (k019:mem=1048576kb:ncpus=1:ngpus=0)
    315233.kman.res z1234567 general1 job.pbs       --    1   1    1gb 01:00 Q   --
        -- 

List information about a particular job
---------------------------------------

.. code-block:: bash

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


qdel
====

Remove a job from the queue or kill it if it's started. To remove an array job, you must include the square braces and they will need to be escaped. In that situation you use :code:`qdel 12345\[\]`. Uses the :code:`$JOBID` 

.. code-block:: bash

    [z1234567@katana2 src]$ qdel 315252


qalter
======
    
Once a job has been submitted, it can be altered. However, once a job begins execution, the only values that can be modified are :code:`cputime`, :code:`walltime`, and :code:`run_count`. These can only be reduced.

Users can only lower resource requests on queued jobs. If you need to increase resources, contact a systems administrator. In this example you will see the resources change - but not the :code:`Submit_arguments`

.. code-block:: bash

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


.. _scheduler_tips:

*****************************************
Tips for using PBS and Katana effectively
*****************************************

Keep your jobs under 12 hours if possible
=========================================

If you request more than 12 hours of :code:`WALLTIME` then you can only use the nodes bought by your school or research group. Keeping your job's run time request under 12 hours means that it can run on any node in the cluster.

.. important::
    Two 10 hour jobs will probably finish sooner that one 20 hour job.

In fact, if there is spare capacity on Katana, which there is most of the time, six 10 hours jobs will finish before a single 20 hour job will.
Requesting more resources for your job decreases the places that the job can run

The most obvious example is going over the 12 hour limit which limits the number of compute nodes that your job can run on but it is worth . For example specifying the CPU in your job script restricts you to the nodes with that CPU. A job that requests 20Gb will run on a 128Gb node with a 100Gb job already running but a 30Gb job will not be able to.

Running your jobs interactively makes it hard to manage multiple concurrent jobs
================================================================================

If you are currently only running jobs interactively then you should move to batch jobs which allow you to submit more jobs which then start, run and finish automatically.
If you have multiple batch jobs that are almost identical then you should consider using array jobs

If your batch jobs are the same except for a change in file name or another variable then you should have a look at using array jobs.



.. _OpenPBS: https://www.pbspro.org/
.. _SLURM: https://slurm.schedmd.com/ 
.. _`brace expansion`: https://www.gnu.org/software/bash/manual/html_node/Brace-Expansion.html
