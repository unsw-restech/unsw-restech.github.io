title: Others

## Ansys

Both Ansys Workbench and Ansys Electronic Desktop are available on Katana. The most user friendly
way to run Ansys is to use [Katana OnDemand](../using_katana/ondemand.md). 

**Ansys Batch Jobs**

**Note:** The version of Ansys available via myAccess **MUST NOT** be used for any research including generating
files to be used on Katana.

Once you are familiar with how running jobs on Katana works you can run them in a [batch job](../using_katana/running_jobs/#batch-jobs). 

**Ansys CFX**

If you want to use Ansys CFX on Katana then you will need to upload the .cfx and .def files to Katana.

A brief batch script example is given below. For more detail visit [our GitHub page](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-cfx.md). 
(Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repository)

``` bash
   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load intelmpi/2019.6.166
   module load ansys/2021r1

   cfx5solve -batch -def <filename>.def -part $NCPUS -start-method "Intel MPI Local Parallel"
```

**Ansys Fluent**

Ansys Fluent input can also be generated locally and transferred to Katana to run in a batch job with a brief bash script shown here snd more complete
examples in [our Github repository](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-fluent.md). (Note: You will need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repo).

``` bash 
   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load ansys/2021r1
   module load intelmpi/2021.7.1

   fluent 3d -g -t $NCPUS -i fluent.in > output.out
```

**Ansys on Gadi**

If you have an account on [Gadi](https://nci.org.au/our-services/supercomputing), the Supercomputer located at [NCI](https://nci.org.au/), you can also use Ansys there once you join the on once you have an account. UNSW has a institutional licence
that requies you join the relevant software group as detailed in [NCI's documentation](https://opus.nci.org.au/display/Help/Ansys+Fluent).

## Biosciences

There are a number of Bioscience software packages installed and datasets installed. If you look in the `#!bash /data` directory you will find several bioscience related datasets including
multiple versions of the [NCBI](https://www.ncbi.nlm.nih.gov/) nr and nt databases. If you are having trouble finding a software package it might be within another package:

**Stand alone**

Blast+: `:::bash module load blast-plus/2.12.0`

Mothur: `:::bash module load mothur/1.48.0`

**Python Module**

Any of the Python versions that you see when running the [module command](../software/environment_modules) on Katana will include **BioPython** and **Snakemake**.
`:::bash module avail python`

**R Module**

`:::bash module avail r`

As Bioconductoris is installed within R the best approach is to load the latest version of R to reduce the possibility of dependency issues. Then you can install
it in your personal R library by following the instructions at the [Bioconductor web site](https://www.bioconductor.org/). An example of how to install Bioconductor
and some Bioconductor packages is shown below.

``` r
    > if (!require("BiocManager", quietly = TRUE))
    install.packages("BiocManager")
    > BiocManager::install("Biobase")
    > BiocManager::install(c("GenomicFeatures", "AnnotationDbi"))
    > BiocManager::install("DESeq2")
```

**Perl Module**
Any of the Perl versions that you see when running the [module command](../software/environment_modules) on Katana will include **BioPerl**. If you do not use the
module command to access Perl then you won't have be able to use BioPerl and other Perl tools. 
`:::bash module avail perl`

**Conda**
If you would like to install software using Conda you there are instructions on how to do it on the [Python page](../software/python/).

## Comsol
The most user friendly way to run Comsol interactively is to use [Katana OnDemand](../using_katana/ondemand.md).

!!! note
    You will need to belong to a group that owns a COMSOL licence (mech, spree, quantum, biomodel) 

**Comsol Batch Jobs**

**Note:** The version of COMSOL available via myAccess **MUST NOT** be used for any research including generating
files to be used on Katana.

An example comsol batch job file is available in [our GitHub repository](https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/comsol/comsol.pbs).
(Note: You need to [join the UNSW GitHub organisation](https://research.unsw.edu.au/github) to access this repository) and an uncommented version is reproduced below.

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

Research Technology Services has a licence for Intel Compiler Collection which can be accessed by loading a module and contains 4 groups of software, namely compilers, libraries, a debugger
and MPI. This software has been optimised by Intel to take advantage of the specific capabilities of the different intel CPUs installed in the Intel based clusters.

- Compilers (module name: intel-compilers)
    - Intel C Compiler (icc)
    - Intel C++ Compiler (icpc)
    - Intel Fortran Compiler (ifort)
- Libraries
    - Intel Math Kernel Library (MKL) (module name: intel-mkl)
    - Intel Threading Building Blocks (TBB) (module name: intel-tbb)
    - Intel Integrated Performance Primitives (IPP)
- Debugger
    - Intel Debugger (idbc)

## Java

Java is installed as part of the Operating System but that version of Java can change without warning leading to reproducable concerns. Because of this risk
we recommend using one of the versions of Java available via the [module command](../software/environment_modules).

`:::bash module avail java`
 
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

You can run an interactive session of Matlab using [Katana OnDemand](../using_katana/ondemand) for a graphical session or [using the qsub command](using_katana/running_jobs/#interactive-jobs)
for a text based session using the commands below.

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

Research software is installed in [environment modules](../software/environment_modules/). This enables multiple versions of the same software to be installed, and each user can choose which version they wish to use.

## Perl

The default version of Perl on Katana is 5.26.3, which is provided by Rocky Linux 8.9 and can be found at `#!bash /usr/bin/perl`.

Perl 5.36.0 is also installed as an environment module. 

It is common for Perl scripts to begin with:

``` bash
#!/usr/bin/perl
```

However, that will restrict you to the default version of Perl supplied with the Linux distribution.  If, instead of using that line, you use the following: 

``` bash
#!/usr/bin/env perl
```

This will then look in your path so if you load Oerl via an environment module) it will not be necessary to modify your scripts.

<!--
## SAS

The 64-bit version of SAS is available as a module.

By default SAS will store temporary files in `/tmp` which can easily fill up, leaving the node offline. In order to avoid this we have set the default to `$TMPDIR` to save temporary files in `/var/tmp` on the Katana head node and local scratch on compute nodes. If you wish to save temporary files to a different location you can do that by using the `-work` flag with your SAS command or adding this line to your `sasv9.cfg` file:

``` bash
    -work /my/directory
```
-->

## Stata

Stata is available as a module. 

When using Stata in a pbs batch script, the syntax is

``` bash
stata -b do StataClusterWorkshop.do
```

If you wish to load or install additional Stata modules or commands you should use findit command on your local computer to find the command that you are looking for.
Then create a directory called `myadofiles` in your home directory and copy the .ado (and possibly the .hlp) file into that directory. Now that the command is there 
it just remains to tell Stata to look in that directory which can be done by using the following Stata command.

``` bash
sysdir set PERSONAL $HOME/myadofiles
```

## tmux

When you login to Katana using the terminal, it is a "live" session - if you close the terminal or turn off your computer the session will close. If you have a long running program such as
downloading a large data set or have an [interactive session running from the command line](../using_katana/running_jobs/#interactive-jobs) and you need to go somewhere else then you may
end up losing all of your work and need to start again. The solution to this problem is to use `tmux` to create an **interruptible session** when you first connect to Katana.

!!! note
    You will need to take note of the Katana login node that you have logged in to (katana1, katana2 or katana3) as you will need to connect to the same login node to return to your `tmux` session.

To start tmux, type `tmux` at the terminal which will create a new session will start with a green information band at the bottom of the screen. Anything you start in this session 
will keep running even if you disconnect from that session for any reason unless the server needs to be restarted.

If you have used `tmux` and are disconnected you can reconnect by logging in to the server and using the command `tmux a`.

`tmux` has a number of other useful features such as multiple sessions and split screens. More information on features and how to use `tmux` is available
on [the tmux website](https://github.com/tmux/tmux/wiki).

## Zip

**Compressing Large Directories**

If you want to compress large directories or directories with a large number of files, we recommend using [tgzme](https://github.com/unsw-edu-au/tgzme/blob/master/tgzme.sh)
which was developed by one of our researchers, [Dr. Edwards](https://orcid.org/0000-0002-3645-5539).

The software is based on the command:

``` bash
tar -c $DIRECTORY | pigz > $DIRECTORY.tar.gz
```

We thank `Dr. Edwards` for his contribution.





