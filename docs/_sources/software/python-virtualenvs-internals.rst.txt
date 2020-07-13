####################################
Virtual Environments from the inside
####################################

We've built a venv in our :code:`~/.venvs` directory. Let's take a look inside. This presumes you have used the command 

.. code-block:: bash

    [z1234567@katana2 src]$ python3 -m venv /home/z1234567/.venvs/venv-tutorial-1

to set up your virtualenv.

Here is a quick overview of the basics.

We can see there is a directory in :code:`~/.venvs` that has the same name as the virtualenv we created.

.. code-block:: bash

    [z1234567@katana2 ~]$ ls -l ~/.venvs/
    total 0 
    drwx------.  5 z1234567 unsw   69 Mar 23 11:45 venv-tutorial-1

Inside that directory we can see some more directories. The two important directories here are :code:`bin` and :code:`lib`.

.. code-block:: bash

    [z1234567@katana2 ~]$ ls -l ~/.venvs/venv-tutorial-1/
    total 8
    drwx------. 2 z1234567 unsw 4096 Mar 23 11:45 bin
    drwx------. 2 z1234567 unsw    6 Mar 23 11:45 include
    drwx------. 3 z1234567 unsw   22 Mar 23 11:45 lib
    lrwxrwxrwx. 1 z1234567 unsw    3 Mar 23 11:45 lib64 -> lib
    -rw-------. 1 z1234567 unsw   83 Mar 23 11:45 pyvenv.cfg

In :code:`bin` you will see executables. The main one of note is :code:`activate`.

.. code-block:: bash

    [z1234567@katana2 ~]$ ls -l ~/.venvs/venv-tutorial-1/bin/
    total 36
    drwx------. 2 z1234567 unsw 4096 Mar 23 11:45 .
    drwx------. 5 z1234567 unsw   69 Mar 23 11:45 ..
    -rw-r--r--. 1 z1234567 unsw 2235 Mar 23 11:45 activate
    -rw-r--r--. 1 z1234567 unsw 1291 Mar 23 11:45 activate.csh
    -rw-r--r--. 1 z1234567 unsw 2443 Mar 23 11:45 activate.fish
    -rwxr-xr-x. 1 z1234567 unsw  266 Mar 23 11:45 easy_install
    -rwxr-xr-x. 1 z1234567 unsw  266 Mar 23 11:45 easy_install-3.7
    -rwxr-xr-x. 1 z1234567 unsw  248 Mar 23 11:45 pip
    -rwxr-xr-x. 1 z1234567 unsw  248 Mar 23 11:45 pip3
    -rwxr-xr-x. 1 z1234567 unsw  248 Mar 23 11:45 pip3.7
    lrwxrwxrwx. 1 z1234567 unsw    7 Mar 23 11:45 python -> python3
    lrwxrwxrwx. 1 z1234567 unsw   30 Mar 23 11:45 python3 -> /apps/python/3.7.4/bin/python3

In :code:`lib` we need to traverse a few more directories, but eventually we will see where the packages are installed. As you can see, :code:`pip` and :code:`setuptools` are already installed. These are the default:

.. code-block:: bash

    [z1234567@katana2 ~]$ ls -l ~/.venvs/venv-tutorial-1/lib/python3.7/site-packages/
    total 16
    -rw-------. 1 z1234567 unsw  126 Mar 23 11:45 easy_install.py
    drwx------. 5 z1234567 unsw   90 Mar 23 11:45 pip
    drwx------. 2 z1234567 unsw 4096 Mar 23 11:45 pip-19.0.3.dist-info
    drwx------. 5 z1234567 unsw   89 Mar 23 11:45 pkg_resources
    drwx------. 2 z1234567 unsw   40 Mar 23 11:45 **pycache**
    drwx------. 6 z1234567 unsw 4096 Mar 23 11:45 setuptools
    drwx------. 2 z1234567 unsw 4096 Mar 23 11:45 setuptools-40.8.0.dist-info
