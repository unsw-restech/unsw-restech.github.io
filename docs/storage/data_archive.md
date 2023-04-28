title: How to use the UNSW Data Archive

The UNSW Data Archive is the primary research storage facility provided by UNSW. The Data Archive gives UNSW researchers a free, safe and secure storage service to store and access research data well beyond the life of the project that collected that data.

To help researchers make use of this system the Katana Data Mover has a script that you can use to copy files from Katana into a project on the Data Archive system.

!!! note
    To use this script you must have access to the UNSW Data Archive which requires setting up a [Research Data Management Plan](https://research.unsw.edu.au/research-data-management-unsw).

The best documentation on how to use the `Data Archive` is found on their website:

- using the `web application`
- using `SFTP`
- using the `Command Line`


To see what versions of the Data Archive script are available log on to `kdm.restech.unsw.edu.au` and type

``` bash
module avail unswdataarchive
```

Use the help command for usage

``` bash
module help unswdataarchive/2021-02-17
```

## Initial Setup

To use the Data Archive you need to set up a configuration file. Here's how to create the generic config in the directory you are in:

``` bash
[z1234567@kdm ~]$ module add unswdataarchive/2021-02-17
[z1234567@kdm ~]$ get-config-file
```

To generate a token send an email to the [IT Service Centre](mailto:ITServiceCentre@unsw.edu.au) asking for a Data Archive token to be generated. A service desk request for an authentication token to be generated needs to indicate a Data Archive namespace (/UNSW_RDS/Dxxx or /UNSW_RDS/Hxxx) as a scope for the token. Your Data Archive namespace is recorded in the Data Archive welcome email.

!!! warning
    This advice has poor results. The help file is too long for most screen sizes and there's no pagination in modules version < 4. Last line should include a location that the researcher can read directly (using less)

Then edit the configuration file `config.cfg` and change the line that looks like `token=`

If you haven't generated a token you can also upload content using your zID and zPass by adding the following line to the file `config.cfg` and you will be asked for your zPass when you start the upload.

``` bash
user=z1234567
```

## Starting a data transfer

To get data **into** the archive, we use `upload.sh`

``` bash
upload.sh /path/to/your/local/directory /UNSW_RDS/D0000000/your/collection/name
```

To get data **from** the archive, we use `download.sh`

``` bash
download.sh /UNSW_RDS/D0000000/your/collection/name /path/to/your/local/directory
```

[Data Archive](http://www.dataarchive.unsw.edu.au/)

[web application](http://www.dataarchive.unsw.edu.au/help/web-application-guide)

[SFTP](http://www.dataarchive.unsw.edu.au/help/sftp-client-guide)

[Command Line](http://www.dataarchive.unsw.edu.au/help/command-line-script-guide)
