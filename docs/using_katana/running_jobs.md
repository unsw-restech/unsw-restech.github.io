# Running Jobs on Katana

## Brief Overview

Katana is a High Performance Computing (HPC) cluster. It allows users to run computationally intensive programs on powerful compute nodes. **Login nodes** are only for preparing, submitting, and managing jobs—not for heavy calculations.

<figure markdown>
  ![Simple HPC Architecture](../assets/simple_HPC.png){ width="800" }
  <figcaption>Simple HPC Architecture</figcaption>
</figure>

**⚠ Warning:** Do not run computationally intensive processes on **login nodes**; use **compute nodes** instead.

Jobs are submitted from the login node, which delivers them to the **Head Node** for job and resource management. Once resources are allocated, the job will run on one or more compute nodes.

Katana uses OpenPBS to manage resources and schedule jobs.

### Job Types

- **Batch Job** – Runs a scripted job automatically from start to finish without user intervention. Ideal for long-running production tasks.
- **Interactive Job** – Provides a live shell session on compute nodes for experimentation and debugging. Useful for testing and planning batch jobs.  

All jobs enter a **queue** while waiting for resources.

### Resources

Main resources requested by jobs:

- **Memory (RAM)**  
- **CPU cores**  
- **Walltime** (time for CPUs)  

> Note: Increasing memory, CPU cores, or walltime may limit the available queues. See the queue limits below.

---

## Batch Jobs (qsub)

A batch job is a script that runs autonomously on a compute node. The script specifies resources and commands to run.

### Step 1: Create a Job Script File

<figure markdown>
  ![Simple HPC Architecture](../assets/powershell_example.png){ width="800" }
  <figcaption>Windows Powershell</figcaption>
</figure>

You can create a job script file in command-line editors like `Powershell`. For example, using `nano`:

```bash
# Create a new file called myjob.pbs
nano myjob.pbs
```

This opens a simple text editor in the terminal. Copy the following template into the editor:

```bash
#!/bin/bash

#PBS -l select=1:ncpus=1:mem=4gb
#PBS -l walltime=12:00:00
#PBS -M zID@ad.unsw.edu.au
#PBS -m ae
#PBS -j oe

cd $PBS_O_WORKDIR

./myprogram
```
!!! note
	What each line does:

	```bash
	#!/bin/bash
	```

	Tells the system to use the Bash shell to run your script. Without this, your commands may not be interpreted correctly.

	```bash
	#PBS -l select=1:ncpus=1:mem=4gb`
	```

	Requests 1 compute node with 1 CPU core and 4GB RAM. The scheduler uses this to allocate resources.

	```bash
	#PBS -l walltime=12:00:00
	```

	Sets the maximum run time to 12 hours. If your job exceeds this, it will be automatically terminated.

	```bash
	#PBS -M your.name.here@unsw.edu.au
	#PBS -m ae
	```

	Sends an email notification if the job aborts (a) or ends normally (e). Useful to know when your job finishes or fails. (Optional)

	```bash
	#PBS -j oe
	```

	Combines the standard output and standard error into a single file, making it easier to review the results. (Optional)

	```bash
	cd $PBS_O_WORKDIR
	```

	Changes the working directory to where you ran qsub. By default, jobs start in your home directory, which may not contain the files you need. (Optional)

	```bash
	./myprogram
	```

	Runs your program. Replace myprogram with the actual program or script you want to execute.

- Press `CTRL+O` to save and `CTRL+X` to exit `nano`.

### Step 2: Submit the Batch Job

```bash
qsub myjob.pbs
```

- Terminal will return a job ID (e.g., `1239.kman.restech.unsw.edu.au`)  
- Scheduler runs the job when resources are available.

---

## Interactive Jobs (qsub -I)

Interactive jobs provide a shell session on compute nodes with requested resources.

```bash
# Default interactive session (1 CPU, 1GB, 1 hour)
qsub -I

# Custom interactive session (2 CPUs, 8GB, 3 hours)
qsub -I -l select=1:ncpus=2:mem=8gb,walltime=3:00:00
```

- Session starts when prompt changes to the compute node name (e.g., `k181`, `k201`).  
- Session ends when time expires, memory is exceeded, or user exits.  
- Useful for developing and testing code before batch submission.  

## Understanding Walltime

Walltime is the maximum amount of real time that your job is allowed to run on the cluster. It is requested when you submit a job and is used by the scheduler to plan resources.

- Walltime is specified in hours, minutes, and seconds, the default walltime is 1 hour if not specified in script.
- If your job runs longer than the walltime, it will be terminated automatically, even if it hasn’t finished.
- Walltime affects which queues your job can be scheduled on. Shorter walltime jobs usually start faster, while longer jobs may only be able to run on specific nodes.
- Always estimate your job’s runtime carefully. If unsure, it’s safer to slightly overestimate but not excessively, as very long walltime requests may reduce scheduling priority.
- For long workflows, consider splitting tasks into multiple jobs to fit within walltime limits.

---

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

---x`

## Useful Commands

```bash
# Check job status
qstat

# Delete a job
qdel <job_id>

# Show queues
qstat -q
```
For details on how to monitor or manage you jobs, see the next page.

---

## Accessing Grace Hopper (GH200) GPU Node

Katana includes a Grace Hopper (GH200) node, which combines an ARM-based CPU with a Hopper-generation GPU. This node offers much higher performance than Katana’s older GPU nodes (V100 and A100) and is designed for experimental, high-end workloads.

#### Key Characteristics of the GH200 Node

- CPU: 72-core ARM CPU (architecture: aarch64) with 480 GB memory.

- GPU: One Hopper GPU with 96 GB HBM3 memory.

- Memory bandwidth: The CPU and GPU are linked with 900 GB/s NVLink, allowing them to share memory. This means you can run GPU jobs requiring more than 96 GB memory, because the GPU can access CPU memory directly.

- Architecture note: The ARM CPU architecture is different from the rest of Katana (x86_64). Regular Katana binaries and modules will not work here.

#### Software Considerations

- Use the default GNU compiler and CUDA libraries available on the node.

- Install your own Python environment using Conda for ARM (aarch64), since many precompiled packages won’t run on ARM.

- Jobs here are best suited for Python-based machine learning, deep learning, and experimental HPC applications.

#### Submitting Jobs to the GH200 Node

To request the GH200 node, you must specify the `cpu_arch=aarch64` resource in your job submission.

Option 1 - Direct `qsub` command

```bash
[z1234567@katana ~]$ qsub -l select=1:cpu_arch=aarch64:ngpus=1:ncpus=72:mem=480gb myjob.pbs
1238.kman.restech.unsw.edu.au
```

Option 2 - Inside your job script

```bash
#PBS -l select=1:cpu_arch=aarch64:ngpus=1:ncpus=72:mem=480gb
```

#### When to Use the GH200 Node

- If your job requires very high GPU memory bandwidth or unified CPU–GPU memory.

- If you are experimenting with next-generation AI/ML workloads.

- If your application can be built or run on ARM (aarch64) architecture.

---

## Restech GitHub Repositories

- [Restech-HPC](https://github.com/unsw-edu-au/Restech-HPC/tree/master/hpc-examples) – Example Katana scripts  
- [UNSW-Data-Archive](https://github.com/unsw-edu-au/UNSW-Data-Archive) – Upload/download scripts  
- [UNSW-eNotebook-LabArchives](https://github.com/unsw-edu-au/UNSW-eNotebook-LabArchives) – LabArchives widgets

---

This guide includes step-by-step instructions for creating job scripts, submitting jobs, and using Katana safely, even for users without programming experience.
