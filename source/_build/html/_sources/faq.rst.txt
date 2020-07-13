##########################
Frequently Asked Questions
##########################

***********
General FAQ
***********

Where is the best place to store my code?
=========================================

The best place to store source code is to use version control and store it in a repository.  This means that you will be able to keep every version of your code and revert to an earlier version if you require. `UNSW has a central github account <https://research.unsw.edu.au/github>`_, but we encourage you to create your own.

I just got some money from a grant. What can I spend it on?
===========================================================

There are a number of different options for using research funding to improve your ability to run computationally intensive programs. The best starting point is to :ref:`contact_us` to figure out the different options.  

Can I access Katana from outside UNSW?
======================================

Yes, if you have an account then you can connect to Katana from both inside and outside UNSW. Some services - like remote desktops - will not be as responsive as inside the UNSW network.

.. _katana_compute_faq:

*************
Scheduler FAQ
*************

Does Katana run a 32 bit or a 64 bit operating system?
======================================================

Katana :term:`Compute Nodes` run a 64 bit version of the CentOS distribution of Linux. Currently version 7.8. The :term:`Head Node` runs RedHat 7.8.

How much memory is available per core and/or per node?
======================================================

The amount of memory available varies across the cluster. To determine how much memory each node has available use the 'pbsnodes' command. Roughly, you can safely use 4GB per core requested. You can request more memory but it may delay time spent in the queue.

How much memory can I use on the login node for compiling software?
===================================================================

The login nodes have a total of 24GB of memory each. Each individual user is limited to 4GB and should only be used to compile software. If you need more, do it in an :term:`Interactive Job`.

Why isn't my job making it onto a node even though it says that some nodes are free?
====================================================================================

There are three main reasons you will see this behavior. The first of them is specific to Katana and the other two apply to any cluster.

Firstly, the compute nodes in Katana belong to various schools and research groups across UNSW. Any job with an expected run-time longer than 12 hours can only run on a compute node that is somehow associated with the owner of the job. For example, if you are in the CCRC you are entitled to run 12+ hour jobs on the General nodes and the nodes jointly purchased by CCRC. However, you cannot run 12+ hour jobs on the nodes purchased by Astrobiology, Statistics, TARS, CEPAR or Physics. So you may see idle nodes, but you may not be entitled to run a 12+ hour job on them.

Secondly, the idle nodes may not have sufficient resources for your job. For example, if you have asked for 100GB memory but there are only 50GB free on the "idle node".

Thirdly, there may be distributed memory jobs ahead of your job in the queue which have reservations on the idle nodes, and they are just waiting for all of their requested resources to become available. In this case, your job can only use the reserved nodes if your job can finish before the nodes are required by the distributed memory job. For example, if a job has been waiting a week (yes, it happens) for :code:`walltime=200,cpu=88,mem=600GB` (very long, two whole nodes), then those resources will need to be made available at some point. This is an excellent example of why breaking your jobs up into smaller parts is good HPC practice.

How many jobs can I submit at the one time?
===========================================

Technically you can submit as many jobs as you wish. The queuing system run by the scheduler is designed to prevent a single user flooding the system - each job will reduce the priority of your next jobs. In this way the infrequent users get a responsive system without impacting the regular users too much.

Whilst there is not a technical limit to the number of jobs you can submit, submitting more that 2,000 jobs at the one time can place an unacceptable load on the job scheduler and your jobs may be deleted without warning. This is an editorial decision by management.

What is the maximum number of CPUs I can use in parallel?
=========================================================

As many as your account and queue will allow you. But there are trade-offs - if you ask for 150 CPUs (~5 full servers) you might be waiting more than a couple of months for your job to run. 

If you are regularly wanting to run large parallel jobs (16+ cores per job) on Katana you should consider speaking to :ref:`help_and_support` so that they are aware of your jobs. They may be able to provide you additional assistance on resource usage for parallel jobs. 

Why does my SSH connection periodically dsconnect?
==================================================

With all networks there is a limit to how long a connection between two computers will stay open if no data is travelling between them. Look to set your ServerAliveInterval or Keep Alive interval to 60 in your secure shell software (putty, ssh). 

Can I change the job script after it has been submitted?
========================================================

