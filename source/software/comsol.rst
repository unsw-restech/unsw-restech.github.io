#######
Comsol
#######

Comsol is best run interactively on :ref:`Katana OnDemand`. 

.. note::

   You will need to belong to a group that owns a COMSOL licence (mech, spree, quantum, biomodel) 


.. TO DO


.. code-block:: console
        
        mkdir -p ${TMPDIR}/comsol

        export MY_COMSOL_DIR=/srv/scratch/$USER/comsoldir

        module load comsol/5.6-spree

        comsol -nn 1 -np $NCPUS \
        -recoverydir ${MY_COMSOL_DIR}/recoveries \
        -tmpdir ${TMPDIR}/comsol \
        batch \
        -inputfile ${MY_COMSOL_DIR}/MyModel.mph \
        -outputfile ${MY_COMSOL_DIR}/MyModelOut.mph \
        -batchlog ${MY_COMSOL_DIR}/MyModel.log
                

