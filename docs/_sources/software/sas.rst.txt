###
SAS
###

The 64-bit version of SAS is available as a module.

By default SAS will store temporary files in :code:`/tmp` which can easily fill up leaving the node offline. In order to avoid this we have set the default to :code:`$TMPDIR` to save temporary files in :code:`/var/tmp` on the Katana head node and local scratch on compute nodes. If you wish to save temporary files to a different location you can do that by using the :code:`-work` flag with your SAS command or adding this line to your :code:`sasv9.cfg` file:

.. code-block:: bash

    -work /my/directory
