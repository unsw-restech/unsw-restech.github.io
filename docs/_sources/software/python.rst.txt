######
Python
######

It is common for python scripts to begin with 

.. code-block:: bash

    #!/usr/bin/python

If you are using a Python module, you will need to change the first line to 

.. code-block:: bash

    #!/usr/bin/env python

or more the likely

.. code-block:: bash

    #!/usr/bin/env python3



******************
Conda and Anaconda
******************

We get a lot of questions about installing Conda and Anaconda. Unfortunately neither are designed to be installed in multi-user environments.

You are able to install them into your home directory and we encourage you to do so.

Alternatively, many packages will give you an option for a :code:`pip install` - if this is an option, we recommend you use :code:`python virtual environments`.


********************
Virtual Environments
********************

We encourage researchers to utilise the power of :ref:`Python Virtual Environments` if they are developing their own software or want to use packages that aren't installed.
