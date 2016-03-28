
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
