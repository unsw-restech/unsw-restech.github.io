title: Ansys

Both Ansys Workbench and Ansys Electronic Desktop are available on [Katana OnDemand](../using_katana/ondemand.md). 

This is the most user-friendly approach to running Ansys jobs on Katana.

## Ansys Batch Jobs

Ansys workbench is available on [myAccess](https://www.myaccess.unsw.edu.au/applications/ansys-workbench) for undergraduate and postgraduate students in the Faculty of Engineering. 

If you install Ansys on your local machine, you can generate your model files locally and transfer them tto katana to run in a batch job. 

## Ansys CFX

For Ansys CFX the .cfx and .def files need to be transferred to Katana.

A brief batch script example is given below. A step-by-step guide is available [on our GitHub](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-cfx.md). (Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repo)

``` bash
   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load intelmpi/2019.6.166
   module load ansys/2021r1

   cfx5solve -batch -def <filename>.def -part $NCPUS -start-method "Intel MPI Local Parallel"
```

## Ansys Fluent

Similarly, Ansys Fluent input can be generated locally and transferred to Katana to run in a batch job.

Again, a brief batch script is below, and a step-by-step guide is available [on our Github](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-fluent.md) (Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repo).

``` bash 
   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load ansys/2021r1
   module load intelmpi/2019.6.166

   fluent 3d -g -t $NCPUS -i fluent.in > output.out
```

## Ansys on Gadi

If you wish to use Ansys on NCI's Gadi, UNSW has a institutional licence that requies you join the relevant software group as detailed in [NCI's documentation](opus.nci.org.au/display/Help/Ansys+Fluent).