Yes you increase the resource values for jobs that are still queued, but even then you are constrained by the limits of the particular queue that you are submitting to. Once it has been assigned to a node the intricacies of the scheduling policy means that it becomes impossible for anyone including the administrator to make any further changes

Where does Standard Output (STDOUT) go when a job is run?
=========================================================

By default Standard Output is redirected to storage on the node and then transferred when the job is completed. If you are generating data you should redirect :code:`STDOUT` to a different location. The best location depends on the characteristics of your job but in general all :code:`STDOUT` should be redirected to local scratch.

How do I figure out what the resource requirements of my job are?
=================================================================

The best way to determine the resource requirements of your job is to be generous with the resource requirements on the first run and then refine the requirements based on what the job actually used. If you put the following information in your job script you will receive an email when the job finishes which will include a summary of the resources used.

.. code-block:: bash 

    #PBS -M z1234567@unsw.edu.au 
    #PBS -m ae

Can I cause problems to other users if I request too many resources or make a mistake with my job script?
=========================================================================================================

Yes, but it's extremely unlikely. We used to say no, but that's not strictly true. The reality is that if something breaks it's usually your job hitting the odd corner case we didn't account for. It doesn't happen often.

Will a job script from another cluster work on cluster X?
=========================================================

It depends on a number of factors including the sceduling software. Some aspects are fairly common across different clusters (e.g. walltime) others are not. You should look at the cluster specific information to see what queuing system is being used on that cluster and what commands you will need to change. You wont find a cluster that doesn't have knowledgable support that can help you migrate.

How can I see exactly what resources (I/O, CPU, memory and scratch) my job is currently using?
==============================================================================================

From *outside* the job, you can run :code:`qstat -f <jobid>`. 

If, for instance, you wanted to measure different steps of your process, then inside your jobscript you can put :code:`qstat -f $PBS_JOBID`

For fine grain detail, you may need to get access to the worker node that the job is running on:

.. code-block:: bash 

    qstat -nru $USER

then you can see a list of your running jobs and where they are running. You can then use ssh to log on to the individual nodes and run :code:`top` or :code:`htop` to see the load on the node including memory usage for each of the processes on the node.

How do I request the installation or upgrade of a piece of software ?
=====================================================================

If you wish to have a new piece of software installed or software that is already installed upgraded please send an email to the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_ from your UNSW email account with details of what software change you require and the cluster that you would like it changed on.

Why is my job stuck in the queue whilst other jobs run?
=======================================================

The queues are not set up to be first-in-first-out. In fact all of the queued jobs sit in one big pool of jobs that are ready to run. The scheduler assigns priorities to jobs in the pool and the job with the highest priority is the next one to run. The length of time spent waiting in the pool is just one of several factors that are used to determine priority.

For example, people who have used the cluster heavily over the last two weeks receive a negative contribution to their jobs' priority, whereas a light user will receive a positive contribution. You can see this in action with the diagnose -p and diagnose -f commands.

You mentioned waiting time as a factor, what else affects the job priority?
===========================================================================

The following three factors combine to generate the job priority.

- How many resources (cpu and memory) have you and your group consumed in the last 14 days? Your personal consumption is weighted more highly than your group's consumption. Heavy recent usage contributes a negative priority. Light recent usage contributes a positive priority.
- How many resources does the job require? Always a positive contribution to priority, but increases linearly with the amount of cpu and memory requested, i.e. we like big jobs.
- How long has the job been waiting in the queue? Always a positive contribution to priority, but increases linearly with the amount of time your job has been waiting in the queue. Note that throttling policies will prevent some jobs from being considered for scheduling, in which case their clock does not start ticking until that throttling constraint is lifted.

What happens if my job uses more memory than I requested?
=========================================================

The job will be killed by the scheduler. You will get a message to that effect if you have any types of notification enabled (logs, emails).

What happens if my job is still running when it reaches the end of the time that I have requested?
==================================================================================================

When your job hits it's :term:`Walltime` it is automatically terminated by the scheduler.

200 hours is not long enough! What can I do?
============================================

If you find that your jobs take longer than the maximum WALL time then there are several different options to change your code so that it fits inside the parameters.

