
[Shiny](http://shiny.rstudio.com)
=================================

Open an hyperlink in a new tab
------------------------------

``` r
a('Click here',
  href = 'http://google.com/', 
  target = '_blank')
```

Functions to look into
----------------------

``` r
observe()
isolate()
DataTables()
updateTextInput()
reactiveValues()
validate()

# Get session info
session
```

Make 2 sliders side by side
---------------------------

``` r
wellPanel(
  h3("Parameters"),
  fluidRow(
    column(6, sliderInput("CL", "Clearance", min = 0, max = 50, value = 5)),
    column(6, sliderInput("V", "Volume", min = 0, max = 100, value = 10))
  )
)
```
