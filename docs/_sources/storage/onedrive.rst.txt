#########
One Drive
#########

.. comment: potentially should refer to as UNSW OneDrive, but would need to update the link names

You can mount Microsoft's OneDrive_ on Katana.

*******************
OneDrive Background
*******************

We don't recommend researchers mount their OneDrive because the configuration process is not ideal. However, it is possible with the following method.

The configuration section will need to be run infrequently, probably only once.

The how-to-run section will need to be run each time.

`There are caveats to using RClone with OneDrive`_ that users will need to be 
aware of and which the UNSW team cannot help with.

And there is one local caveat that needs to be understood:

Mounting OneDrive locally will only work on the machine on which the `mount`
command is run.
    
.. note:: Please mount OneDrive on Katana Data Mover, not the login nodes. 
        
        
.. .. warning:: In practice, this has implications. All of our other common 
    drives, like /apps, /home and /srv/scratch, will automatically mount on all
    machines. OneDrive only mounts on the machine you mount it on. If you mount
    it on the login node katana1, it will not be available on katana2 or any of 
    the worker nodes. You will need to mount OneDrive within your PBS script.


*************
Prerequisites
*************

1. `OneDrive needs your consent`_ to give rclone access to your files. 

2. We can't guarantee or test if this works for researchers in Germany or 
China. OneDrive has a different set up for Germany and China due to local laws 
about data storage.

3. You will need to install rclone in your home directory on katana as the 
version in the CentOS/RHEL operating system is too old.

4. You will need at least one empty directory for mounting. Restech recommends:

- you create a directory /home/z1234567/bin to put rclone into
- you add /home/z1234567/bin to your PATH
- you create a directory /home/z1234567/OneDrive to mount OneDrive onto.

5. You will need to ask us for the secret key. Please don't write it down.

**************
Install RClone
**************

1. Go to this page and copy the link to the latest version for 
`Intel/AMD - 64 Bit Linux`. Currently that looks like this:

https://downloads.rclone.org/v1.55.0/rclone-v1.55.0-linux-amd64.zip

2. On Katana, unzip

.. code-block:: bash

        [z1234567@katana1 ~]$ wget https://downloads.rclone.org/v1.55.0/rclone-v1.55.0-linux-amd64.zip
        [z1234567@katana1 ~]$ unzip rclone-v1.55.0-linux-amd64.zip
		Archive:  rclone-v1.55.0-linux-amd64.zip
			creating: rclone-v1.55.0-linux-amd64/
			inflating: rclone-v1.55.0-linux-amd64/rclone.1
			inflating: rclone-v1.55.0-linux-amd64/README.txt
			inflating: rclone-v1.55.0-linux-amd64/git-log.txt
			inflating: rclone-v1.55.0-linux-amd64/rclone
			inflating: rclone-v1.55.0-linux-amd64/README.html

3. Install into a local directory

.. code-block:: bash

    [z1234567@katana1 ~]$ cp rclone-v1.55.0-linux-amd64/rclone ~/bin/

4. Give OneDrive consent to allow rclone access to your files

*****************************
Configure RClone for OneDrive
*****************************
            
1. Login to Katana and run:

.. code-block:: bash
    
        [z1234567@katana1 ~]$ rclone config

You will be asked a set of questions. The short answers are:

1. new remote ('n')
2. name ('OneDrive')
3. storage type **Microsoft OneDrive** ('26' in rclone version 1.55)
4. OAuth Client Id ("c8800f43-7805-46c2-b8b2-1c55f3859a4c")
5. OAuth Client Secret (This is actually a secret. You will need to ask ResTech)
6. Choose national cloud region for OneDrive ("1. global")
7. Edit advanced config? ("n")
8. Remote config? ("n")
9. Paste result: 
10. Choose a number from below, or type in an existing value ("1. OneDrive Personal or Business")
11. Chose drive to use: ("0")
12. Is that okay? ("Y")

You should then see something like this to which you should answer yes:

.. code-block:: bash

   --------------------
    [MS OneDrive]
    type = onedrive
    client_id = c8800f43-7805-46c2-b8b2-1c55f3859a4c
    client_secret = SECRET
    region = global
    token = {"access_token":"eyJ0e...asdasd"}
    drive_type = business
    --------------------
    y) Yes this is OK (default)
    e) Edit this remote
    d) Delete this remote
    y/e/d> 


*********************
How to mount OneDrive
*********************

Once logged in or in a pbs script:

1. Mount the drive. The basic syntax is:

.. code-block:: bash

    rclone mount <remote-name>: /path/to/local/mount

We need to add a couple of flags to make this warning free and usable. Most 
notably `--daemon` and `--vfs-cache-mode writes`.

If you have followed the Restech recommendations, your command will look like:

.. code-block:: bash

    [z1234567@katana1 ~]$ rclone mount OneDrive: /home/z1234567/OneDrive --daemon --vfs-cache-mode writes


***********
Final Notes
***********

ResTech really only recommends this if you have sensitive data.

If your data is relatively large - anything above 1GB - we recommend you follow
our standard procedure for large datasets:

1. copy data from source (OneDrive) closer to the cpus ($PBS_TMPDIR)
2. <do analysis>
3. write results to local drive ($PBS_TMPDIR)
4. copy results from local drive to OneDrive
5. delete local data and results

`$PBS_TMPDIR` is only visible to the PBS job that is running. This is as secure
as possible. 

.. _OneDrive: https://onedrive.live.com/
.. _OneDrive needs your consent: https://consenthelper.it.unsw.edu.au/consent?appId=c8800f43-7805-46c2-b8b2-1c55f3859a4c
.. _There are caveats to using RClone with OneDrive: https://rclone.org/onedrive/
