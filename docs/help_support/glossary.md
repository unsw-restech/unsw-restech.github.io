### Active Job 

Active jobs (also called running jobs) are jobs that have been assigned to a compute node and are currently running. These can be seen by running `qstat`
and looking for a R in the second last column. [See examples](../../using_katana/running_jobs#managing-jobs-on-katana).

---

### Array Job

If you want to run the same job multiple times with slight differences (filenames, data sources, variables, etc), then you can create an array job
which will submit multiple jobs for you from the one job script. 

---

### Batch Job

A batch job is a job on a cluster that runs without any further input once it has been submitted
and will start running as soon as it reaches a compute node even if it is the middle of the night. Almost all jobs on the cluster are
batch jobs with the remainder being [Interactive Jobs](./#interactive-job) including those that are run 
using [Katana OnDemand](../../using_katana/ondemand/). [See examples](../../using_katana/running_jobs/#batch-jobs).

---

### Cluster

A computational cluster is a set of connected computers that work together to create a single system. They are often referred to as
supercomputers or HPC (High Performance Computing) systems. All clusters will have [Management Servers](./#management-servers), 
[Login Nodes](./#login-node) and [Compute Nodes](./#compute-nodes). Most clusters also have one of more [Data Transfer Nodes](./#data-transfer-node).
The data transfer node for Katana is called the [Katana Data Mover or KDM](./storage/).

---

### Compute Nodes

The compute nodes are where the compute jobs run. Users submit jobs from the [Login Node](./#login-node) and the
[Job Scheduler](./#job-scheduler) on the [Head Node](./#head-node) will assign the job to one or more compute nodes.

---

### CPU Core

Each node in the cluster has one or more CPUs each of which has 6 or more cores. Each core is able to run one 
job at a time so a node with 12 cores could have 12 jobs running in parallel.

---

### Data Transfer Node

The Data Transfer Node, also known as the [Katana Data Mover (KDM)](./storage/), is a server that is used for transferring
files to, from, and within the cluster. Due to the nature of moving data around, it uses a significant amount of memory and
network bandwidth. This server is used to take that load off the [Login Node](./#login-node).

---

### Environment Variable 

Environment variables are variables that are set in Linux to give programs information. These all start with a $ symbol
and could be something like your usename or the computer name, or where to find programs and set options. They will start
with a $ symbol. 

For example, all users can reference `$TMPDIR` in their [Job Script](./#job-script) in order to use [Local Scratch](./#local-scratch)

---

### Global Scratch 

Global scratch is a large data store that isn't backed up. It is availble from all nodes in Katana including the
from every node including the [Head Node](./#head-node)[Katana Data Mover (KDM)](./storage/). If you have data files 
or or want to save your computation results when your job finishes this is where you should put them.

---

### Head Node

The head node of the [Cluster](./#cluster) is the computer that manages job and resource management. This is where the 
[Job Scheduler](./#job-scheduler) and [Resource Manager](./#resource-manager) are run. It is kept separate from 
the [Login Node](./#login-node) so that the cluster can keep running if something goes wrong with the [Login Node](./#login-node).
It also means that if anything goes wrong with the scheduler it doesn't affect the users who are using the login nodes.

---

### Held Jobs

Held jobs are jobs that cannot currently run. They are most often put into that state by either the server or the system administrator
but users an also put their jobs on hold. Users sometime want to submit multiple jobs at the same time but want to control
the order in which they run. Unless a job has been held by a user, it will stay on the held state until released by one of the people managing Katana,
at which point they become [Queued Jobs](./#queued-jobs). These can be seen by running `qstat` and looking for an H in the
second last column. [See examples](../../using_katana/running_jobs#managing-jobs-on-katana).

---

### Interactive Job 

An interactive job is a way of running your software on a cluster without negatively impacting the [Login Node](./#login-node). 
Once a request has been submitted and starts running on a [Compute Nodes](./#compute-nodes) with the resources that you requestd.
You can thn figure out exactly what you need to do to run your program and you can use that information to convert your commands
into a baatch job. All jobs on Katana are either a [Batch Job](./#batch-job) or an interactive job. Instructions on
using [interactive jobs](../../using_katana/running_jobs#interactive-jobs).
    
---

### Job Scheduler

The job scheduler monitors the jobs currenty running on the cluster and assigns [Queued Jobs](./#queued-jobs) to
[Compute Nodes](./#compute-nodes) based on recent cluster useage, job resource requirements and nodes available to
the research group of the submitter. To put it another way, the job scheduler determines when and where a job should run.
The job scheduler that is used on Katana is called Owe use is called OpenPBS.

---

### Job Script

A job script is a file containing all of the information needed to run a [Batch Job](./#batch-job) including the resource
requirements and the commands to run the job. A job script can be thought of as a file containing everything that you
would need to type in if you wre running an interative job. [See examples](../../using_katana/running_jobs/#batch-jobs).

---

### Local Scratch 

Local scratch refers to the storage available internally on each compute node. Of all the different storage options on Katana
storage has the best performance which can improve the performance of your job especially if it generates a large number of small
files. The downside of using local scratch is that you will need to copy your data in when your job starts and then copy
it out when your job finishes as local scratch is removed when your job ends. If the software that you are using allows you to specify
a working or temp directory then you can just specify local scratch as the option. Otherwise you will need to add something to 
your job script to move data in and out. You can use local scratch with the [Environment Variable](./#environment-variable) `$TMPDIR`.

---

### Login Node

The login nodes of the cluster is the computer that you connect to when you log in to the cluster. This node is most 
often used to manage jobs incuding checking their status but is also used to compile software. 

---

### Module

The module command is a means of providing access to software including allowing you to chose between different versions
of same software. Read more on the [Environmental Modules page](../../software/environment_modules).

---

### Management Servers

The Management Servers are a collection of servers, including the [Head Node](./#head-node), that can only be accessed by the
people running the cluster. These servers are used to create accounts, schedule jobs, provide computational software, manage Katana storage and keep Katana running.

---

### MPI

Message Passing Infrastructure (MPI) is a technology for running a [Batch Job](./#batch-job) on more than one [Compute Node](./#compute-nodes). Some software has
been designed for situations where parts of the job can run on independent compute nodes with the results being transferred to other nodes
for the next part of the job to be run.

---

### Network Drive 

A network drive is a drive that is independent from the cluster. Whilst files are not accessible from the login or compute nodes
it may be possible to get access to your files on [Katana Data Mover (KDM)](./storage/) and copy files across.

---

### Queue

All submitted jobs are put into a queue which has a collection of resources available to it. As those resources become available, new jobs will be assigned to those resources. 
Job prioritisation is done by the scheduler and depends on a number of factors including length of wait time and total resource use by the user over the previous month.

---

### Queued Jobs 

Queued jobs are jobs that are waiting for a [Compute Nodes](./#compute-nodes) that matches their requirements to become available. 
Which idle job will be assigned to a compute node next depends on the [Job Scheduler](./#job-scheduler) and can be seen by 
running `qstat` and looking for a Q in the second last column. [See examples](../../using_katana/running_jobs#managing-jobs-on-katana).

---

### Resource Manager 

A resource manager works with the [Job Scheduler](./#job-scheduler) to manage running jobs on a cluster. Amongst other tasks it receives and parses job submissions, starts jobs on [Compute Nodes](./#compute-nodes), monitors jobs, kills jobs, and manages how many [CPU Core](./#cpu-core) are available on each [Compute Nodes](./#compute-nodes)

---

### Scratch Space 

Scratch space is a non backed up storage area where users can store transient data. It should not be used for job code as it is not backed up.

---

### Walltime

Om HPC systems like Katana, walltime is the amount of time that you will be allocated when your job runs. If your job runs longer than the walltime, 
it will be killed by the [Job Scheduler](./#job-scheduler) to free up resources for one of the waiting jobs. Walltiime is one of the factors used by
the scheduler to decide when your job will run. 
On **Katana** it Walltime is also used to determine which [Queue](./#queue) your job will be assigned to. The shorter the walltime, the more 
opportunity your job has to run which in turn means that it will start sooner. In short, running a job with a walltime of 12 hours is easier than running
a job will a walltime of 100 hours. It is imprtant to note rhat youur job will be scheduled based on the walltime that you have requested and not
how long your job takes to run.