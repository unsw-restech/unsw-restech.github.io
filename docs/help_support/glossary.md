### Active Job 

Active jobs (also called running jobs) are jobs that have been assigned to a compute node and are currently running. These can be seen by running `qstat` and 
looking for a R in the second last column. [See examples](../../using_katana/running_jobs#managing-jobs-on-katana).

---

### Array Job

If you want to run the same job multiple times with slight differences (filenames, data sources, variables, etc), then you can create an array job which will submit multiple jobs for you from the one job script. 

---

### Batch Job

A batch job is a job on a cluster that runs without any further input once it has been submitted and will start running as soon as it reaches a compute node. Almost all jobs on the cluster are batch jobs with the remainder being [Interactive Jobs](./#interactive-job) including those
that are run using [Katana OnDemand](../../using_katana/ondemand/)

---


### Cluster

A computational cluster is a set of connected computers that work together to create a single system amd is often referred to as a supercomputer or HPC (High Performance Computing) system. Most will have a [Management Plane](./#management-plane) and several [Compute Nodes](./#compute-nodes).

---

### Compute Nodes

The compute nodes are where the compute jobs run. Users submit jobs from the [Login Node](./#login-node) and the [Job Scheduler](./#job-scheduler) on the [Head Node](./#head-node) will assign the job to one or more compute nodes.

---

### CPU Core

Each node in the cluster has one or more CPUs each of which has 6 or more cores. Each core is able to run one job at a time so a node with 12 cores could have 12 jobs running in parallel.

---

### Data Transfer Node

The Data Transfer Node, also known as the [Katana Data Mover (KDM)](./storage/), is a server that is used for transferring files to, from, and within the cluster. Due to the nature of moving data around, it uses a significant amount of memory and network bandwidth. This server is used to take that load off the [Login Node](./#login-node).

---

### Environment Variable 

Environment variables are variables that are set in Linux to tell applications where to find programs and set program options. They will start with a $ symbol. For example, all users can reference `$TMPDIR` in their [Job Script](./#job-script) in order to use [Local Scratch](./#local-scratch)

---

### Global Scratch 

Global scratch is a large data store for data that isn't backed up. It differs from local scratch in that it is available from every node including the [Head Node](./#head-node). If you have data files or working directories this is where you should put them.

---

### Head Node

The head node of the [Cluster](./#cluster) is the computer that manages job and resource management. This is where the [Job Scheduler](./#job-scheduler) and [Resource Manager](./#resource-manager) are run. It is kept separate from the [Login Node](./#login-node) so that production doesn't stop if someone accidentally breaks the [Login Node](./#login-node).

---

### Held Jobs

Held jobs are jobs that cannot currently run. They are put into that state by either the server or the system administrator. Jobs stay held until released by a systems administrator, at which point they become [Queued Jobs](./#queued-jobs). These can be seen by running `qstat` and looking for an H in the second last column. [See examples](../../using_katana/running_jobs#managing-jobs-on-katana).

---

### Interactive Job 

An interactive job is a way of testing your program and data on a cluster without negatively impacting the [Login Node](./#login-node). Once a request has been submitted and accepted for an interactive job, the user will no longer be on the relatively small login nodes, and will have access to the resources requested on the [Compute Nodes](./#compute-nodes). In other words, your terminal session will move from a small (virtual) computer you share with many people to a large computer you share with very few people. All jobs are either a [Batch Job](./#batch-job) or an interactive job. Instructions on using [interactive jobs](../../using_katana/running_jobs#interactive-jobs).
    
---

### Job Scheduler

The job scheduler monitors the jobs currenty running on the cluster and assigns [Queued Jobs](./#queued-jobs) to [Compute Nodes](./#compute-nodes) based on recent cluster useage, job resource requirements and nodes available to the research group of the submitter. In summary the job scheduler determines when and where a job should run. The job scheduler that we use is called PBSPro.

---

### Job Script

A job script is a file containing all of the information needed to run a [Batch Job](./#batch-job) including the resource requirements and the actual commands to run the job.

---

### Local Scratch 

Local scratch refers to the storage available internally on each compute node. Of all the different scratch directories this storage has the best performance however you will need to move your data into local scratch as part of your job script. You can use local scratch with the [Environment Variable](./#environment-variable) `$TMPDIR`

---

### Login Node

The login nodes of the cluster is the computer that you log in to when you connect to the cluster. This node is used to compile software and submit jobs.

---

### Module

The module command is a means of providing access to different versions of software without risking version conflicts across multiple users.

---

### Management Servers

The Management Servers are a collection of servers, including the [Head Node](./#head-node), that can only be accessed by the people running the cluster. These servers
are used to create accounts, schedule jobs, provide computational software, manage Katana storage and keep Katana running.

---

### MPI

Message Passing Infrastructure (MPI) is a technology for running a [Batch Job](./#batch-job) on more than one [Compute Node](./#compute-nodes). Some software has
been designed for situations where parts of the job can run on independent compute nodes with the results being transferred to other nodes
for the next part of the job to be run.

---

### Network Drive 

A network drive is a drive that is independent from the cluster such as oneDrive.

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