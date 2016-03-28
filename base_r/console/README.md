
Working in the console
======================

Note that the working directory will be the same one as the command window R was called from.

Run the R script and print output on the console
------------------------------------------------

``` r
Rscript script.R
```

Run the R script and print output in a file `script.Rout`
---------------------------------------------------------

``` r
R CMD BATCH script.R
```

Open interactive session
------------------------

``` r
R                          # Opens interactive session 
source("../../rscript.R")  # Call a script
```

Execute system command
----------------------

``` r
system('execute run001.mod')
```
