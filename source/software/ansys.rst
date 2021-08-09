#####
Ansys
#####

Both Ansys Workbench and Ansys Electronic Desktop are available  on :ref:`Katana OnDemand`. 


You can run an interactive job, starting Ansys with 'runwb2', but this is not recommended!



.. Using https://web.aeromech.usyd.edu.au/AMME5981/Course_Documents/files/Tutorial%20Week%203c%20-%20MECH3361%20Workbench%20Guide.pdf




****************
Ansys Batch Jobs
****************

Ansys workbench is available on `myAccess <https://www.myacess.unsw.edu.au/applications/ansys-workbench>_` for undergraduate and postgraduate students in the Faculty of Engineering.

Ansys CFX

.. code-block:: bash

   #!/bin/bash
   #<RESOURCE REQUESTS>

   cd $PBS_O_WORKDIR

   module load ansys/2021r1

   cfx5solve -batch -def <filename>.def -part 
   -start-method 

Ansys Fluent

.. code-block:: bash 

   fluent 3d -g -t 8 -i fluent.in > output.out


************
Ansys on Gadi
*************

If you wish to use Ansys on NCI's Gadi, UNSW has a institutional licence that requies you join the relevant software group as detailed in `NCI's documentation <opus.nci.org.au/display/Help/Ansys+Fluent>_` 





.. Batch Job



