title: Others

## Ansys

Both Ansys Workbench and Ansys Electronic Desktop are available on [Katana OnDemand](../using_katana/ondemand.md). 

This is the most user-friendly approach to running Ansys jobs on Katana.

**Ansys Batch Jobs**

Ansys workbench is available on [myAccess](https://www.myaccess.unsw.edu.au/applications/ansys-workbench) for undergraduate and postgraduate students in the Faculty of Engineering. 

If you install Ansys on your local machine, you can generate your model files locally and transfer them tto katana to run in a batch job. 

**Ansys CFX**

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

**Ansys Fluent**

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

**Ansys on Gadi**

If you wish to use Ansys on NCI's Gadi, UNSW has a institutional licence that requies you join the relevant software group as detailed in [NCI's documentation](opus.nci.org.au/display/Help/Ansys+Fluent).

## Biosciences

There are a number of Bioscience softwares installed. Note that if the software
you want isn't obvious, it might be within another package:

**Stand alone**

Blast+: `:::bash module load blast+/2.9.0`

Mothur: `:::bash module load mothur/1.44.2`

**Python Module**

`:::bash module avail python`

BioPython

Snakemake

**R Module**

`:::bash module avail R`

Bioconductor

**Perl Module**
`:::bash module avail perl`

BioPerl 


## Comsol

Comsol is best run interactively on [Katana OnDemand](../using_katana/ondemand.md). 

!!! note
    You will need to belong to a group that owns a COMSOL licence (mech, spree, quantum, biomodel) 

**Comsol Batch Jobs**

