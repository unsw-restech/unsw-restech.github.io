title: Comsol

Comsol is best run interactively on [Katana OnDemand](../using_katana/ondemand.md). 

!!! note
    You will need to belong to a group that owns a COMSOL licence (mech, spree, quantum, biomodel) 

## Comsol Batch Jobs

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
