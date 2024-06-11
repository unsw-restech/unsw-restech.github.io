title: Python

It is common for python scripts to begin with 

``` bash
#!/usr/bin/python
```

If you are using a Python module, you will need to change the first line to 

``` bash
#!/usr/bin/env python
```

or, idealy,

``` bash
#!/usr/bin/env python3
```

## Conda and Anaconda

We get a lot of questions about installing Conda and Anaconda. Unfortunately neither are designed to be installed in multi-user environments but you can install them into your 
[your home or scratch directories](../storage/storage_locations/). The advantage of installing Conda in your scratch directory is that
each Conda environment can become quite large which is amplified if you require multiple Conda environments. To install Conda for yourself you can using the following commands.

Start by downloading yout preferred version of Conda, with the two main versions being [Anaconda](https://www.anaconda.com/) and [Miniconda](https://docs.anaconda.com/free/miniconda/).

If you have downloaded Miniconda the instructions are provided below. The instructions for Anaconda are the same once the Conda environment has been created.
``` bash 

bash Miniconda3-latest-Linux-x86_64.sh
[z1234567@katana2 z1234567]$ conda activate
(base) [z1234567@katana2 z1234567]$ conda create --name my_conda
(base) [z9470105@katana1 ~]$ conda activate my_conda
(my_conda) [z9470105@katana1 ~]$ conda install python
```

!!! note
    If you are using Conda for GPU-enabled software, make sure it is installed on a GPU node during an interactive session.


## Python Virtual Environments

Many packages will give you the option option to use `#!bash pip install` - if this is an option, we recommend you use `#!bash python virtual environments` especially if you are developing
your own software or want to use packages that aren't installed.

### Background

To use packages, a collection of files written in Python, not already installed you will need to use what is known as a *virtual environment* or *venv*. This gives us a version of Python in our home directory where
we can install any packages we like. 

### Setting up the default environment
Information on virtual environments can be found in the [Python documentation](https://docs.python.org/3/library/venv.html). To begin with you should run the command 
`#!bash python3 -m venv /path/to/new/virtual/environment`. This directory is where the Python environment is installed and you should not use it as a location to write Python scripts. 

You should consider making a directory to contain your virtual environments using the following command:

``` bash
[z1234567@katana2 ~]$ mkdir /home/z1234567/environments
```

### Setting up the virtual environment - creation and activation

As Katana has multiple Python modules you should pick the one that works best for you. If you don't have any specific requirements then you should choose the most recent one.

``` bash
[z1234567@katana2 ~]$ module load python/3.10.8
[z1234567@katana2 ~]$ which python3
/apps/python/3.10.8/bin/python3
z1234567@katana2 ~]$ python3 -m venv /home/z1234567/environments/my_env
```

!!! note
    The command `which` show the path to a command. You don't need to use it unless you want to confirm where the command it installed.

The next stage is to activate the virtual environment which means that we are using the version of Python that we just created using the command `source /home/z1234567/environments/my_env`. 
After you have activated the virtual environment the prompt changes to make it clear that you are working in your virtual environment. If you use the `which` command with `python3` and `pip3`

``` bash
[z1234567@katana2 ~]$ source /home/z1234567/environments/my_env/bin/activate
```

After activation, python will run from the new location.  the defaults, but the versions in our *venv*

``` bash
(my_env) [z1234567@katana2 ~]$ which python3
/home/z1234567/environments/my_env/bin/python3
```

### pip3 - the Python package manager ("the *Package Installer for Python*")

Using [pip3](https://pypi.org/project/pip) we can see whats installed and install new packages. 

``` bash
(my_env) [z1234567@katana2 ~]$ pip3 list
Package    Version
---------- -------
pip        19.0.3 
setuptools 40.8.0 
You are using pip version 19.0.3, however version 20.0.2 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```

If it says that a newer verions of pip is available then you should upgrade.

``` bash
(my_env) [z1234567@katana2 ~]$ pip3 install --upgrade pip
Collecting pip
    Downloading https://files.pythonhosted.org/packages/54/0c/d01aa759fdc501a58f431eb594a17495f15b88da142ce14b5845662c13f3/pip-20.0.2-py2.py3-none-any.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 1.5MB/s 
Installing collected packages: pip
    Found existing installation: pip 19.0.3
    Uninstalling pip\-19.0.3:
    Successfully uninstalled pip\-19.0.3
Successfully installed pip-20.0.2
(venv-tutorial-1) [z1234567@katana2 ~]$ pip install --upgrade setuptools
Collecting setuptools
    Downloading setuptools-46.1.1-py3-none-any.whl (582 kB)
        |████████████████████████████████| 582 kB 13.5 MB/s 
Installing collected packages: setuptools
    Attempting uninstall: setuptools
    Found existing installation: setuptools 40.8.0
        Uninstalling setuptools\-40.8.0:
            Successfully uninstalled setuptools\-40.8.0
Successfully installed setuptools-46.1.1
(venv-tutorial-1) [z1234567@katana2 w~]$ pip3 list
Package    Version
---------- -------
pip        20.0.2 
setuptools 46.1.1 
```

### Installing software

To install a package that you want to use you can run the command `pip3 install ...`:

``` bash
(my_env) [z1234567@katana2 ~]$ pip3 install numpy
Collecting numpy
    Downloading numpy-1.18.2-cp37-cp37m-manylinux1*x86*64.whl (20.2 MB)
        |████████████████████████████████| 20.2 MB 38 kB/s 
Installing collected packages: numpy
Successfully installed numpy-1.18.2
(my_env) [z1234567@katana2 ~]$ pip3 list
Package    Version
---------- -------
numpy      1.18.2 
pip        20.0.2 
setuptools 46.1.1 
```

### Exiting the venv, and coming around again


To leave a venv, you use the `deactivate` command like this:

``` bash
(my_env) [z1234567@katana2 ~]$ deactivate 
[z1234567@katana2 ~]$
```

You can create additional virtual environments in the same way and we can see both environments.
``` bash
[z1234567@katana2 ~]$ python3 -m venv /home/z1234567/environments/my_new_env
[z1234567@katana2 ~]$ ls -l /home/z1234567/environments
total 0
drwx------. 5 z1234567 unsw 69 Mar 23 15:07 my_env
drwx------. 5 z1234567 unsw 69 Mar 23 11:45 my_new_env

[z1234567@katana2 ~]$ source /home/z1234567/environments/my_new_env/bin/activate
(my_new_env) [z1234567@katana2 ~]$ pip3 list
Package    Version
---------- -------
pip        19.0.3 
setuptools 40.8.0 
You are using pip version 19.0.3, however version 20.0.2 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
```

When we install SciPy it will automatically install NumPy as it is a ependency.

``` bash
(scipy-example) [z1234567@katana2 ~]$ pip install scipy
Collecting scipy
    Downloading scipy-1.4.1-cp37-cp37m-manylinux1*x86*64.whl (26.1 MB)
        |████████████████████████████████| 26.1 MB 95 kB/s 
Collecting numpy>=1.13.3
    Using cached numpy-1.18.2-cp37-cp37m-manylinux1*x86*64.whl (20.2 MB)
Installing collected packages: numpy, scipy
Successfully installed numpy-1.18.2 scipy-1.4.1
(scipy-example) [z1234567@katana2 ~]$ pip list
Package    Version
---------- -------
numpy      1.18.2 
pip        20.0.2 
scipy      1.4.1  
setuptools 46.1.1 
```

You can install an older version by specifying the version that you want to install.
``` bash
(old-scipy-example) [z1234567@katana2 ~]$ pip install scipy==1.2.3
Collecting scipy==1.2.3
    Downloading https://files.pythonhosted.org/packages/96/e7/e06976ab209ef44f0b3dc638b686338f68b8a2158a1b2c9036ac8677158a/scipy-1.2.3-cp37-cp37m-manylinux1_x86_64.whl (24.8MB)
    100% |████████████████████████████████| 24.8MB 239kB/s 
Collecting numpy>=1.8.2 (from scipy==1.2.3)
    Using cached https://files.pythonhosted.org/packages/b7/ce/d0b92f0283faa4da76ea82587ff9da70104e81f59ba14f76c87e4196254e/numpy-1.18.2-cp37-cp37m-manylinux1_x86_64.whl
Installing collected packages: numpy, scipy
Successfully installed numpy-1.18.2 scipy-1.2.3
(old-scipy-example) [z1234567@katana2 src]$ pip list
Package    Version
---------- -------
numpy      1.18.2 
pip        20.0.2 
scipy      1.2.3  
setuptools 46.1.1 
```

## Special Cases

If you want your virtual environment to take advantage of a module that has been installed on Katana.

In that case, your workflow would be:

 - load the module
 - create the Virtual Environment with the flag `--system-site-packages`
 - install software in question

For example, using samtools and wanting to run your virtual environment in Jupyter:

``` bash
[z1234567@katana2 ~]$ module add samtools/1.15.1
[z1234567@katana2 ~]$ python3 -m venv /home/z1234567/environments/my_samtools_env --system-site-packages
[z1234567@katana2 ~]$ source /home/z1234567/environments/my_samtools_env/bin/activate
(my_samtools_env) [z1234567@katana2 ~]$ pip3 install pysam
```

You can now create a [Jupyter kernel](./jupyter-notebooks.md) which will ellow you to use the virtual environment that you have created.
