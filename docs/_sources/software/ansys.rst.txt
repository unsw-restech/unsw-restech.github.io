#####
Ansys
#####

Both Ansys Workbench and Ansys Electronic Desktop are available on :ref:`Katana OnDemand`. 

This is the most user-friendly approach to running Ansys jobs on Katana.

.. You can run an interactive job, starting Ansys with 'runwb2', but this is not recommended!

.. Could use for quick guides and images https://web.aeromech.usyd.edu.au/AMME5981/Course_Documents/files/Tutorial%20Week%203c%20-%20MECH3361%20Workbench%20Guide.pdf


****************
Ansys Batch Jobs
****************

Ansys workbench is available on `myAccess <https://www.myaccess.unsw.edu.au/applications/ansys-workbench>`_ for undergraduate and postgraduate students in the Faculty of Engineering. 

If you install Ansys on your local machine, you can generate your model files locally and transfer them tto katana to run in a batch job. 

---------
Ansys CFX
---------
For Ansys CFX the .cfx and .def files need to be transferred to Katana.

A brief batch script example is given below. A step-by-step guide is availabe `on our GitHub <https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-cfx.md>`_.

.. code-block:: bash

   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load intelmpi/2019.6.166
   module load ansys/2021r1

   cfx5solve -batch -def <filename>.def -part $NCPUS -start-method "Intel MPI Local Parallel"

---------
Ansys Fluent
---------
Similarly, Ansys Fluent input can be generated locally and transferred to Katana to run in a batch job.

Again, a brief batch script is below, and a step-by-step guide is available `on our Github <https://github.com/unsw-edu-au/Restech-HPC/blob/master/hpc-examples/ansys/ansys-fluent.md>`_. 

.. code-block:: bash 

   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load ansys/2021r1
   module load intelmpi/2019.6.166

   fluent 3d -g -t $NCPUS -i fluent.in > output.out


************
Ansys on Gadi
*************

If you wish to use Ansys on NCI's Gadi, UNSW has a institutional licence that requies you join the relevant software group as detailed in `NCI's documentation <opus.nci.org.au/display/Help/Ansys+Fluent>_`.

