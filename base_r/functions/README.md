
Working with functions
======================

Benchmark code
--------------

``` r
system.time(mean(1:10^7))
```

    ##    user  system elapsed 
    ##   0.044   0.011   0.056

Inform users with a message
---------------------------

``` r
message('Hello world')
```

    ## Hello world

Abort execution and return error message
----------------------------------------

``` r
stop('Something went wrong')
> Error: Something went wrong
```

Do not abort code if error is returned
--------------------------------------

``` r
try(log("you can't log a string"))
```

Suppress messages
-----------------

``` r
# When using a function
suppressMessages(expr)
  
# When loading libraries
suppressPackageStartupMessages( library(ggplot2) )
```
