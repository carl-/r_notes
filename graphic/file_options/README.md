
File options
============

Save as pdf
-----------

Save graphs in png format in pages of 6 x 12 inches as vector graphic.

``` r
pdf(filename = 'graph.pdf', width = 12, height = 6)
```

Save as png
-----------

Save graphs in png format in pages of 6 x 12 inches and a resolution of 200 ppi. The `%03d` in filename allows to create numbered files for each page.

``` r
png(filename = 'graph_%03d.png', units = 'in', 
    width = 12, height = 6, res = 200)
```
