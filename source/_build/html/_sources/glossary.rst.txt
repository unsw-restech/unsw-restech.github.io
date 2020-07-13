

========
Glossary
========

.. glossary::

    Active Job 
        Active jobs are jobs that have been assigned to a compute node and are currently running. These can be seen by running :code:`qstat` and looking for an A in the second last column. See :ref:`more_info_from_pbs`

    Array Job
        If you want to run the same job multiple times with slight differences (filenames, data source, etc), then you can create an array job which will submit multiple jobs for you from the one job script. 

    Batch Job
        A batch job is a job on a cluster that runs without any further input once it has been submitted. Almost all jobs on the cluster are batch jobs. All jobs are either batch jobs or :term:`Interactive Job`.

    Blade 
        Some of the compute nodes Katana are called blade servers which allow a higher density of servers in the same space. Each blade consists of multiple CPUs with 6 or more cores.

    Cluster
        A computer cluster is a set of connected computers that work together so that, in many respects, they can be viewed as a single system. Using a cluster is referred to as High Performance Computing or HPC. Most will have a :term:`Management Plane` and several :term:`Compute Nodes`.

    Compute Nodes
        The compute nodes are where the compute jobs run. Users submit jobs from the :term:`Login Node` and the :term:`Job Scheduler` on the :term:`Head Node` will assign the job to one or more compute nodes.

    CPU Core
        Each node in the cluster has one or more CPUs each of which has 6 or more cores. Each core is able to run one job at a time so a node with 12 cores could have 12 jobs running in parallel.

    Data Transfer Node
        The Data Transfer Node, also known as the :ref:`Katana Data Mover` (KDM), is a server that is used for transferring files to, from, and within the cluster. Due to the nature of moving data around, it uses a significant amount of memory and network bandwidth. This server is used to take that load off the :term:`Login Node`.

    Environment Variable 
        Environment variables are variables that are set in Linux to tell applications where to find programs and set program options. They will start with a $ symbol. For example, all users can reference :code:`$TMPDIR` in their :term:`Job Script` in order to use :term:`Local Scratch`

    Global Scratch 
        Global scratch is a large data store for data that isn't backed up. It differs from local scratch in that it is available from every node including the :term:`Head Node`. If you have data files or working directories this is where you should put them.

    Head Node
        The head node of the :term:`Cluster` is the computer that manages job and resource management. This is where the :term:`Job Scheduler` and :term:`Resource Manager` run. It is kept separate from the :term:`Login Node` so that production doesn't stop if someone accidentally breaks the :term:`Login Node`.

    Held Jobs
        Held jobs are jobs that cannot currently run. They are put into that state by either the server or the system administrator. Jobs stay held until released by a systems administrator, at which point they become :term:`Queued Jobs`. These can be seen by running :code:`qstat` and looking for an H in the second last column. See :ref:`more_info_from_pbs`

    Interactive Job 
        An interactive job is a way of testing your program and data on a cluster without negatively impacting the :term:`Login Node`. Once a request has been submitted and accepted for an interactive job, the user will no longer be on the relatively small login nodes, and will have access to the resources requested on the :term:`Compute Nodes`. In other words, your terminal session will move from a small (virtual) computer you share with many people to a large computer you share with very few people. All jobs are either a :term:`Batch Job` or an interactive job. Instructions on using :ref:`interactive_job`
    
    Job Scheduler
        The job scheduler monitors the jobs currenty running on the cluster and assigns :term:`Queued Jobs` to :term:`Compute Nodes` based on recent cluster useage, job resource requirements and nodes available to the research group of the submitter. In summary the job scheduler determines when and where a job should run. The job scheduler that we use is called PBSPro.

    Job Script
        A job script is a file containing all of the information needed to run a :term:`Batch Job` including the resource requirements and the actual commands to run the job.

    Local Scratch 
        Local scratch refers to the storage available internally on each compute node. Of all the different scratch directories this storage has the best performance however you will need to move your data into local scratch as part of your job script. You can use local scratch with the :term:`Environment Variable` :code:`$TMPDIR`

    Login Node
        The login nodes of the cluster is the computer that you log in to when you connect to the cluster. This node is used to compile software and submit jobs.

    Module
        The module command is a means of providing access to different versions of software without risking version conflicts across multiple users.

    Management Plane
        The Management Plane is the set of servers that sit above or adjacent to the :term:`Compute Nodes`. These servers are used to manage the system, manage the storage, or manage the network. User's have access to the :term:`Login Node` and :term:`Data Transfer Node`. Other servers include the :term:`Head Node`. 

    MPI
        Message Passing Infrastructure (MPI) is a technology for running a :term:`Batch Job` on more than one :term:`Compute Nodes`. Designed for situations where parts of the job can run on independent nodes with the results being transferred to other nodes for the next part of the job to be run.

    Network Drive 
        A network drive is a drive that is independant from the cluster. 

    Queue
        All submitted jobs are put into a queue. Each queue has a collection of resources available to it. As those resources become available, new jobs will be assigned to those resources. Job prioritisation is done by the scheduler and depends on a number of factors including length of wait time and total resource use by user over the previous month.

    Queued Jobs 
        Queued jobs are eligible to run but are waiting for a :term:`Compute Nodes` that matches their requirements to become available. Which idle job will be assigned to a compute node next depends on the :term:`Job Scheduler`. These can be seen by running :code:`qstat` and looking for a Q in the second last column. See :ref:`more_info_from_pbs`

    Resource Manager 
        A resource manager works with the :term:`Job Scheduler` to manage running jobs on a cluster. Amongst other tasks it receives and parses job submissions, starts jobs on :term:`Compute Nodes`, monitors jobs, kills jobs, and manages how many :term:`CPU Core` are available on each :term:`Compute Nodes`

    Scratch Space 
        Scratch space is a non backed up storage area where users can store transient data. It should not be used for job code as it is not backed up.

    Walltime
        In HPC, walltime is the amount of time that you will be allocated when your job runs. If your jobs runs longer than the walltime, it will be killed by the :term:`Job Scheduler`. It is used by the scheduler for helping allocate resources onto servers. On **Katana** it is also used to determine which :term:`Queue` your job will end up in. The shorter the walltime, the more opportunity your job has to run which in turn means that it will start sooner. In short - it's harder to find 100 hours of space than it is to find 12 hours of space.
