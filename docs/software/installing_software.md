title: Installing software


## Checking if a package is installed

!!! note
    Before installing software yourself, check if it has already been installed as part of the environment or as a module. You may also want to
	speak to one of your research colleagues to find out if they have installed it for themselves.

Some common software packages are already available as part of katana's the operating system. They are listed by: 

``` bash 
    yum list installed
```

!!! warning
    Do not try to run administration commands as a user. These include: apt-get install, yum install, su, or sudo. 

You can see the list of software applications already installed on Katana using the [Environment Module](../environment_modules) command:

``` bash 
   module avail
```

## R and Python Packages

Many Python and R packages are installed on katana. We have specific documentation to install your own packages in [Python](../python#pip3---the-python-package-manager-the-package-installer-for-python)`
and [R](./r#installing-libraries)

!!! note
    If you have tried to install packages software yourself, but you need further assistance, then please send an email to [restech.support@unsw.edu.au](mailto:restech.support@unsw.edu.au).
    
## Installing a binary package

While installing from source is preferred for effeciency reasons, sometimes
only precompiled binaries are available. When downloading binaries, make sure you select the
correct architecture and operating system for Katana.

!!! note
    The login and compute nodes of Katana are currently Rocky Linux 8.9, 64-bit, Intel x86_64 / AMD64.
	
    If the software is small then you may want to install it in your home directory to take advantage of the
	nightly backup. Otherwise you should install the software in your scratch directory.


``` bash
wget https://website.org/binary/application  
```

Change file permissions to make the application readable and executable
   
``` bash
chmod u+rx ./application
```

## Compiling from source

Compiling from source is preferred for efficiency reasons but is generally a more complicted process than a binary installation. 

!!! note
    If the software is small then you may want to install it in your home directory to take advantage of the nightly backup.
	Otherwise you should install the software in your scratch directory.


### Installing software from Github

Source code is commonly stored on GitHub for easy version control. Git is available by default on katana. Remember: UNSW has its own [GitHub organisation](../../using_katana/github).

The process to install code which comes from GitHub depends on how the author of the code has set it up. 

### Installing a Github software release

Often the software owner will create a software __release__  which is a copy of the software with everything frozen at that point in time. This helps with reproducability of results
as you can refer to a specific version of the software and someone else can easily install the same version on their computer. You will often be able to choose between a
binary version and downloading the source. Downloading the binary and the source versions will look something like:

``` bash
   wget https://github.com/project/project/releases/download/v1.48.1/project.linux_8.zip
```

``` bash
   wget https://github.com/project/project/archive/refs/tags/v1.48.1.tar.gz
```

### Github cloning

If the owner of the software has not created a release or the latest release is too old then you can download the repository and use it to compile the software.
Copy the web address revealed by the green 'Code' button on the repository. Creating a local copy of the repository uses the following command:

``` bash
   git clone https://github.com/project/project.git
```

The created folder will then contain the source code and some documentation files.

### README and INSTALL files

The README file contains general information for the software, and often a brief installation guide. INSTALL will contain more detailed installation instructions, including configuration for 
certain archictures. Please read the README and INSTALL files in full before attempting compilation.

### Compilers 

It is generally best to use the system compilers `gcc` and `ld`. However, many code requires specific compilers and versions. Katana has many compilers available as modules 
including the [Intel Compilers and Software Libraries](../others/#intel-compilers-and-software-libraries)

!!! note 
    Please install software using an interactive session, qsub -I, not directly on the login node. 

### Configuring installation files

Commonly, a configuration script is available which allows you to set where the software is installed by using the --prefix flag as well as other options. To install
the software in your Katana home directory you can use the following command:

``` bash
   ./configure --prefix=$HOME/apps/{PACKAGE}/{VERSION}
```

The software can then be installed according to the rules in the MakeFile. This
is typically invoked with

``` bash
   make
   make install
```

## Creating module files

Much like katana's `Environment Modules`, you can also have multiple versions
of the application available through your own modules.

The template for environment modules is in:

``` bash
/apps/modules/templates/module_file
```

The template module file makes some assumptions and examples, which may not be applicable to your software.
Key sections will likely need to modify are:

 
   * set      basepath          $env(HOME)/apps/{SOFTWARE_NAME}
   * set      version           {VERSION_NUMBER}        

   * set      url
   * set      installed

   * set      compiled_with
   * set      mpiversion

   * prereq     {PREREQUISITE_SOFTWARE)


!!! note
    Insert "module use --append $HOME/apps/Modules" into your ~/.bashrc to enable using your own modules upon login. 

You should be able to module load your own software module as your own.
