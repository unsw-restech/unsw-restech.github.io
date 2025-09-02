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

- **Interactive Job** – Provides a live shell session on compute nodes for experimentation and debugging. Useful for testing and planning batch jobs.  
- **Batch Job** – Runs a scripted job automatically from start to finish without user intervention. Ideal for long-running production tasks.

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

You can create a job script file using command-line editors like `nano` or `vi`. For example, using `nano`:

```bash
# Create a new file called myjob.pbs
nano myjob.pbs
```

This opens a simple text editor in the terminal. Copy the following template into the editor:

```bash
#!/bin/bash

#PBS -l select=1:ncpus=1:mem=4gb
#PBS -l walltime=12:00:00
#PBS -M your.name.here@unsw.edu.au
#PBS -m ae
#PBS -j oe

cd $PBS_O_WORKDIR

./myprogram
```

- Press `CTRL+O` to save and `CTRL+X` to exit `nano`.

### Step 2: Submit the Batch Job

```bash
qsub myjob.pbs
```

- Returns a job ID (e.g., `1239.kman.restech.unsw.edu.au`)  
- Scheduler runs the job when resources are available.

### Step 3: Specifying Resources

```bash
# 1 CPU, 4GB RAM, 12 hours
qsub -l select=1:ncpus=1:mem=4gb,walltime=12:00:00 myjob.pbs

# 1 GPU, 6 CPUs, 46GB RAM, 12 hours
qsub -l select=1:ncpus=6:ngpus=1:mem=46gb,walltime=12:00:00 myjob.pbs
# Formula for multiple GPUs: ncpus=(#ngpus*6):mem=(#ngpus*46)
```

### Using Local Scratch for Large Data

```bash
cp /home/z1234567/project/massivedata.tar.gz $TMPDIR
tar xvf $TMPDIR/massivedata.tar.gz

my_analysis.py $TMPDIR/massivedata

cp -r $TMPDIR/my_output /home/z1234567
```

> Using `$TMPDIR` (local scratch) is faster than reading/writing over network drives.

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

---

## Job Queue Limits

| Resource       | Queue limit cut-offs                       |
|----------------|-------------------------------------------|
| Memory (GB)    | 124, 180, 248, 370, 750, 1000           |
| CPU Cores      | 16, 20, 24, 28, 32, 44                   |
| Walltime (hrs) | 12, 48, 100, 200                          |

Walltime determines node eligibility:

| Walltime (hrs) | Node type                                    |
|----------------|---------------------------------------------|
| 12             | Any node                                    |
| 48-100         | School-owned or general-use nodes           |
| 200            | School-owned nodes only                      |

> Tip: Combine or divide batch jobs to fit within 12-hour limit for faster starts.

---

## Accessing Grace Hopper (GH200) GPU Node

- High-performance ARM CPU with 72 cores + Hopper GPU (96GB HBM3).  
- Use `cpu_arch=aarch64` to request:

```bash
qsub -l select=1:cpu_arch=aarch64:ngpus=1:ncpus=72:mem=585505mb myjob.pbs
```

- Different architecture → different binaries required.  
- Recommended: GNU compilers, CUDA libraries, and Conda (aarch64).  
- CPU & GPU connected via 900GB/s NVLink → can use unified memory pool (>96GB GPU memory).

---

## Useful Commands

```bash
# Check job status
qstat

# Delete a job
qdel <job_id>

# Show queues
qstat -q
```

---

## Restech GitHub Repositories

- [Restech-HPC](https://github.com/unsw-edu-au/Restech-HPC/tree/master/hpc-examples) – Example Katana scripts  
- [UNSW-Data-Archive](https://github.com/unsw-edu-au/UNSW-Data-Archive) – Upload/download scripts  
- [UNSW-eNotebook-LabArchives](https://github.com/unsw-edu-au/UNSW-eNotebook-LabArchives) – LabArchives widgets

---

This guide includes step-by-step instructions for creating job scripts, submitting jobs, and using Katana safely, even for users without programming experience.
