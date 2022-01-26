Lecture 1 v 2: Basics of R
================

# R Resources

-   [R for Data Science](https://r4ds.had.co.nz/)
-   [tidyverse](https://www.tidyverse.org)
-   [RStudio Cheat
    Sheets](https://www.rstudio.com/resources/cheatsheets/)

# Packages

``` r
# install.packages(tidyverse) # gets pacakges from CRAN repository
library(tidyverse) # loads the package
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.1 ──

    ## ✓ ggplot2 3.3.5     ✓ purrr   0.3.4
    ## ✓ tibble  3.1.4     ✓ dplyr   1.0.7
    ## ✓ tidyr   1.1.3     ✓ stringr 1.4.0
    ## ✓ readr   2.0.1     ✓ forcats 0.5.1

    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

Variable assignment and types

``` r
a <- "tree" # a = "tree"
b <- 7
16 -> c # don't do this
```

We can do stuff with variables.

``` r
print(b + c)
```

    ## [1] 23

## Vectors and vectorization

Rules:

-   Vecgtors are one-dimensional and ordered.
-   Vectors can be any length (including 0 length).
-   Vector elements an be names (but they aren’t required to). We can
    access vector elements by position or by name
-   Vectors must contain the same data type: numeric, character, logical
    (TRUE/FALSE), but *not* a mix of those.

``` r
vec.1 <- c(1, 2, 3, 4, 5) # c stands for "concatenate"
vec.2 <- c(6, 5, 4, 3, 2)
vec.1 + vec.2
```

    ## [1] 7 7 7 7 7

Use vectorization whenever possible! But be careful of vector lengths
and recycling

``` r
vec.2 + 3
```

    ## [1] 9 8 7 6 5

``` r
vec.2 + c(1, 2, 3)
```

    ## Warning in vec.2 + c(1, 2, 3): longer object length is not a multiple of shorter
    ## object length

    ## [1] 7 7 7 4 4

Note that we can name vector elements:

``` r
named.vec <- c(a = 1, b = 2, c = 3)
```
