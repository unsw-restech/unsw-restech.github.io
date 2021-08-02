###################
Installing software
###################

.. Note:: 

   Before installing software yourself, check if it has already been installed in a system-wide module or by one of your research colleagues.


*********************************
Checking if a package is installed
**********************************

.. console:: 

   yum list installed

.. console:: 
   
   module avail




.. warning::

   Do not try to run system commands such as: sudo, su, apt-get install, yumm install


*********************
Compiling from source
*********************



.. note::

   It is best to install your software in your home directory, since it is backed up every night. 


Compilers 
=========
Try to use the system compilers `gcc` and `ld`. However, many codes require specific compilers and versions. We do 




Compile during and interactive session. Use qsub -I.


README and INSTALL files
========================

.. Say README is for quick instructions, INSTALL for detailed instructions


Configuring installation files
==============================
Commonly, a configuration script is available. This allows you to set where the software is installed by using the --prefix flag. Again, it is best to install software in your home directory 


.. console::

   ./configure --prefix=$HOME/apps/{PACKAGE}/{VERSION}



***************************
Installing a binary package
***************************

Using wget...



*********************
Creating module files
*********************

Sample files 

.. console::
   
   /apps/module/templates/module_file


The template module file makes some assumptions and examples, which may not be applicable to your software.

Key sections will likely need to modify are:
* set      basepath          $env(HOME)/apps/{SOFTWARE_NAME}
* set      version           {VERSION_NUMBER}        

* set      url
* set      installed

* set      compiled_with
* set      mpiversion


* prereq     {PREREQUISITE_SOFTWARE)


.. note::

   Insert "module use --append $HOME/apps/Modules" into your ~/.bashrc to your own modules. Enabled with module use upon login.


You should be able to module load your own software module as your own.

*********************
R and Python Packages
*********************


Many R and Python packages are installed on katana. We have specific documentation to install your own packages in :ref:`R<Installing libraries>` and :ref:`Python <pip3 - the Python package manager ("the *Package Installer for Python*")>`






.. note:: If you do need assistance with software install after trying yourself, then send an email to itservicecentre@unsw.edu.au with Katana in the subject line.


.. TO DO: Summarise John and Ducan's guides into the katana docs

