title: OneDrive

You can mount Microsoft's OneDrive on Katana.

## OneDrive Background

We don't recommend researchers mount their OneDrive because the configuration process is not ideal. However, it is possible with the following method.

`There are limitations to using RClone with OneDrive` that users will need to be 
aware of and which the UNSW team cannot help with.


Note: Mounting OneDrive locally will only work on the machine on which the `mount`
command is run.
    
!!! note
    Please mount OneDrive on Katana Data Mover, not the login nodes. 

The configuration section will probably only need to run once, whereas the "how to mount" section will need to be run each time.

## Prerequisites

1. [OneDrive needs your consent](https://consenthelper.it.unsw.edu.au/consent?appId=c8800f43-7805-46c2-b8b2-1c55f3859a4c) to give rclone access to your files. 
2. We can't guarantee or test if this works for researchers in Germany or 
China. OneDrive has a different set up for Germany and China due to local laws 
about data storage.
3. You will need at least one empty directory for mounting. Restech recommends creating the directory ``/home/z1234567/OneDrive`` to mount OneDrive onto.

## Configure RClone for OneDrive
            
1. Login to KDM and run:

``` bash
[z1234567@kdm ~]$ rclone config
```

You will be asked a set of questions. The short answers are:

1. new remote ('n')
2. name ('OneDrive')
3. storage type **Microsoft OneDrive** (**'26'** in rclone version 1.55.1)
4. OAuth Client Id (Press Enter for the default)  
5. OAuth Client Secret (Press Enter for the default)
6. Choose national cloud region for OneDrive ("1. global")
7. Edit advanced config? ("n")
8. Remote config? ("n")
9. *On a machine with rclone and a web browser* **(not kdm)**: Run the ``rclone authorize "onedrive" ...`` command and copy to the clipboard. 
10. *Back to kdm:* Paste result: 
11. Choose a number from below, or type in an existing value ("1. OneDrive Personal or Business")
12. Chose drive to use: ("0")
13. Is that okay? ("Y")

You should then see something like this to which you should answer yes:

``` bash
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
```

## How to mount OneDrive

Once logged in:

1. Mount the drive. The basic syntax is:

``` bash
rclone mount <remote-name>: /path/to/local/mount
```

We need to add a couple of flags to make this warning free and usable. Most 
notably `--daemon` and `--vfs-cache-mode writes`.

If you have followed the ResTech recommendations, your command will look like:

``` bash
[z1234567@kdm ~]$ rclone mount OneDrive: /home/z1234567/OneDrive --daemon --vfs-cache-mode writes
```

!!! info
    Your OneDrive file contents should now be available at /home/z1234567/OneDrive (or chosen mount point). 

[OneDrive](https://onedrive.live.com/)

[OneDrive needs your consent](https://consenthelper.it.unsw.edu.au/consent?appId=c8800f43-7805-46c2-b8b2-1c55f3859a4c)

[There are limitations to using RClone with OneDrive](https://rclone.org/onedrive/)
