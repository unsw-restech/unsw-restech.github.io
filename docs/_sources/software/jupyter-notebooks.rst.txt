#################
Jupyter Notebooks
#################

.. contents:: Table of Contents
    :depth: 2
    :local:

Jupyter Notebooks and JupyterLab are best run in :ref:`Katana OnDemand`. 

Katana OnDemand comes with some built in environment and kernels that are 
available for use out of the box.

===========================
Python virtual environments
===========================

If you need to use the Jupyter Notebook or Jupyter Lab with your own 
:ref:`Python Virtual Environments` you will need to create your own
Python Jupyter kernel. Here is an example:

***************************************
Create and load the virtual environment
***************************************

.. code-block:: bash

    $ module load python/3.8.3
    $ python3 -m venv --system-site-packages /home/z1234567/.venvs/jupyter-kernel
    $ source /home/z1234567/.venvs/jupyter-kernel/bin/activate

**************************
Create the Jupyter Kernel 
**************************

+++++++++++++++++++++++
Using the helper script
+++++++++++++++++++++++

This script can automatically setup Jupyter kernels for use in Katana OnDemand.

.. code-block:: bash

    (jupyter-kernel) $ install_jupyter_kernels


++++++++++++++++++++++++++++++
Installing the kernel manually
++++++++++++++++++++++++++++++

.. code-block:: bash
    
    (jupyter-kernel) $ python3 -m ipykernel install --prefix=$HOME/.local --name=jlab-kernel

.. warning::
   
    The :code:`--name=XXXX` isn't strictly necessary, but if you don't use it, 
    the kernel will be called "Python 3". This will not be distinguishable
    from the Katana supplied Python 3 and could cause confusion.

Now when you load a JupyterLab session, you should see your personal kernel 
in the list of available kernels. This kernel will have access to your
virtual environment.

==================
Conda environments
==================

If you need to use the Jupyter Notebook or Jupyter Lab with your own
Conda environment you will need to create your own Python or R Jupyter kernel.
Here is an example:

***********************************
Create and activate the environment
***********************************

.. code-block:: bash

    $ conda create -n my_conda_env -c conda-forge ipykernel
    $ conda activate my_conda_env


**************************
Create the Jupyter Kernel 
**************************

You can alternatively install the kernel manually: `Installing the kernel manually`_

.. code-block:: bash

    (my_conda_env) $ install_jupyter_kernels

.. note::

    Kernels other than ``ipykernel``, need ``jupyter_core`` to be installed in addition to the kernel.

    For example, to use the R kernel ``r-irkernel``, you must also install the ``jupyter_core`` package before running the helper script.

    .. code-block:: bash

        (my_conda_env) $ conda install jupyter_core
