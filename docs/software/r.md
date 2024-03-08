title: R and RStudio

R is installed as a module. Each version has a number of libraries installed 
within it.

If you would like a new library installed, please email [restech.support@unsw.edu.au](mailto:restech.support@unsw.edu.au).

If you would like to use **RStudio**, we recommend you use the [Katana OnDemand](../using_katana/ondemand.md) service.

## Installing libraries

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

[CRAN](https://cran.r-project.org/web/packages/index.html)
