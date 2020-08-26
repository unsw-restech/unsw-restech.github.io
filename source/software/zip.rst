#############################
Compressing Large Directories
#############################

If you want to compress large directories or directories with a large number of files, we recommend a simple tool called tgzme_ developed by one of our researchers.

It's essentially a smart shell script wrapped around the one line command:

.. code-block:: bash

    tar -c $DIRECTORY | pigz > $DIRECTORY.tar.gz

We thank `Dr. Edwards`_ for his contribution.

.. _tgzme: https://github.com/unsw-edu-au/tgzme/blob/master/tgzme.sh
.. _Dr. Edwards: https://orcid.org/0000-0002-3645-5539
