title: Jupyter Notebooks

Jupyter Notebooks and JupyterLab are best run via [Katana OnDemand](../using_katana/ondemand.md). 

When you use Jupyter Notebooks and JupyterLab on Katana OnDemand it comes with some built in environments and kernels 
that are available for use. If none of these work for you then you can install your own environment or kernel.

## Python virtual environments

If you need to use the Jupyter Notebook or Jupyter Lab with your own 
[Python Virtual Environments](./python.md#python-virtual-environments) you will need to create your own
Python Jupyter kernel using the instructions below:

### Create and load the virtual environment

``` bash
$ module load python/3.8.3
$ python3 -m venv --system-site-packages /home/z1234567/.venvs/jupyter-kernel
$ source /home/z1234567/.venvs/jupyter-kernel/bin/activate
$ pip install ipykernel
```

!!! note "Create the Jupyter Kernel "
    === "Using the helper script"

        This script can automatically setup Jupyter kernels for use in Katana OnDemand.

        ``` bash
        (jupyter-kernel) $ install_jupyter_kernels
        ```
    === "Installing the kernel manually"
        ``` bash
        (jupyter-kernel) $ python3 -m ipykernel install --prefix=$HOME/.local --name=jlab-kernel
        ```

        !!! warning
            The `--name=XXXX` isn't strictly necessary, but if you don't use it, 
            the kernel will be called "Python 3". This will not be distinguishable
            from the Katana supplied Python 3 and could cause confusion.

Now when you load a JupyterLab session, you should see your personal kernel 
in the list of available kernels. This kernel will have access to your
virtual environment.

## Conda environments

If you need to use the Jupyter Notebook or Jupyter Lab with your own
Conda environment you will need to create your own Python or R Jupyter kernel.
Here is an example:

### Create and activate the environment

``` bash
$ conda create -n my_conda_env -c conda-forge ipykernel
$ conda activate my_conda_env

```
Once you have activated your Conda environment you will need to create your Jupyter kernal using one of the commands listed above.

### Other Jupyter Kernels

Kernels other than ``ipykernel``, need ``jupyter`` to be installed in addition to the kernel.

For example, to use the R kernel ``r-irkernel``, you must also install the ``jupyter`` package before running the helper script.

``` bash
(my_conda_env) $ conda install jupyter
```