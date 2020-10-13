.. _r:

#############
R and RStudio
#############

R is installed as a module. Each version has a number of libraries installed within it.

If you would like a new library installed, please email the `IT Service Centre <ITServiceCentre@unsw.edu.au>`_ with Katana R Module installation in the subject line.

If you would like to use RStudio, we recommend you use the :ref:`Katana OnDemand` service.

If you want to install your own R library for testing or non-general usage, you can use the regular method and the library will be installed locally:

.. code-block:: r
    
    > library('devtools')
    > getwd()
    [1] "/home/z1234567"
    > install('src/mypackage/')
    ...
    * DONE (mypackage)

