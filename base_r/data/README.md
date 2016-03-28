
Working with datasets
=====================

Stack columns with `melt`
-------------------------

``` r
stacked <- reshape2::melt(data, 
                          id.vars, 
                          measure.vars, 
                          variable.name = "variable", 
                          na.rm = FALSE, 
                          value.name = "value", 
                          factorsAsStrings = TRUE)
```

*Note: equivalent to `gather()` in [`tidyr`](https://github.com/hadley/tidyr).*

Add subtitles or units to column headers
----------------------------------------

``` r
Hmisc::label(dat, column) <- 'Some subtitle'
Hmisc::units(dat, column) <- 'mL/h'
```

Sort by list of columns
-----------------------

``` r
data[do.call('order', as.list(Data[, c('ID', 'TIME', 'MDV')])), ]
```

*Note: equivalent to `arrange()` in [`dplyr`](https://github.com/hadley/dplyr).*

Vectors
-------

``` r
# Make sure to return a vector and not contain any lists
c(A, B, recursive = TRUE)

# Keep the dimensions of the subsetted object 
data[data[, 1] > 4, drop = TRUE]
```

### Create unique factor based on the combination of 2 columns.

``` r
interaction(c('yellow', 'Blue'), 1:2)
```

    ## [1] yellow.1 Blue.2  
    ## Levels: Blue.1 yellow.1 Blue.2 yellow.2

### Remove duplicated elements

``` r
# Based on one column
data[!duplicated(data[, 'ID']), ]

# Based on multiple columns
data[!duplicated(data[, c('ID', 'TIME')]), ]
```

*Note: equivalent to `distinct()` in [`dplyr`](https://github.com/hadley/dplyr).*

### Compare elements

``` r
# Given
A <- 1:10
B <- 6:13

# Are A and B identical
identical(A, B)
```

    ## [1] FALSE

``` r
# Combine elements of A and B
union(A, B)
```

    ##  [1]  1  2  3  4  5  6  7  8  9 10 11 12 13

``` r
# Elements of A not in B
setdiff(A, B)
```

    ## [1] 1 2 3 4 5

``` r
# Comon elements in A and B
intersect(A, B)
```

    ## [1]  6  7  8  9 10

``` r
# ???
setequal(A,B)
```

    ## [1] FALSE

Modulus
-------

``` r
# Modulus of 230 by 12
230 %% 12
```

    ## [1] 2

``` r
# Generate on/off infusion dosing for multiple dosing
Mod   <- Time %% Cycle            # Modulus of time by treatment cycle
Dose  <- ifelse(Mod < 14, 0, 50)  # Generate on off dosing switch
 
# Select rows with odd number
data[rownames(data) %% 2 == 1, ]
```

Create a dataset skeleton
-------------------------

Creates all combination of `ID` and `TIME` and adds `MDV` at the end.

``` r
MDV = 0
expand.grid(ID = 1:4, TIME = 0:10, include = MDV)
```

Match
-----

``` r
# Common use for match()
A %in% B

# Replace COL in A based on the value of B using matching ID
A$var <- B$var[match(A$ID, B$ID)]
```

*Note: see package [fmatch](https://cran.r-project.org/web/packages/fastmatch/fastmatch.pdf) for faster equivalent.*

`for` loops
-----------

``` r
# Crashes if x not defined
for (i in 1:length(x)) { print(x[i]) }
 
# Can handle missing x
for (i in seq_along(x)) { print(x[i]) }
```

Replicate rows
--------------

``` r
# Replicate each rows 2 times
data[rep(row.names(data), times = 2), ]

# Replicate each row based on the values in a column
data[rep(row.names(data), times = data$n_rep), ]
```

Remove `NULL` elements from a list
----------------------------------

``` r
x <-  x[-(which(sapply(x, is.null), arr.ind = TRUE))]
```

Aggregate
---------

``` r
# Using formula
aggregate(DV ~ TIME, data = data, FUN = mean)

# Using by
aggregate(x = data[, 'DV'], by = list(data[, 'TIME']), FUN = mean)
```

*Note: see also `summarise()` in [`dplyr`](https://github.com/hadley/dplyr).*

Read in big data
----------------

See packages [`readr`](https://github.com/hadley/readr) (`read_table()`, `read_csv()`) and [`data.table`](https://github.com/Rdatatable/data.table) (`fread()`)

Get the mean between points
---------------------------

Get mid point with previous row in a vectorized manner.

``` r
x[-length(x)] + (diff(x) / 2)
```
