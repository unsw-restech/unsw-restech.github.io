#################
Storage Locations
#################

The storage on Katana is split into several different types, each of which serves a different purpose. 

.. important::
    We have just said *each of which serves a different purpose*. Despite that, there will be overlap. And personal preference. In most cases it will be obvious where to put your information. If it isn't and you need help with your decision making, you can `email <rdm@unsw.edu.au>`_ the `Research Data <https://research.unsw.edu.au/research-data-management-unsw>`_ team for advice. They are friendly people.

*****************
Storage  Summary
*****************

+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+
|Storage type           |  Location                   | Alias                     | Purpose                                                  |  Backed up? | Size limit                      |         Who has acess  |
+=======================+=============================+===========================+==========================================================+=============+=================================+========================+
|Home drive             | /home/z1234567              | $HOME                     | Source code and programs                                 |  Y          | 10Gb                            | Only the user          |
+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+
|Global scratch         | /srv/scratch/z1234567       |  /srv/scratch/$USER       | Data files                                               |  N          | 16Tb  between all users         | Only the user          |
+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+
|Shared  scratch        | /srv/scratch/group_name     |  /srv/scratch/group_name  | Data files and programs shared by a team                 |  N          | Upon group requirements         | All users in the group |
+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+
|Local  scratch         | Intialised upon job running |  $TMPDIR                  | Faster job completion with large datasets and temp files |  N          | 200Gb shared between node users | Temporary for each job |
+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+
|UNSW Research Storage  | /home/z1234567/sharename    |  $HOME/sharename          | Storage of shared user and data files                    |  Y          | According to Data Management Plan                        |
+-----------------------+-----------------------------+---------------------------+----------------------------------------------------------+-------------+---------------------------------+------------------------+


.. 
   NOTE: deleted previous description in favour of a quick reference table -KR 
