.. _about_katana:

############
About Katana
############

Katana is a shared computational cluster located on campus at UNSW that has been designed to provide easy access to computational resources for groups working with non-sensitive data. It contains over 6,000 CPU cores, 8 GPU compute nodes (V100 and A100), and 6Pb of disk storage. Katana provides a flexible compute environment where users can run jobs that wouldn't be possible or practical on their desktop or laptop. For full details of the compute nodes including a full list see the compute node information section below.

Katana is powerful on its own, but can be seen as a training or development base before migrating up to systems like Australia's peak HPC system Gadi_, located at NCI_. Research Technology Services also provide training, advice and support for new users or those uncertain if High Performance Computing is the right fit for their research needs.

.. _system_configuration:

********************
System Configuration
********************

- RPM based Linux OSes. RedHat on the management plane, CentOS on the nodes
- PBSPro_ version 19.1.3
- Large global scratch at :code:`/srv/scratch`, local scratch at :code:`$TMPDIR`
- 12, 48, 100, 200 hour :term:`Walltime` queues with prioritisation

.. _compute_resources:

*******
Compute
*******

- Heterogenous hardware: Dell, Lenovo, Huawei.
- Roughly 170 nodes

.. _gpu_resources:

***********
GPU Compute
***********

The most popular use of these nodes is for Tensorflow.

- Eight GPU capable nodes

  - Tesla V100-SXM2, 32GB
  - Nvidia A100, 40GB
- Five are dedicated for the department that owns them
- Three are general use for all researchers

You cannot use Tensorflow on the login nodes because they don't have GPUs. You will need to get access to a GPU node to do this. 

.. warning::

    Unfortunately, GPU nodes are in incredibly high demand. 
    We cannot provide special accommodation for any project. 
    You will need to wait in the common queue - or buy
    a GPU node for your group on which you will get priority

To access a GPU node interactively, you can use a command like

.. code-block:: bash

    [z1234567@katana ~]$ qsub -I -l select=1:ncpus=8:ngpus=1:mem=46gb,walltime=2:00:00

Note the 2 hour limit - that is the *fastest* way to get onto the GPU nodes. Unfortunately, there's no way to tell you that 
your session has started, so you will need to monitor your command.

.. We **know** that this isn't ideal and we **wish** there was an easier solution - we love making your lives easier. It's literally our jobs. But in this case, we don't have the resources available to make this faster, smoother or easier.  
.. note:: 

   GPU-enabled software should be installed on a GPU node within an interactive session, in case the software is probing for GPU hardware or libraries.

.. _Gadi: https://nci.org.au/our-systems/hpc-systems
.. _NCI: https://nci.org.au/
.. _PBSPro: https://www.pbspro.org/