- Can your job be split into several independent jobs?
- Can you export the results to a file which can then be used as input for the next time the job is run?

You may want to also look to see if there is anything that you can do to make your code run better like making better use of local scratch if your code is I/O intensive.

Do sub-jobs within an array job run in parallel, or do they queue up serially?
==============================================================================

Submitting an array job with 100 sub-jobs is equivalent to submitting 100 individual jobs. So if sufficient resources are available then all 100 sub-jobs could run in parallel. Otherwise some sub-jobs will run and other sub-jobs must wait in the queue for resources to become available.

The '%' option in the array request offers the ability to self impose a limit on the number of concurrently running sub-jobs. Also, if you need to impose an order on when the jobs are run then the 'depend' attribute can help.

In a pbs file does the MEM requested refer to each node or the total memory on all nodes being used (if I am using more than 1 node?
=====================================================================================================================================

MEM refers to the amount of memory per node.

.. _storage_faq:

***********
Storage FAQ
***********

What storage is available to me?
================================

Katana provides three different storage areas, cluster home drives, local scratch and global scratch. The storage page has additional information on the differences and advantages of each of the different types of storage. You may also want to consider storing your code using a version control service like GitHub. This means that you will be able to keep every version of your code and revert to an earlier version if you require.

Which storage is fastest?
=========================

In order of performance the best storage to use is local scratch, global scratch and cluster home drive.

Is any of the cluster based storage backed up?
==============================================

The only cluster based storage that gets backed up is the cluster home drives. All other storage including local and global scratch is not backed up.

How do I actually use local scratch?
====================================

The easiest way of making use of local scratch is to use scripts to copy files to the node at the start of your job and from the node when your job finishes. You should also use local scratch for your working directory and temporary files.

Why am I having trouble creating a symbolic link?
=================================================

Not all filesystems support symbolic links. The most common examples are some Windows network shares. On Katana this includes Windows network shares such as hdrive. The target of the symbolic link can be within such a filesystem, but the link itself must be on a filesystem that supports symbolic links, e.g. the rest of your home directory or your scratch directory. 

What storage is available on compute nodes?
===========================================

As well as local scratch, global scratch and your cluster home drive are accessible on the compute nodes.

What is the best way to transfer a large amount of data onto a cluster?
=======================================================================

Use :code:`rsync` to copy data to the KDM server. More information is above.

Is there any way of connecting my own file storage to one of the clusters?
==========================================================================

Whilst it is not possible to connect individual drives to any of the clusters, some units and research groups have purchased large capacity storage units which are co-located with the clusters. This storage is then available on the cluster nodes. For more information please contact the Research Technology Service Team by placing a request with the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_.

Can I specify how much file storage I want on local scratch?
============================================================

If you want to specify the minimum amount of space on the drive before your job will be assigned to a node then you can use the file option in your job script. Unfortunately setting up more complicated file requirements is currently problematic.

Can I run a program directly from scratch or my home drive after logging in to the cluster rather submitting a job?
===================================================================================================================

As the file server does not have any computational resources you would be running the job from the head node on the cluster. If you need to enter information when running your job then you should start an interactive job.

****************
Expanding Katana
****************

Katana has significant potential for further expansion. It offers a simple and cost-effective way for research groups to invest in a powerful computing facility and take advantage of the economies that come with joining a system with existing infrastructure. A sophisticated job scheduler ensures that users always receive a fair share of the compute resources that is at least commensurate with their research group’s investment in the cluster. For more information please contact us.

********************
Acknowledging Katana
********************

If you use Katana for calculations that result in a publication then you should add the following text to your work.

::

    This research includes computations using the computational cluster Katana supported by Research Technology Services at UNSW Sydney.

If you are using nodes that have been purchased using an external funding source you should also acknowledge the source of those funds.

For information about `acknowledging ARC funding <https://www.arc.gov.au/acknowledging-arc>`_

Your School or Research Group may also have policies for compute nodes that they have purchased.

Facilities external to UNSW
===========================

If you are using facilities at Intersect_ or NCI_ in addition to Katana they may also require some form of acknowledgement.


.. _Intersect: https://intersect.org.au/attribution
.. _NCI: http://nci.org.au/users/nci-terms-and-conditions-access
