title: Tips for using PBS and Katana effectively

## Keep your jobs under 12 hours if possible

If you request more than 12 hours of `WALLTIME` then you can only use the nodes bought by your school or research group. Keeping your job's run time request under 12 hours means that it can run on any node in the cluster.

!!! important
    Two 10 hour jobs will probably finish sooner that one 20 hour job.

In fact, if there is spare capacity on Katana, which there is most of the time, six 10 hours jobs will finish before a single 20 hour job will.
Requesting more resources for your job decreases the places that the job can run

The most obvious example is going over the 12 hour limit which limits the number of compute nodes that your job can run on. For example specifying the CPU in your job script restricts you to the nodes with that CPU. A job that requests 20Gb will run on a 128Gb node with a 100Gb job already running but a 30Gb job will not be able to.

## Running your jobs interactively makes it hard to manage multiple concurrent jobs

If you are currently only running jobs interactively then you should move to batch jobs which allow you to submit more jobs which then start, run and finish automatically.
If you have multiple batch jobs that are almost identical then you should consider using array jobs

If your batch jobs are the same except for a change in file name or another variable then you should have a look at using array jobs.

# Other Advanced Usages

As well as the information presented here, examples are available in the [Restech HPC Github repository](../using_katana/running_jobs/#restech-github-repositories)

## Array Jobs

One common use of computational clusters is to do the same thing multiple times - sometimes with slightly different input, sometimes to get averages from randomness within the process.
This is made easier with array jobs.

An array job is a single job script that spawns many almost identical sub-jobs. The only difference between the sub-jobs is an environment variable `$PBS_ARRAY_INDEX` whose value uniquely
identifies an individual sub-job. A regular job becomes an array job when it uses the `#PBS -J` flag. 

For example, the following script will spawn 100 sub-jobs. Each sub-job will require one CPU core, 1GB memory and 1 hour run-time, and it will execute the same application. However, a different
input file will be passed to the application within each sub-job. The first sub-job will read input data from a file called `1.dat`, the second sub-job will read input data from a file called
`2.dat` and so on. 

!!! note
    In this example we are using `brace expansion` - the {} characters around the bash variables - because they are needed for variables that change, like array indices. They aren't strictly
	necessary for `$PBS_O_WORKDIR` but we include them to show consistency.

``` bash
#!/bin/bash
    
#PBS -l select=1:ncpus=1:mem=1gb
#PBS -l walltime=1:00:00
#PBS -j oe
#PBS -J 1-100
    
cd ${PBS_O_WORKDIR}
    
./myprogram ${PBS_ARRAY_INDEX}.dat
```

There are some more examples of array jobs including how to group your computations in an array job on the [UNSW Github HPC examples](https://github.com/unsw-edu-au/Restech-HPC/tree/master/hpc-examples) page. (Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repo)

## Splitting large Batch Jobs

If your batch job can be split into multiple steps you may want to split one big job up into a number of smaller jobs. There are a number of reasons to spend the time to implement this.

1. If your large job runs for over 200 hours, it won't finish on Katana.
2. If your job has multiple steps which use different amounts of resources at each step. If you have a pipeline that takes 50 hours to run and needs 200GB of memory for an hour, but only 50GB the rest of the time, then the memory is sitting idle. 
3. Katana has prioritisations based on how many resources any one user uses. If you ask for 200GB of memory, this will be accounted for when working out your next job's priority.
4. Because there are many more resources for 12 hour jobs, seven or eight 12 hour jobs will often finish well before a single 100 hour job even starts. 