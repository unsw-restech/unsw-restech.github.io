#####
Stata
#####

Stata is availt as a module. 

When using Stata in a pbs batch script, the syntax is

.. code-block:: bash

    stata -b do StataClusterWorkshop.do

If you wish to load or install additional Stata modules or commands you should use findit command on your local computer to find the command that you are looking for. Then create a directory called :code:`myadofiles` in your home directory and copy the .ado (and possibly the .hlp) file into that directory. Now that the command is there it just remains to tell Stata to look in that directory which can be done by using the following Stata command.

.. code-block:: bash

    sysdir set PERSONAL $HOME/myadofiles
