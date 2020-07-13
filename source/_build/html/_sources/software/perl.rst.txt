####
Perl
####

The default version of Perl on Katana is 5.16.3 which is provided by CentOS 7 and can be found at :code:`/usr/bin/perl`.

This is an older version of Perl. We have Perl 5.28.0 installed as a module. 

It is common for perl scripts to begin with 

.. code-block:: bash

    #!/usr/bin/perl

If you are using the Perl module, you will need to change the first line to 

.. code-block:: bash

    #!/usr/bin/env perl
