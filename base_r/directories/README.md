
Working with directories
========================

Isolate the directory from a full path
--------------------------------------

``` r
dirname('analysis/model/sdtab001')
```

    ## [1] "analysis/model"

Isolate the file name from a full path
--------------------------------------

``` r
basename('analysis/model/sdtab001')
```

    ## [1] "sdtab001"

Get absolute path
-----------------

``` r
path.expand("~")
```

    ## [1] "/Users/bengu839"
