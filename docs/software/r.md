title: R and RStudio

R is installed as a module with a number of R packages installed. It can be useful to always use the same version of R to avoid the possibility of changes that affect your
calculations between different versions.

## Installing libraries

Because R allows users to have their own library of installed packages you will be able to install most packages for yourself. You can see the packages that you have installed along with those
that come with the version of R that you have loaded by using the following command.

``` r
    >library()
```

Most of the time you will be able to install R packages from [CRAN](https://cran.r-project.org/web/packages/index.html) using the following command which will automatically
look after all of the package dependencies for you.

``` r
    > install.packages('devtools')
        ...
    * DONE (mypackage)
```

If one or more of the packages that you have installed for yourself rely on accessing software via the module command then you will need to load the module before starting R. If you forgot or are
using RStudio where it is not possible to load the module ahead of time you can load the module from within R by making use of the file `/usr/share/Modules/init/r.R` in the following way to
both load and unload modules.

``` r
    > load("/usr/share/Modules/init/r.R")
    > module("add","gdal/3.5.3")
	> module("del","gdal/3.5.3")
```

If you have problems installing a package and would like help installing it please email [restech.support@unsw.edu.au](mailto:restech.support@unsw.edu.au).

## Rstudio and JupyterLab in Katana OnDemand

If you would like to use **RStudio** on Katana, we recommend you use [Katana OnDemand](../using_katana/ondemand.md) which allows you to specify the version of R that you
wish to use including any packages that you have previously installed. 

It is also possible to use the R Kernel with the packages that you have installed from within JuypterLab but we recommend using RStudio instead as it has been written specificlly for R.
If you wish to use the R Kernel within JuypterLab then you can use the following commands within R before you start JupyterLab.

``` r
    > install.packages("devtools")
    > devtools::install_github("IRkernel/IRkernel")
    > IRkernel::installspec()
```

# R and RStudio in Conda

If you wish to install R as part of a Conda environment then you can use the following commands to install R in a new repository called R_software after Conda is installed. This may be the
easiest approach to take if you need to use a version of R that is not installed on Katana or a specialised collection of packages like [BioConductor](https://www.bioconductor.org/). If you are having trouble figuring out which approach
to take please email Restech team at [restech.support@unsw.edu.au](mailto:restech.support@unsw.edu.au).

``` bash
    conda create --name R_software
	conda activate R_software
    conda install R
```

If you then want to use RStudio you can install it using the following command: 

```
    conda install RStudio
```

If there is a conflict between the installed Conda packages then you can create a new Conda environment using the following commands:

``` bash
    conda create --name RStudio
	conda activate RStudio
    conda install RStudio
```

Once RStudio has been installed you can start a **FastX Desktop** with your required resour, open a terminal window by clinking on the item at the top of the screen.
You can then activate your Conda environment and then use the following command to start RStudio:

``` bash
    rstudio
```