
[`lattice`](https://cran.r-project.org/web/packages/lattice/lattice.pdf)
========================================================================

`lattice` example
-----------------

``` r
xyplot(DV ~ TIME | DOSE, 
       data = data, 
       type  = 'o',
       groups = ID, 
       strip = strip.custom(strip.names = TRUE),
       xlab = 'Time (hr)', 
       ylab = 'DV', 
       as.table = TRUE)
```

[`xpose4`](https://cran.r-project.org/web/packages/xpose4/xpose4.pdf)
=====================================================================

`xpose4` example
----------------

### Load NONMEM data

``` r
xpdb <- xpose.data(runno = '001')
```

### Change `xpose4` variable definitions

``` r
change.xvardef(xpdb, 'ranpar') <- paste0('ETA', 1:6)
```

### Access `xpose4` data

``` r
data <- xpdb@Data
```
