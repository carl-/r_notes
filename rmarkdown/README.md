
[`rmarkdown`](http://rmarkdown.rstudio.com/)
============================================

Add a comment
-------------

Based on a post from [Ken Kleinman](http://rmarkdown.rstudio.com/authoring_basics.html).

    [//]: # "Somme comments"
    [//]: # (Some comments)

Common mistakes
---------------

-   Two chunks with the same name will cause a big error message
-   Bullet list need to have an empty line before to be recognized it can also be forced with html code `<li>item1</li>` or `\list{\item \item}`

CSS styling in HTML
-------------------

      # Centering knitr::kable() on the page
      <style type="text/css">
        .table {
          margin-left:30%; 
          width:40%;
        }
      </style>

YAML template settings for `\LaTeX`
-----------------------------------

    ---
    title: 'My Report'
    author: 'Benjamin Guiastrennec'
    date: "28 March, 2016"
    mainfont: Arial
    sansfont: Arial
    monofont: Courier
    output:
      pdf_document:
      latex_engine: xelatex
    number_sections: yes
    toc: yes
    ---

Change [`knitr`](http://yihui.name/knitr/) options
--------------------------------------------------

``` r
# Change default to eval = FALSE
knitr::opts_chunk$set(eval = FALSE)
    
# Enable soft wrap in latex
knitr::opts_chunk$set(tidy = TRUE, tidy.opts = list(width.cutoff = 60))
```

*Note : see also `opt_knit$set()` for changing global `knitr` options.*

Extract Rmarkdown R code
------------------------

``` r
knitr::purl('file.Rmd', documentation = 2)
```
