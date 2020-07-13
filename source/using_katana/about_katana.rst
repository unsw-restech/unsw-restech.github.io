.. _about_katana:

############
About Katana
############

Katana is a shared computational cluster located on campus at UNSW that has been designed to provide easy access to computational resources. With over 4000 CPU cores spread over a large number of compute nodes each with up to 1Tb of memory, Katana provides a flexible compute environment where users can run jobs that wouldn't be possible or practical on a desktop or laptop. 

Katana is powerful on it's own, but can be seen as a training or development base before migrating up to systems like as Australia's peak HPC system Gadi_, located at NCI_. Research Technology Services also provide training, advice and support for new users or those uncertain if High Performance Computing is the right fit for their research needs.

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

- Hetergenous hardware: Dell, Lenovo, Huawei.
- roughly 170 nodes

.. _gpu_resources:

***********
GPU Compute
***********

The most popular use of these nodes is for Tensorflow.

- four GPU capable nodes, Tesla V100-SXM2, 32GB
- three are dedicated for the department that owns them
- one is general use for all researchers

.. _Gadi: https://nci.org.au/our-systems/hpc-systems
.. _NCI: https://nci.org.au/
.. _PBSPro: https://www.pbspro.org/
