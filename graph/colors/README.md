
Colors
======

Make transparent colors
-----------------------

### Using `rgb()`

``` r
color  <- col2rgb('dodgerblue3')
alpha  <- 0.5
plot(..., col = rgb(color[1], color[2], color[3], alpha = 255 * alpha, max = 255))
```

### Using HEX

The HEX color should be in the format `#RRGGBBAA`, where R is for red, G is for green, B is for blue and A is for alpha in hexadecimals.

``` r
# Hex color example of dodgerblue3 with alpha of 0.5.
color <- '#1874cd80'
plot(..., col = color)
```

Conversion between HEX and decimal in R

``` r
# hex to decimal
as.integer(paste0('0X', 'ff'))
```

    ## [1] 255

``` r
# decimal to hex
as.hexmode(255)
```

    ## [1] "ff"

*See also r hex convertor [`hex_color()`](https://github.com/guiastrennec/modelviz/blob/master/R/hex_color.R) a hex color converter using `x11_hex()` from the package [diagrammeR](https://github.com/rich-iannone/DiagrammeR).*

Palettes
--------

A couple nice colors for graphs. See also [Colorbrewer](http://colorbrewer2.org) for many more examples.

### Color example

``` r
palette  <- c(rgb(166, 206, 227, maxColorValue = 255),
              rgb(31 , 120, 180, maxColorValue = 255), # Blues
              rgb(178, 223, 138, maxColorValue = 255),
              rgb(51 , 160,  44, maxColorValue = 255), # Greens
              rgb(251, 154, 153, maxColorValue = 255),
              rgb(227,  26,  28, maxColorValue = 255), # Reds
              rgb(253, 191, 111, maxColorValue = 255),
              rgb(255, 127,   0, maxColorValue = 255), # Oranges
              rgb(202, 178, 214, maxColorValue = 255),
              rgb(106,  61, 154, maxColorValue = 255)) # Purples

blues   <- c('dodgerblue2', 'deepskyblue2', 'cornflowerblue',
             'dodgerblue3', 'dodgerblue4', 'darkblue')

reds    <- c('tomato2', 'indianred2', 'coral')

purples <- c('slateblue2', 'violetred2')

greens  <- c('yellowgreen', 'chartreuse3')
```

### Create range of colors

``` r
my_palette <- colorRampPalette(c('white', 'blue'))
my_palette(10)
```

    ##  [1] "#FFFFFF" "#E2E2FF" "#C6C6FF" "#AAAAFF" "#8D8DFF" "#7171FF" "#5555FF"
    ##  [8] "#3838FF" "#1C1CFF" "#0000FF"

### Mimic [`ggplot2`](http://ggplot2.org) colors

Function from John Colby on [stack overflow](http://stackoverflow.com/questions/8197559/emulate-ggplot2-default-color-palette)

``` r
gg_color_hue <- function(n) {
  hues = seq(15, 375, length = n + 1)
  hcl(h = hues, l = 65, c = 100)[1:n]
}

# The following will return the red and green of ggplot2
gg_color_hue(2)
```

    ## [1] "#F8766D" "#00BFC4"
