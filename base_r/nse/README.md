
Non-standard evaluation (NSE)
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
