#################
Jupyter Notebooks
#################

Jupyter Notebooks and JupyterLab are best run in :ref:`Katana OnDemand`. 

Katana OnDemand comes with some built in environment and kernels that are 
available for use out of the box.

If you need to use the Jupyter Notebook or Jupyter Lab with your own 
:ref:`Python Virtual Environments` you will need to create your own
Python Jupyter kernel. Here is an example:

***************************************
Create and load the virtual environment
***************************************

.. _code-block: bash

    $ module load python/3.8.3
    $ python3 -m venv --system-site-packages /home/z1234567/.venvs/jupyter-kernel
    $ source /home/z1234567/.venvs/jupyter-kernel/bin/activate

**************************
Create the Jupyter Kernel 
**************************

.. _code-block: bash
    
    python3 -m ipykernel install --prefix=$HOME/.local --name=jlab-kernel

.. _warning:
   
    The :code:`--name=XXXX` isn't strictly necessary, but if you don't use it, 
    the kernel will be called "Python 3" which will cause confusion in the 
    interface.

Now when you load a JupyterLab session, you should see your personal kernel 
in the list of available kernels. This kernel will have access to your
virtual environment.

