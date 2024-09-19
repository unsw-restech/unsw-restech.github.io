title: Running Jobs on Katana

## Brief Overview

<figure markdown>
  ![Image title](../assets/simple_HPC.png){ width="800" }
  <figcaption>
    Simple HPC Architecture
  </figcaption>
</figure>

The [Login Node](../../help_support/glossary#login-node) of a cluster is a shared resource for all users and is used for preparing, submitting and managing jobs. 

!!! warning
    Run any computationally intensive processes on the **compute nodes**, not **login nodes** 
   
Jobs are submitted from the login node, which delivers them to the [Head Node](../../help_support/glossary#head-node) for job and resource management. Once the resources have been
allocated and are available, the job will run on one or more of the compute nodes as requested. 

Different clusters use different tools to manage resources and schedule jobs - OpenPBS and SLURM are two popular systems. Katana, like NCI's Gadi, uses OpenPBS for this purpose.

Jobs are submitted using the `qsub` command. There are two types of job that `qsub` will accept: an [Batch Job](../../help_support/glossary#batch-job) and a [Interactive Job](../../help_support/glossary#interactive-job).
Regardless of type, the resource manager will put your job in a [Queue](../../help_support/glossary#queue).

An **interactive job** provides a shell session on a [Compute Nodes](../../help_support/glossary#compute-nodes). You interact directly with the compute node running the software you need explicitly.
Interactive jobs are useful for experimentation, debugging, and planning for **batch jobs**. You may also want to use [Katana on Demand](../using_katana/ondemand/) for the same purpose.

!!! note
    For calculations that run longer than a few hours, **batch jobs** are preferred.   

In contrast, a [Batch Job](../../help_support/glossary#batch-job) is a scripted job that - after submission via `qsub` - runs from start to finish without any user intervention. The vast majority
of jobs on the cluster are batch jobs. This type of job is appropriate for production runs that will consume several hours or days. 

To submit a [Batch Job](../../help_support/glossary#batch-job) you will need to create a job script which specifies the resources that your job requires and calls your program. The general structure
of [A Job Script](#a-job-script) is shown below.

!!! important
    All jobs go into a [Queue](../../help_support/glossary#queue) while waiting for resources to become available. The length of time your jobs wait in a queue for resources depends on a number of factors.

The main resources available for use are Memory (RAM), [CPU Core](../../help_support/glossary#cpu-core) (number of CPUs) and [Walltime](../../help_support/glossary#walltime)
(how long you want the CPUs for). These need to be considered carefully when writing your job script, since the decisions you make will impact which queue your jobs ends up on.

As you request more memory, CPU cores, or walltime, the number of available queues goes down. The limits are which the number of queues decrease are summarised in the table below

## Batch Jobs (qsub)

A batch job is a script that runs autonomously on a compute node. The script must contain the necessary sequence of commands to complete a task independently of any input from the user. This section
contains information about how to create and submit a batch job on Katana.

### Getting Started

The following script simply executes a pre-compiled program ("myprogram") in the user's home directory:

``` bash
#!/bin/bash

cd $HOME

./myprogram
```

This script can be submitted to the cluster with `qsub` and it will become a job and be assigned to a queue. If the script is in a file called `myjob.pbs` then the following
command will submit the job with the default resource requirements (1 CPU core with 1GB of memory for 1 hour):

``` bash
[z1234567@katana1 ~]$ qsub myjob.pbs
1237.kman.restech.unsw.edu.au
```

As with interactive jobs, the `-l` (lowercase L) flag can be used to specify resource requirements for the job:

``` bash
[z1234567@katana ~]$ qsub -l select=1:ncpus=1:mem=4gb,walltime=12:00:00 myjob.pbs
1238.kman.restech.unsw.edu.au
```

If we wanted to use the GPU resources, we would write something like this - note that because of configuration of machines, you should request: `ncpus=(#ngpus*6):mem=(#ngpus*46)`

``` bash
[z1234567@katana ~]$ qsub -l select=1:ncpus=6:ngpus=1:mem=46gb,walltime=12:00:00 myjob.pbs
1238.kman.restech.unsw.edu.au
```


### A Job Script

Job scripts offer a much more convenient method for invoking any of the options that can be passed to `qsub` on the command-line. In a shell script, a line starting with # is a comment and will
be ignored by the shell interpreter. However, in a job script, a line starting with #PBS can be used to pass options to the `qsub` command.

Here is an overview of the different parts of a job script which we will examine further below. In the following sections we will add some code, explain what it does, then show some new code,
and iterate up to something quite powerful.

For the previous example, the job script could be rewritten as:

``` bash 
#!/bin/bash

#PBS -l select=1:ncpus=1:mem=4gb
#PBS -l walltime=12:00:00
    
cd $HOME
    
./myprogram
```

!!! warning
    Be careful not to use resource request formats that are old, or intended for a different cluster.
    E.g. *nodes=X:ppn=Y* is an old resource request format, not meant katana's current setup. 

This structure is the most common that you will use. The top line must be `#!/bin/bash` - we are running bash scripts, and this is required.
The following section - the lines starting with `#PBS` - are where we will be configuring how the job will be run - here we are asking for resources.
The final section shows the commands that will be executed in the configured session.

The script can now be submitted with much less typing:

``` bash
[z1234567@katana ~]$ qsub myjob.pbs
1239.kman.restech.unsw.edu.au
```

Unlike submission of an interactive job, which results in a session on a compute node ready to accept commands, the submission of a batch job returns the ID of the new job. This is confirmation
that the job was submitted successfully. The job is now processed by the job scheduler and resource manager. Commands for checking the status of the job can be found in the section below,
[Managing Jobs on Katana](#managing-jobs-on-katana).

If you wish to be notified by email when the job finishes then use the `-M` flag to specify the email address and the `-m` flag to declare which events cause a notification. Here we will get an
email if the job aborts (`-m a`) due to an error or ends (`-m e`) naturally. 

``` bash
#PBS -M your.name.here@unsw.edu.au
#PBS -m ae
```

The output that would normally go to screen and error messages of a batch job will be saved to file when your job ends. By default these files will be called `JOB_NAME.oJOB_ID` and `JOB_NAME.eJOB_ID`, 
and they will appear in the directory that was the current working directory when the job was submitted. In the above example, they would be `myjob.o1239` and `myjob.e1239`.  You can merge these into
a single file with the `-j oe` flag. The `-o` flag allows you to rename the file.

``` bash
#PBS -j oe
```

When a job starts, it needs to know where to save its output and do its work. This is called the *current working directory*. By default the job scheduler will make your *current working directory*
your home directory (`/home/z1234567`). This isn't likely or ideal and is important that each job sets its current working directory appropriately. There are a couple of ways to do this, the easiest
is to set the *current working directory* to the directory you are in when you execute `qsub` by using

``` bash
cd $PBS_O_WORKDIR
```

There is one last special variable you should know about, especially if you are working with large datasets. The storage on the compute node your job is running on will always be faster than
the network drive.

If you use the storage close to the CPUs - in the server rather than on the shared drives, called [Local Scratch](../../help_support/glossary#local-scratch) - you can often save hours of time
reading and writing across the network. 

In order to do this, you can copy data to and from the local scratch, called `$TMPDIR`:

``` bash
cp /home/z1234567/project/massivedata.tar.gz $TMPDIR
tar xvf massivedata.tar.gz
my_analysis.py massive_data
cp -r $TMPDIR/my_output /home/z1234567
```

There are a lot of things that can be done with PBSPro, but you don't and won't need to know it all. These few basics will get you started. 

Here's the full script as we've described. You can copy this into a text editor and once you've changed our dummy values for yours, you only need to change the last line.

``` bash
#!/bin/bash

#PBS -l select=1:ncpus=1:mem=4gb
#PBS -l walltime=12:00:00
#PBS -M your.name.here@unsw.edu.au
#PBS -m ae
#PBS -j oe
    
cd $PBS_O_WORKDIR
    
./myprogram
```

### Restech Github repositories

Once you follow the instructions on the [UNSW research GitHub page](https://research.unsw.edu.au/github) which has information about GitHub including how to join the UNSW GitHub organisation
you will be able to access the following research related repositories including the Restech HPC repository that includes examples of Katana job scripts:

- [Restech-HPC](https://github.com/unsw-edu-au/Restech-HPC/tree/master/hpc-examples) - Example job scripts for Katana and NCI 
- [UNSW-Data-Archive](https://github.com/unsw-edu-au/UNSW-Data-Archive) - Scripts for uploading to and downloading from the UNSW Data Archive
- [UNSW-eNotebook-LabArchives](https://github.com/unsw-edu-au/UNSW-eNotebook-LabArchives) - UNSW eNotebook (LabArchives) widgets

## Interactive Jobs (qsub)

An interactive job or interactive session is a session on a compute node with the required physical resources for the period of time requested. To request an interactive job, add the -I flag
(capital i) to `qsub`. Default sessions will have 1 CPU core, 1GB and 1 hour

For example, the following two commands. The first provides a default session, the second provides a session with two CPU core and 8GB memory for three hours. You can tell when an interactive
job has started when you see the name of the server change from `katana1` or `katana2` to the name of the server your job is running on. In these cases it's `k181` and `k201` respectively.


=== "Default Resources"
    ``` bash
    [z1234567@katana1 ~]$ qsub -I
    qsub: waiting for job 313704.kman.restech.unsw.edu.au to start
    qsub: job 313704.kman.restech.unsw.edu.au ready
    [z1234567@k181 ~]$ 
    ```
=== "Custom Resources"
    ``` bash
    [z1234567@katana2 ~]$ qsub -I -l select=1:ncpus=2:mem=8gb,walltime=3:00:00
    qsub: waiting for job 1234.kman.restech.unsw.edu.au to start
    qsub: job 1234.kman.restech.unsw.edu.au ready
    [z1234567@k201 ~]$ 
    ```

Jobs are constrained by the resources that are requested. In the previous example the first job - running on `k181` - would be terminated after 1 hour or if a command within the session consumed more than 8GB memory. The job (and therefore the session) can also be terminated by the user with `CTRL-D` or the `logout` command.

Interactive jobs can be particularly useful while developing and testing code for a future batch job, or performing an interactive analysis that requires significant compute resources. Never attempt such tasks on the login node -- submit an interactive job instead.



## Job queue limits summary 

Typical job queue limit cut-offs are shown below. **The walltime is what determines whether a job can be run on any node, or only on a restricted set of nodes.**

<table>
	<tbody>
		<tr>
			<td>Resource</td>
			<td colspan="6">Queue limit cut-offs</td>
		</tr>
		<tr>
			<td>Memory (GB)</td>
			<td>124</td>
			<td>180</td>
			<td>248</td>
			<td>370</td>
			<td>750</td>
			<td>1000</td>
		</tr>
		<tr>
			<td>CPU Cores</td>
			<td>16</td>
			<td>20</td>
			<td>24</td>
			<td>28</td>
			<td>32</td>
			<td>44</td>
		</tr>
		<tr>
			<td rowspan="2">Walltime (hrs)</td>
			<td>12</td>
			<td>48</td>
			<td>100</td>
			<td colspan="3">200</td>
		</tr>
		<tr>
			<td>Any node</td>
			<td colspan="2">School-owned or general-use nodes</td>
			<td colspan="3">School-owned nodes only</td>
		</tr>
	</tbody>
</table>


!!! note
    Try to combine or divide batch jobs to fit within that 12 hour limit for fastest starting times. 
 
The resources available on a specific compute node can be shown with the [qstat](#managing-jobs-on-katana) command.


## Accessing the Grace Hopper (GH200) GPU Node on Katana

The GH200 features higher performance compared to V100 and A100. This node is accessible to all users but uses a different architecture, ARM. Regular Katana modules do not run on this node, this is a powerful machine suitable for experimental applications using mainly Python. To use this node simply add *cpu_arch = aarc64* resource:
``` bash
[z1234567@katana ~]$ qsub  -l select=1:cpu_arch=aarch64:ngpus=1:ncpus=72:mem=585505mb  myjob.pbs
1238.kman.restech.unsw.edu.au

Or add below to your myjob.pbs file
#PBS -l select=1:cpu_arch=aarch64:ngpus=1:ncpus=72:mem=585505mb
```

!!! Key Notes
    * The Grace Hopper node contains one 72-core ARM CPU with 480GB memory.
    	It has a different CPU architecture to the rest of Katana, consequently different binaries must be used.
     	We recommend using the default GNU compiler and CUDA libraries on the node, and your own installation of Conda (aarch64).
    * The node contains one Hopper generation GPU with 96GB HBM3 memory.
    * The CPU and GPU are connected via 900GB/s NVLink.
    	This makes it feasible for the CPU and GPU to present their memory as unified pool, i.e. you can run GPU jobs that require more than 96GB memory.
