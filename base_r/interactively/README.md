
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
