#################
Storage Locations
#################

The storage on Katana is split into several different types, each of which serves a different purpose. 

.. important::
    We have just said *each of which serves a different purpose*. Despite that, there will be overlap. And personal preference. In most cases it will be obvious where to put your information. If it isn't and you need help with your decision making, you can `email <rdm@unsw.edu.au>`_ the `Research Data <https://research.unsw.edu.au/research-data-management-unsw>`_ team for advice. They are friendly people.


******************
Cluster Home Drive
******************

Location
========

.. code-block:: bash

    /home/z1234567

Also known as
=============

.. code-block:: bash

    $HOME

.. _Cluster Attributes:

Attributes
==========

- Backed up
- 10Gb limit
- By default only user has access
- Able to be used everywhere on Katana

.. _Cluster Best used for:

Best used for
=============

Source code and programs

**************
Global Scratch
**************

.. _Global Location:

Location
========

.. code-block:: bash

    /srv/scratch/z1234567

.. _Global Also known as:

Also known as
=============

.. code-block:: bash

    /srv/scratch/$USER

.. _Global Attributes:

Attributes
==========

- Not available to users in STUDENT groups such as ESTUDENT.
- Not backed up
- Generally 16Tb shared between multiple users
- By default only user has access 
- Able to be used everywhere on Katana

.. _Global Best used for:

Best used for
=============

Data files


**************
Shared Scratch
**************

Only available to groups of users that have requested it - primarily for teams that share data sets or results.

.. _Shared Location:

Location
========

.. code-block:: bash

    /srv/scratch/name

.. _Shared Attributes:

Attributes
==========

- Not backed up
- Spaced based on group requirements
- All users in the group have access 
- Able to be used everywhere on Katana

.. _Shared Best used for:

Best used for
=============

Shared data files and copies of programs


*************
Local Scratch
*************

Found on the node on which your job is running. 

.. _Local Location:

Location
========

The location is created by the job scheduler as part of initialising the running of the job.

.. _Local Also known as:

Also known as
=============

.. code-block:: bash

    $TMPDIR

.. _Local Attributes:

Attributes
==========

- Not backed up
- Only exists whilst job is running
- 200Gb shared between node users
- Storage located on compute node so good for compute

.. _Local Best used for:

Best used for
=============

Much fast completion of jobs that require large datasets to be near the CPU, calculations and temp files.

*********************
UNSW Research Storage
*********************

Location
========

.. code-block:: bash

    /home/z1234567/sharename

Also known as
=============

.. code-block:: bash

    $HOME/sharename

Attributes
==========

- Backed up
- Only available on Katana head node

Best used for
=============

Shared and user data files.

