title: R and RStudio

R is installed as a module with a number of R packages installed. It can be useful to always use the same version of R to avoid the possibility of changes that affect your
calculations between different versions.

## Installing libraries

Because R allows users to use their on library of installed packages you will be able to install most packages for yourself. To see the packages that are already installed as
part of R or you have installed for yourself you can use the library command as shown below.

``` r
    >library()
```

If you would like help installing a new library please email [restech.support@unsw.edu.au](mailto:restech.support@unsw.edu.au).

Most of the time you will be able to install R packages using the following command which will automatically look after all of the software dependencies. 

``` r
    
    > install.packages('devtools')
        ...
    * DONE (mypackage)
```



If you would like to use **RStudio**, we recommend you use [Katana OnDemand](../using_katana/ondemand.md) which allows you to specify the version of R that you
wish to use.


Please try installing R libraries yourself before contacting the service desk. 

If you want to install a library from CRAN_, github or your own R library for 
testing or non-general usage, you can use the regular method and the library 
will be installed locally:

Create directory inside your home directory for the source code, and clone the library (if using Git):

``` bash
mkdir ~/src
cd ~/src
git clone https://github.com/user/mypackage 
```

Start R or RStudio and install 

``` r
    
    > library('devtools')
    > getwd()
    [1] "/home/z1234567/src"
    > install('mypackage')
    ...
    * DONE (mypackage)
```

If one or more of the packages that you have installed for yourself rely on accessing software via the module command then you will need to load the module before starting R. If you forgot or are
using RStudio where this is not possible then you can load the module from within R by making use of the file `/usr/share/Modules/init/r.R` in the following way to both load and unload modules.
``` r

    > load("/usr/share/Modules/init/r.R")
    > module("add","gdal/3.5.3")
	> module("del","gdal/3.5.3")
```


[CRAN](https://cran.r-project.org/web/packages/index.html)
