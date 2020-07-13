####
Java
####

Java is installed as part of the Operating System but we would strongly recommend against using that version - we cannot guarantee scientific reproducibility with that version. Please use the java modules. 

Each Java module sets 

.. code-block:: bash
    
    _JAVA_TOOL_OPTIONS -Xmx1g

This sets the heap memory to 1GB. If you need more, set the environment variable :code:`_JAVA_OPTIONS` which overrides :code:`_JAVA_TOOL_OPTIONS`

.. code-block:: bash

    export _JAVA_OPTIONS="-Xmx5g"

