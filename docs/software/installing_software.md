title: Installing software


## Checking if a package is installed

!!! note
    Before installing software yourself, check if it has already been installed in a system-wide module or by one of your research colleagues.

Some common software packages are already available as part of katana's the operating system. They are listed by: 

``` bash 
    yum list installed
```

!!! warning
    Do not try to run system commands as a user. These include: apt-get install, yum install, su, or sudo. 

As detailed in [Environment Modules](./environment_modules.md), software already installed by HPC staff is listed with:

``` bash 
   module avail
```

## Installing a binary package

While installing from source is preferred for effeciency reasons, sometimes
only precompiled binaries. When downloading binaries, make sure you select the
correct architecure and operating system for katana.

!!! note
    Centos 7, 64-bit, Intel x86_6 / AMD64

``` bash
wget https://website.org/binary/application  
```

Change file permissions to make the application readable and executable
   
``` bash
chmod u+rx ./application
```

## Compiling from source

Compiling from source is preferred for efficiency reasons, but is a more involved process than binary installation. 

!!! note
    It is best to install your software in your home directory, since it is backed up every night. 


### Github cloning

Source code is commonly stored on GitHub for easy version control. Git is available by default on katana. Remember: UNSW has its own [GitHub organisation](../using_katana/github.md).


Copy the web address revealed by the green 'Code' button on the repository. Creating a local copy of the repository is then as simple as:

``` bash
   git clone https://github.com/project/project.git
```

The created folder will contain the source code and some documentation files.

### README and INSTALL files

The README file contains general information for the software, and often a brief installation guide. INSTALL will contain more detailed installation instructions, including configuration for certain archictures.
Please read the README and INSTALL files in full before attempting compilation.

### Compilers 

Try to use the system compilers `gcc` and `ld`. However, many codes require specific compilers and versions. Katana has many compilers available as modules including the [Intel Compilers and Software Libraries](./intel_compilers_and_libraries.md)

!!! note 
    Please install software using an interactive session, qsub -I, not directly on the login node. 

### Configuring installation files

Commonly, a configuration script is available. This allows you to set where the software is installed by using the --prefix flag. Again, it is best to install software within your home directory 

``` bash
   ./configure --prefix=$HOME/apps/{PACKAGE}/{VERSION}  make && make install
```

The software can then be installed according to the rules in the MakeFile. This
is typically invoked with

``` bash
   make && make install
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

## R and Python Packages

Many R and Python packages are installed on katana. We have specific documentation to install your own packages in [R](./r.md#installing-libraries) and [Python](./python.md#pip3---the-python-package-manager-the-package-installer-for-python)`

!!! note
    If you do need assistance with software install after trying yourself, then send an email to [IT Service Centre](mailto:ITServiceCentre@unsw.edu.au) with Katana in the subject line.
