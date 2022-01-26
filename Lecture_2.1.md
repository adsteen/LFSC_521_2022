Lecture\_2
================
Drew Steen
1/26/2022

# Most important data classes

## (Atomic) vectors

Talked about these in Lecture 1.5. We can create a vector with:

-   `vector()`
-   `c()`
-   `seq()`
-   `:`

``` r
a <- c(1, 2, 3)
print(a)
```

    ## [1] 1 2 3

``` r
b <- seq(from = 1, to = 3, by = 1)
print(b)
```

    ## [1] 1 2 3

``` r
c <- 1:3 # "infix" operator, because it goes between numbers
```

### Vector coercion

``` r
a <- c(1, "two", 3)
print(a)
```

    ## [1] "1"   "two" "3"

#### Data types

-   logical: TRUE/FALSE
-   integer: 1, 2, 3
-   double: 1.00000…, 3.14159 (32 bits of precision, scientific
    notation)
-   character
-   factor

R *coerces* data types to the *least general* type that works

``` r
my.logical <- c(TRUE, FALSE, TRUE)
class(my.logical)
```

    ## [1] "logical"

``` r
my.result <- my.logical + 3
class(my.result)
```

    ## [1] "numeric"

#### Factors

Factors *look* like character vectors but are actually named itegers.

``` r
my.fac <- factor(1:3)
```

### Accessing vector elements

We can access by *index*. Note R indices start with 1.

``` r
my.vec <- c("a", "b", "c") # note the built-in vector letters is useful
my.vec[c(1,3)] # note that this expression is only 2 elements long
```

    ## [1] "a" "c"

We can also access with a logical vector. This is particularly useful in
selecting based on a test.

``` r
my.vec[c(TRUE, FALSE, TRUE)] # this logical vector has the same length as the vector
```

    ## [1] "a" "c"

``` r
str_detect(my.vec, "[ac]") # this tests for a OR c - str_detect is short for "string detect"
```

    ## [1]  TRUE FALSE  TRUE

``` r
my.vec[str_detect(my.vec, "[ac]")]
```

    ## [1] "a" "c"

``` r
num.vec <- 1:15
num.vec[num.vec >= 8]
```

    ## [1]  8  9 10 11 12 13 14 15

Remember that we can name the elements of a vector, and use those names
to return values

``` r
named.vec <- c(first = 1, second = 2, third = 3)
named.vec["second"]
```

    ## second 
    ##      2

# Lists

Lists store different kinds of information that need to be held together
in one place. I think of them like Santa’s bag - you can put anything in
a list.

``` r
my_list <- list(dataset = mtcars, my.favorite.letter = "f", some.numbers = 1:7)
summary(my_list)
```

    ##                    Length Class      Mode     
    ## dataset            11     data.frame list     
    ## my.favorite.letter  1     -none-     character
    ## some.numbers        7     -none-     numeric

Access list elements by element number:

``` r
my_list[2]
```

    ## $my.favorite.letter
    ## [1] "f"

``` r
# my_list[3] + 3
```

``` r
my_list[[3]] + 3
```

    ## [1]  4  5  6  7  8  9 10

`[[` gives us the actual list element, `[` gives us a list containing
only the element that we asked for.

Access with `$`

``` r
my_list$some.numbers + 3
```

    ## [1]  4  5  6  7  8  9 10

Access by name

``` r
my_list[["some.numbers"]] + 3
```

    ## [1]  4  5  6  7  8  9 10

An example where accessing by name is really useful:

``` r
model <- lm(mpg ~ disp, data = mtcars) # linear model
model$coefficients
```

    ## (Intercept)        disp 
    ## 29.59985476 -0.04121512

## Data frames

Data frames are a special kind of list.

Insert description

``` r
#print(mtcars, n=nrow(mtcars))
```

We access elements of data.frames just the same way we access elements
of any other list

``` r
efficiency <- mtcars[[4]] / mtcars[[3]]
# Take the 4th column of mtcars and divide it by the 3rd column of mtcars
# then assign that to `efficiency`
mean(efficiency)
```

    ## [1] 0.6983209

A much better way to access data frame columns is by name. Basically
tyou should only ever do this.

``` r
efficiency <- mtcars$hp / mtcars$disp
```

# Other object classes

THere are a million but they’re mostly versions of lists.
