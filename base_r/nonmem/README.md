
Working with NONMEM
===================

Import NONMEM tables
--------------------

This function is getting old and is slow, prefer the new one [`read_nmtab()`](https://github.com/guiastrennec/modelviz/blob/master/R/read_nmtab.R).

``` r
# Loads the function
  NM.import  <- function(dir    = NULL, 
                         prefix = 'run', 
                         runno, 
                         ext    = '.mod'){
  tmp <- readLines(paste0(dir, prefix, runno, ext))
  tmp <- sapply(strsplit(tmp[grep(' FILE=', tmp)],
                         ' FILE=', fixed = TRUE), '[', 2)
  tmp <- tmp[file.exists(paste0(dir, tmp))]
  tmp <- do.call('cbind', lapply(paste0(dir, tmp), 
                                 read.table,
                                 skip   = 1, 
                                 header = TRUE, 
                                 as.is  = TRUE))
  tmp <- tmp[,!duplicated(colnames(tmp))]
  return(tmp)
}

# Call the function
data <- NM.import(runno = '001')
```

Get chi square cutoff value for NONMEM
--------------------------------------

``` r
risk  <- 0.05
n_prm <- 1
qchisq(1 - risk, df = n_prm)
```

    ## [1] 3.841459
