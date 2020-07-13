#################
Jupyter Notebooks
#################

*******************************************************************************************************
or, the '"it's slightly frustrating, but can be done using the tools" way of running Juypter on Katana'
*******************************************************************************************************


Background
==========

Due to the nature of the Katana system, it's not possible to serve content to the internet from Katana. Juypter notebooks are, essentially, python engines served via http. As a result, getting a notebook working requires a little more work than usual, but should get adequate results. This documentation is minimal, because it points to two other sets of documenation that have more information. WARNING: while it's possible to run Jupyter Notebooks on Katana, it's not an ideal system to run Jupyter on (at the moment). As a result, you may find that you experience is degraded - it's highly dependant on network latency. We would recommend against it and will not be able to provide usability or performance support. We can continue to provide you with Python support, and Jupyter support.  

We need to do a two main steps to prepare, and then each connection will require two steps to connect to your notebook.


Preparation
===========

These two steps are independant of each other - there is no need to do them in order.

1. You will need to be able to connect to Katana via Remote Desktop. There are Instructions on how to do this in :ref:`graphical_session`. 

2. You will need to create a Python Virtual Environment (:ref:`Python Virtual Environments`), activate it, and install Jupyter via the command :code:`pip install jupyter`

Reconnection
============

Everytime you would like to use your Jupyter notebook, you will need to: 

    - Remote Desktop into Katana
    - open a terminal 
    - start an interactive session - :code:`qsub -I` - and wait for it to start
    - in the interactive session, load any relevant modules (if applicable)
    - activate your virtual environment
    - grab the IP address of the node you are on: :code:`hostname -I | awk '{print $1}'`
    - launch Jupyter Notebook like this: :code:`jupyter-notebook --ip=$(hostname -I | awk '{print $1}') --no-browser`
    - copy one of the first two options (the :code:`file:///` or :code:`http://10.197.34.xxx:8888`) into firefox in the remote desktop session

Caveat: if you get an error about a port being busy, launch Jupyter with this command, susbstituting XXXX with a large number: 

.. code-block:: bash

    jupyter-notebook --port=XXXX



IMPORTANT FINAL POINT
=====================

We will forcibly kill Jupyter notebooks we find running on katana1 or katana2 (collectively known as "the head nodes") without warning or explanation beyond this paragraph. All jupyter notebooks must be started in an :ref:`interactive_session` on Katana.
