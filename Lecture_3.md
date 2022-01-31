Using the tidyverse
================

``` r
library(tidyverse)
```

# Data input

We often work with built-in data sets in R. Common types of data files:

-   Excel (actually a zip file containing xml (!?!))
-   SQL and sql-like
-   .rds
    -   R specific: stores one object in native R format
    -   `saveRDS()` and `readRDS()`
    -   Usually best bet
-   .rData
    -   R specific: stores multiple R objects in native R format
-   json (javascript object notation)
-   tabular text files
    -   fixed-width
    -   tab-separated (tsv)
    -   comma-separated (csv)
        -   watch out for European number formats: commas and periods
            are switched!

For small-ish, tabular data: try to get tsv or csv files! You can read
them with `readr::read_tsv()` or `readr::read_csv()`. These yield *data
frames*.

## Examining data frames (and other objecets)

-   `glimpse()`
-   `head()`
-   `str()`

``` r
# d <- read_csv("diamonds.csv")
```

# Split-apply-combine

-   *dplyr* package allows calculations with data frames
-   split-apply-combine underlies the *dplyr* philosophy
-   paper (using an outdated package)
    [here](https://www.jstatsoft.org/article/view/v040i01)

## Tidy data

Tidy data are described in [R4DS](https://r4ds.had.co.nz/tidy-data.html)
and in an [original
paper](https://www.jstatsoft.org/article/view/v059i10) (which uses the
outdated *reshape* package rather than the newer *tidyr* package).
Briefly, tidy data should follow the following rules:

1.  Each variable must have its own column.
2.  Each observation must have its own row.
3.  Each value must have its own cell.

What constitutes a variable or an observation is somewhat subjective.
Imagine, for instance, that we have sent out a survey to a bunch of
respondants. Depending on the questions we ask, and the format of the
response, we might consider each response to each question to be an
“observation”, or we might consider each survey to be an “observation”,
and each question within the survey to be a “variable”.

There are criticisms! The tidy format can be inefficient for large data
sets, for instance.

### Wide vs long data

We very often get our data in a relatively “wide” format (especially if
we get it from an excel user). We will want to “pivot” it to a “longer”
format prior to analysis.

I’ve put examples from the lecture in [this Excel
spreadsheet](wide_vs_long_examples.xlsx).

To switch between formats, we can use `tidyr::pivot_longer()` and
`tidyr::pivot_wider()`. Note that the `::` syntax refers to the package
that these come from: that is, "use `pivot_longer()` from the `tidyr`
package.

## Summarise vs mutate

-   `summarise()` yields *one answer per group*
-   `mutate()` yields *one answer per row of data*

``` r
diamonds_summary <- diamonds %>% # pipe operator! 
  group_by(cut) %>%
  summarise(mean.price = mean(price, na.rm = TRUE),
            sd.price = sd(price, na.rm = TRUE)) # don't need to specify the data frame because dplyr has created a new environment for the diamodns data frame
print(diamonds_summary)
```

    ## # A tibble: 5 × 3
    ##   cut       mean.price sd.price
    ##   <ord>          <dbl>    <dbl>
    ## 1 Fair           4359.    3560.
    ## 2 Good           3929.    3682.
    ## 3 Very Good      3982.    3936.
    ## 4 Premium        4584.    4349.
    ## 5 Ideal          3458.    3808.

Here, `mean()` yields a *single* answer for each group, so we use a
summarise operation. Note that, because there is no unique value the
columns other than the grouping variable, only the grouping variable is
retained. That is to say: we grouped on `cut`, so there is not one
unique value for `color` (or `carat`, or `x`, or…) for each value of
`cut`. Thus, we only preserve the `cut` column, plus the two new columns
that `summarise()` created.

Now imagine that we want to see how cut and color combine to yield
price. We just add `color` to the `group_by()` function - easy as that!

``` r
diamonds_summ_2 <- diamonds %>%
  group_by(cut, color) %>%
  summarise(mean.price = mean(price, na.rm = TRUE),
            sd.price = sd(price, na.rm = TRUE), .groups = "drop")
head(diamonds_summ_2)
```

    ## # A tibble: 6 × 4
    ##   cut   color mean.price sd.price
    ##   <ord> <ord>      <dbl>    <dbl>
    ## 1 Fair  D          4291.    3286.
    ## 2 Fair  E          3682.    2977.
    ## 3 Fair  F          3827.    3223.
    ## 4 Fair  G          4239.    3610.
    ## 5 Fair  H          5136.    3886.
    ## 6 Fair  I          4685.    3730.

There are 7 levels of `color` and 5 levels of `cut` so we end up with a
data frame with 7 × 5 = 35 rows.

We added some odd syntax to `sumarise()` here - `.groups = drop`. That’s
because *dplyr* has to make a decision as to how the resulting data
frame should be grouped. By default, it will group the data frame by all
but the last column to be specified. It also gives us a message that is
kind of intrusive and annoying. Personally I don’t want my data frames
to be grouped unless I explicitly grouped them, so I use this option to
return an ungrouped data frame.

Now imagine that we want to perform a grouped operation that will yield
one answer for each row of the data frame. For instance, say we want to
normalize the price of each diamond to the maximum price of diamonds in
that group[1].

Since we have one answer per row of the data frame, we’ll use
`mutate()`.

``` r
diamonds_norm <- diamonds %>%
  group_by(cut) %>%
  mutate(norm.price = price / max(price, na.rm = TRUE))
glimpse(diamonds_norm)
```

    ## Rows: 53,940
    ## Columns: 11
    ## Groups: cut [5]
    ## $ carat      <dbl> 0.23, 0.21, 0.23, 0.29, 0.31, 0.24, 0.24, 0.26, 0.22, 0.23,…
    ## $ cut        <ord> Ideal, Premium, Good, Premium, Good, Very Good, Very Good, …
    ## $ color      <ord> E, E, E, I, J, J, I, H, E, H, J, J, F, J, E, E, I, J, J, J,…
    ## $ clarity    <ord> SI2, SI1, VS1, VS2, SI2, VVS2, VVS1, SI1, VS2, VS1, SI1, VS…
    ## $ depth      <dbl> 61.5, 59.8, 56.9, 62.4, 63.3, 62.8, 62.3, 61.9, 65.1, 59.4,…
    ## $ table      <dbl> 55, 61, 65, 58, 58, 57, 57, 55, 61, 61, 55, 56, 61, 54, 62,…
    ## $ price      <int> 326, 326, 327, 334, 335, 336, 336, 337, 337, 338, 339, 340,…
    ## $ x          <dbl> 3.95, 3.89, 4.05, 4.20, 4.34, 3.94, 3.95, 4.07, 3.87, 4.00,…
    ## $ y          <dbl> 3.98, 3.84, 4.07, 4.23, 4.35, 3.96, 3.98, 4.11, 3.78, 4.05,…
    ## $ z          <dbl> 2.43, 2.31, 2.31, 2.63, 2.75, 2.48, 2.47, 2.53, 2.49, 2.39,…
    ## $ norm.price <dbl> 0.01733489, 0.01731924, 0.01740473, 0.01774425, 0.01783053,…

# List-columns

There’s one situation in which things get a little complicated. What if
I want to apply a transformation that doesn’t yield either a single
number/character/logical result (`summarise()`) or one result per line
of data (`mutate()`)?

Lists are very flexible. We can use the *purrr* package to create
**list-columns**, i.e. columns of data frames where each element is a
list.

The first step is to use `nest()` to “nest” each group of our data frame
into a data frame that sits inside a list-column:

``` r
nested_df <- diamonds %>%
  group_by(cut) %>%
  nest()

glimpse(nested_df$data[[1]])
```

    ## Rows: 21,551
    ## Columns: 9
    ## $ carat   <dbl> 0.23, 0.23, 0.31, 0.30, 0.33, 0.33, 0.33, 0.23, 0.32, 0.30, 0.…
    ## $ color   <ord> E, J, J, I, I, I, J, G, I, I, I, D, D, G, I, E, I, E, G, G, I,…
    ## $ clarity <ord> SI2, VS1, SI2, SI2, SI2, SI2, SI1, VS1, SI1, SI2, VS1, SI1, SI…
    ## $ depth   <dbl> 61.5, 62.8, 62.2, 62.0, 61.8, 61.2, 61.1, 61.9, 60.9, 61.0, 60…
    ## $ table   <dbl> 55.0, 56.0, 54.0, 54.0, 55.0, 56.0, 56.0, 54.0, 55.0, 59.0, 57…
    ## $ price   <int> 326, 340, 344, 348, 403, 403, 403, 404, 404, 405, 552, 552, 55…
    ## $ x       <dbl> 3.95, 3.93, 4.35, 4.31, 4.49, 4.49, 4.49, 3.93, 4.45, 4.30, 4.…
    ## $ y       <dbl> 3.98, 3.90, 4.37, 4.34, 4.51, 4.50, 4.55, 3.95, 4.48, 4.33, 4.…
    ## $ z       <dbl> 2.43, 2.46, 2.71, 2.68, 2.78, 2.75, 2.76, 2.44, 2.72, 2.63, 2.…

Next we apply a function (here, a function I wrote, `lm_fun`) to each
element of the list-column, and put the result in a new list-column
using `mutate()`.

``` r
lm_fun <- function(df) {
  lm(price ~ carat, data = df)
}
nested_df_2 <- nested_df %>%
  mutate(lms = map(data, lm_fun))
summary(nested_df_2$lms[[1]])
```

    ## 
    ## Call:
    ## lm(formula = price ~ carat, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -13786.0   -685.2      4.3    492.7  11452.2 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) -2300.37      18.04  -127.5   <2e-16 ***
    ## carat        8192.39      21.85   374.9   <2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1388 on 21549 degrees of freedom
    ## Multiple R-squared:  0.8671, Adjusted R-squared:  0.8671 
    ## F-statistic: 1.406e+05 on 1 and 21549 DF,  p-value: < 2.2e-16

Finally, we can get information out of the list column by writing a
function to extract data - in this case, the slope of the linear model,
which is the second element in the vector that `coef()` returns.

This time, I will use `map_dbl()` because the slope is a
double-precision number. `map()` always returns a list, but `map_dbl()`
retunrns a double-precision vector (and `map_chr()` returns a character,
etc).

``` r
get_slope <- function(my.lm) {
  coef(my.lm)[2]
}

slopes <- nested_df_2 %>%
  mutate(slope = map_dbl(lms, get_slope)) # map_dbl because I want double precision numbers, not a list
print(slopes)
```

    ## # A tibble: 5 × 4
    ## # Groups:   cut [5]
    ##   cut       data                  lms    slope
    ##   <ord>     <list>                <list> <dbl>
    ## 1 Ideal     <tibble [21,551 × 9]> <lm>   8192.
    ## 2 Premium   <tibble [13,791 × 9]> <lm>   7808.
    ## 3 Good      <tibble [4,906 × 9]>  <lm>   7480.
    ## 4 Very Good <tibble [12,082 × 9]> <lm>   7936.
    ## 5 Fair      <tibble [1,610 × 9]>  <lm>   5924.

[1] I’m not totally sure why we would want to do that, but it makes more
sense if we think of, for instance, a growth rate experiment, where we
want to normalize cell numbers to the initial cell number, which might
vary among treatments
