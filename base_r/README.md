
Miscellaneous functions
=======================

Miscelaneous function worth looking into.

``` r
confint()    # Confidence intervals
Map()        # lapply on 2 lists
get()        # Get value of an object
mget()       # Get value of multiple objects
is.formula()
is.element(x, y)
```

Non standard evaluation (NSE)
=============================

``` r
# Functions for handling NSE
substitute()
quote()
deparse(substitute(x))
eval(expression())

# Use variables without quotes
function(x) { as.character(substitute(x)) }
```

*Note : see also the package [`lazyeval`](https://github.com/hadley/lazyeval).*

Debuging
========

``` r
browser()   # Can be used in a loop to analyze a specific call with if statement
debugonce() # Debug ene time
debug()     # Debug all times
undebug()   # Stop debugging
source()    # To enable break points in Rstudio
```

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

Working with data
=================

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

Working with strings
====================

``` r
string <- 'Hello world 2016 !!'
```

Remove all characters from a string
-----------------------------------

``` r
gsub('\\D', '', string, fixed = FALSE)
```

    ## [1] "2016"

Multiple character substitution
-------------------------------

``` r
chartr('wld', 'bde', string)
```

    ## [1] "Heddo borde 2016 !!"

Regular expressions
-------------------

``` r
string2 <- c('blank', 'wade', 'waste', 'rubbish', 'dedekind', 'bated')

# Get all words finishing by "de" or "te"
grepl(pattern = "^.+(de|te)$", x = string2, ignore.case = FALSE)
```

    ## [1] FALSE  TRUE  TRUE FALSE FALSE FALSE

``` r
# Match either empty strings or letters
grepl("(^$)|([A-Za-z]+)", x = string2, ignore.case = FALSE)
```

    ## [1] TRUE TRUE TRUE TRUE TRUE TRUE

*Note: `grepl()` returns logic, `grep()` returns numeric and `grep(value = TRUE).` returns the match*

Working with functions
======================

Benchmark code
--------------

``` r
system.time(mean(1:10^7))
```

    ##    user  system elapsed 
    ##   0.045   0.012   0.059

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

Working interactively
=====================

Make a progress bar
-------------------

From [R function of the day](http://rfunction.com/archives/194).

``` r
 total <- 2
 pb <- txtProgressBar(min = 0, max = total, style = 3)
 for (i in 1:total) {
   # Insert your function call here
   setTxtProgressBar(pb, i)
 }
 close(pb)
```

    ## 
      |                                                                       
      |                                                                 |   0%
      |                                                                       
      |================================                                 |  50%
      |                                                                       
      |=================================================================| 100%

Interactive file selection
--------------------------

``` r
List.dat   <- list.files(pattern = '.mod')
model.name <- select.list(List.dat, 
                          title = cat('\nSelect the model file to analyze:'))
```

### Interactive menu

``` r
switch(menu(c('Choice1', 'Choice2'), 
            title = cat('\nSelect an option:')),
       print('Result1'),    # Action option 1
       print('Result2'))    # Action option 2

> Select an option:
  1: Choice1
  2: Choice2
  
  Selection:
```