COMSOL is available to download on [myAccess](https://www.myaccess.unsw.edu.au/applications/ansys-workbench) for undergraduate and postgraduate students in Chemical and Biomedical Engineering. This will allow you to generate module files locally, and transfer them to Katana to process in a batch job.

An example comsol batch job file is available [on our GitHub](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/comsol/comsol.pbs) (Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repo) and an uncommented version is reproduced below.

``` bash title="console"
    #!/bin/bash
    #<RESOURCE REQUESTS>

    mkdir -p ${TMPDIR}/comsol

    export MY_COMSOL_DIR=/srv/scratch/$USER/comsoldir

    module load comsol/5.6-spree

    comsol -nn 1 -np $NCPUS \
    -recoverydir ${MY_COMSOL_DIR}/recoveries \
    -tmpdir ${TMPDIR}/comsol \
    batch \
    -inputfile ${MY_COMSOL_DIR}/MyModel.mph \
    -outputfile ${MY_COMSOL_DIR}/MyModelOut.mph \
    -batchlog ${MY_COMSOL_DIR}/MyModel.log
```

## Intel Compilers and Software Libraries

Research Technology Services has a licence for Intel Compiler Collection which can be accessed by loading a module and contains 3 groups of software, namely compilers, libraries and a debugger. This software has been optimised by Intel to take advantage of the specific capabilities of the different intel CPUs installed in the Intel based clusters.

- Compilers
    - Intel C Compiler (icc)
    - Intel C++ Compiler (icpc)
    - Intel Fortran Compiler (ifort)
- Libraries
    - Intel Math Kernel Library (MKL)
    - Intel Threading Building Blocks (TBB)
    - Intel Integrated Performance Primitives (IPP)
- Debugger
    - Intel Debugger (idbc)

## Java

Java is installed as part of the Operating System but we would strongly recommend against using that version - we cannot guarantee scientific reproducibility with that version. Please use the java modules. 

Each Java module sets 

``` bash 
    _JAVA_TOOL_OPTIONS -Xmx1g
```

This sets the heap memory to 1GB. If you need more, set the environment variable `_JAVA_OPTIONS` which overrides `_JAVA_TOOL_OPTIONS`

```bash
    export _JAVA_OPTIONS="-Xmx5g"
```


## Matlab

**Running interactively**

Interactive sessions of matlab are best run on [Katana OnDemand](../using_katana/ondemand.md). 


**Batch Jobs**

You can run matab within a batch job. The example below shows the flags used to start matlab without a graphical interface. The matlab script (scriptfile.m) needs to be in the same directory as qsub was run to submit the batch job. 

``` bash
module load matlab/R2022a

matlab -nodisplay -nosplash -r scriptfile
```

If you wish to submit the job from a different directory than your matlab script, you need to provide the full file path to cd (change directory) command with matlab. Note the use of quotes.

``` bash
module load matlab/R2022a

matlab -nodisplay -nosplash -r "cd('/path/to/script/');scriptfile"
```

Later versions of matlab provide the '-batch' flag as an alternative. 

``` bash
module load matlab/R2022a

matlab -batch -r scriptfile
```

## Operating Systems

Katana nodes currently run Rocky Linux 8.9.

Research software is installed in environment modules. This enables multiple versions of the same software to be installed, and each user can choose which version they wish to use.

## Perl

The default version of Perl on Katana is 5.26.3, which is provided by Rocky Linux 8.9 and can be found at `#!bash /usr/bin/perl`.

Perl 5.36.0 is also installed as an environment module. 

It is common for Perl scripts to begin with:

``` bash
#!/usr/bin/perl
```

However, that will restrict you to the default version of Perl supplied with the Linux distribution.  For greater flexibility, try using the following: 

``` bash
#!/usr/bin/env perl
```
Then if you choose to use a different version of Perl (by loading an environment module) it will not be necessary to modify your scripts.

## SAS

The 64-bit version of SAS is available as a module.

By default SAS will store temporary files in `/tmp` which can easily fill up, leaving the node offline. In order to avoid this we have set the default to `$TMPDIR` to save temporary files in `/var/tmp` on the Katana head node and local scratch on compute nodes. If you wish to save temporary files to a different location you can do that by using the `-work` flag with your SAS command or adding this line to your `sasv9.cfg` file:

``` bash
    -work /my/directory
```

## Stata

Stata is available as a module. 

When using Stata in a pbs batch script, the syntax is

``` bash
stata -b do StataClusterWorkshop.do
```

If you wish to load or install additional Stata modules or commands you should use findit command on your local computer to find the command that you are looking for. Then create a directory called `myadofiles` in your home directory and copy the .ado (and possibly the .hlp) file into that directory. Now that the command is there it just remains to tell Stata to look in that directory which can be done by using the following Stata command.

``` bash
sysdir set PERSONAL $HOME/myadofiles
```

## TMUX

tmux is available on Katana. It can be used to manage multiple sessions, including keeping them alive despite a terminal losing connectivity or being shutdown.

When you login to Katana using the terminal, it is a "live" session - if you close the terminal, the session will also close. If you shut your laptop or turn off the network, you will also kill the session.

This is fine, except when you have a long running program - say you are downloading a large data set - and you need to leave campus.

In these situations, you can use `tmux` to create an **interruptible session**. `tmux` has other powerful features - multiple sessions and split screens being two of many features.

To start tmux, type `tmux` at the terminal. A new session will start and there will be a green information band at the bottom of the screen. 

Anything you start in this session will keep running even if you are disconnected from that session regardless of the reason. Except when the server itself is rebooted. That - and I hope this is obvious - will kill all sessions.

If you do get detached, you can re-attach by logging into the same server and using the command `tmux a`

tmux is similar to another program called `screen` which is also available. 

[tmux](https://github.com/tmux/tmux/wiki)

## Zip

**Compressing Large Directories**

If you want to compress large directories or directories with a large number of files, we recommend a simple tool called tgzme developed by one of our researchers.

It's essentially a smart shell script wrapped around the one line command:

``` bash
tar -c $DIRECTORY | pigz > $DIRECTORY.tar.gz
```

We thank `Dr. Edwards` for his contribution.

[tgzme](https://github.com/unsw-edu-au/tgzme/blob/master/tgzme.sh)

[Dr. Edwards](https://orcid.org/0000-0002-3645-5539)

