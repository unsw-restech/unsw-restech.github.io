###########################
Python Virtual Environments
###########################

**************************************************************************************************
or how to run a python library until the admins install it (or what to do if they wont install it)
**************************************************************************************************

Background
==========

Sometimes you will need to use a particular version of Python, or you will need to use a set of Python libraries that aren't available in the provided installation.

In these cases, we can use what's known as a *virtual environment* or *venv*. A venv gives us a static version of Python in our home directory in which we can install any packages we like. 

In this tutorial we will see how to set up a venv and explain what's happening under the hood. Then we will show how you can use one in your development process. As a note, in this walk through we will refer to packages and libraries as "packages". When we use these terms, we are referring to a collection of files, written in Python, usually with some versions and potentially some requirements of their own. 

Setting up the default environment
==================================

According to the `Python documentation <https://docs.python.org/3/library/venv.html>`_, we will run something like this :code:`python3 -m venv /path/to/new/virtual/environment`. This is not a directory that we need to see or need to spend time in actively, so it's ok to make it hidden. This is an important distinction - the *venv* should not be where you are doing your development. It's meant to be flexible - as soon as you fill it with development code, it's no longer flexible. Also, you want your development code backed up or in a repository - it is unnecessary bloat to add the Python software to that backup or repository.

We will make a directory in which we can keep many venvs. What I found was that once I started using venvs, it didn't make any sense to do Python development without them.

In Linux we can make a directory or file invisible by naming it with a leading dot:

.. code-block:: bash

    [z1234567@katana2 ~]$ mkdir /home/z1234567/.venvs/


Setting up the virtual environment - creation and activation
============================================================

I'll be using the latest version of Python available to me. Since this is one of the modules that we offer, I can use it with the understanding that it will be there indefinitely.

.. code-block:: bash

    [z1234567@katana2 ~]$ module load python/3.7.4
    [z1234567@katana2 ~]$ which python3
    /apps/python/3.7.4/bin/python3
    z1234567@katana2 ~]$ python3 -m venv /home/z1234567/.venvs/venv-tutorial-1


That's it, we are done. If you want to take a look under the hood, see `what's in my virtualenv? <python-virtualenvs-internals.rst>`_

Next, we need to **activate** our venv. This makes our virtualenv our current environment. To activate, we execute :code:`source /path/to/venv/bin/activate`. Note that after activation, the prompt changes to make it clear you are now in a venv. You can see the change in which versions of :code:`python3` and :code:`pip3` are available before and after activation:

Before we activate our environment

.. code-block:: bash

    [z1234567@katana2 ~]$ which python3; which pip3
    /apps/python/3.7.4/bin/python3
    /apps/python/3.7.4/bin/pip3


Activation

.. code-block:: bash

    [z1234567@katana2 ~]$ source ~/.venvs/venv-tutorial-1/bin/activate


After activation, our python binaries are not the defaults, but the versions in our *venv*

.. code-block:: bash

    (venv-tutorial-1) [z1234567@katana2 ~]$ which python3; which pip3
    ~/.venvs/venv-tutorial-1/bin/python3
    ~/.venvs/venv-tutorial-1/bin/pip3


pip3 - the Python package manager ("the *Package Installer for Python*")
========================================================================

Using `pip3 <https://pypi.org/project/pip/>`_ we can see whats installed and install new packages. You will often see packages give installation advice for pip (Conda is another popular system).


Now that we are using the venv, we can list what's in the venv, and then install a new package:

.. code-block:: bash

    (venv-tutorial-1) [z1234567@katana2 ~]$ pip3 list
    Package    Version
    ---------- -------
    pip        19.0.3 
    setuptools 40.8.0 
    You are using pip version 19.0.3, however version 20.0.2 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.


At this point - before any work is done, and while using your venv - it's a great time to perform that update.

.. code-block:: bash

    (venv-tutorial-1) [z1234567@katana2 ~]$ pip install --upgrade pip
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


Installing software
===================

And then package installation is as easy as using :code:`pip install ...`:

.. code-block:: bash

    (venv-tutorial-1) [z1234567@katana2 ~]$ pip install numpy
    Collecting numpy
      Downloading numpy-1.18.2-cp37-cp37m-manylinux1*x86*64.whl (20.2 MB)
    	 |████████████████████████████████| 20.2 MB 38 kB/s 
    Installing collected packages: numpy
    Successfully installed numpy-1.18.2
    (venv-tutorial-1) [z1234567@katana2 ~]$ pip list
    Package    Version
    ---------- -------
    numpy      1.18.2 
    pip        20.0.2 
    setuptools 46.1.1 


Exiting the venv, and coming around again
=========================================

To leave a venv, you use the :code:`deactivate` command like this:

.. code-block:: bash

    (venv-tutorial-1) [z1234567@katana2 ~]$ deactivate 
    [z1234567@katana2 ~]$

Notice how the prompt returned to the way it was? Let's create a new venv:

.. code-block:: bash

    [z1234567@katana2 ~]$ python3 -m venv /home/z1234567/.venvs/scipy-example
    [z1234567@katana2 ~]$ ls -l ~/.venvs/
    total 0
    drwx------. 5 z1234567 unsw 69 Mar 23 15:07 scipy-example
    drwx------. 5 z1234567 unsw 69 Mar 23 11:45 venv-tutorial-1
    [z1234567@katana2 ~]$ source ~/.venvs/scipy-example/bin/activate
    (scipy-example) [z1234567@katana2 src]$ 
    (scipy-example) [z1234567@katana2 src]$ pip list
    Package    Version
    ---------- -------
    pip        19.0.3 
    setuptools 40.8.0 
    You are using pip version 19.0.3, however version 20.0.2 is available.
    You should consider upgrading via the 'pip install --upgrade pip' command.


When we install SciPy, it automatically knows to install NumPy, a dependency:

.. code-block:: bash

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


If you want to install an older version, it's relatively easy

.. code-block:: bash

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


That's a quick introduction to how you can install Python packages locally. 


Special Cases
=============

Say for instance you want to use software X in a Jupyter Notebook. X is already

installed on Katana.

In that case, your workflow would be:

 - load the module in question

 - create the Virtual Environment with the flag :code:`--system-site-packages`

 - install software in question with an understanding that you might not be able to get the latest release

For example, using the Katana TensorFlow installation and a desire for Jupyter:

.. code-block:: bash

    [z1234567@katana1 ~]$ module load tensorflow/1.14gpu
    [z1234567@katana1 ~]$ python3 -m venv /home/z1234567/.venvs/tf --system-site-packages
    [z1234567@katana1 ~]$ source ~/.venvs/tf/bin/activate
    (tf) [z1234567@katana2 ~]$ pip install jupyter


This will throw errors because there are a collection of packages missing in relation to the latest Jupyter. They shouldn't affect your ability to run :ref:`Jupyter Notebooks` with tensorflow.
