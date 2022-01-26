LFSC 521 Lecture 1 - originally planned version
================
Drew Steen
1/24/2022

# Intro to LFSC 521

Welcome!

Sections:

-   Module 1: Tools for computational biology
    -   R, tidyverse & data visualization (Drew Steen)
    -   Databases & NGS sequence analysis (Meg Station)
-   Module 2: Genomics
    -   working with sequences (Scott Emrich)
    -   quantitative genetics (Bode Olukolu)
-   Module 3: Modeling
    -   stochastic modeling (Steven Abel)
-   Module 4: Structural Biology
    -   protein structure (Hong Guo)
    -   AlphaFold and MD (Loukas Petridis)
    -   Molecular docking (Tongye Shen)

# For the next two weeks:

**Reproducible research using R and associated tools**

-   Why reproducible research?
-   Reproducible research with R and RStudio
-   Scripts and notebooks
-   Using the [tidyverse](https://www.tidyverse.org/)
-   Data visualization

# Why reproducible research?

The parable of the [Reinhart-Rogoff Excel
error](https://theconversation.com/the-reinhart-rogoff-error-or-how-not-to-excel-at-economics-13646)

-   Writeups
    [here](https://theconversation.com/the-reinhart-rogoff-error-or-how-not-to-excel-at-economics-13646)
    and [here](https://www.bbc.com/news/magazine-22223190)

![Original excel
sheet](https://cms.qz.com/wp-content/uploads/2013/04/reinhart_rogoff_coding_error_0.png?quality=75&strip=all&w=1240&h=1226&crop=1)

## Working with other people

-   You always work with other people (sometimes more closely than other
    times)
-   You, in 6 months, are another person!

Ideally we would have a way to write down every computational step we
do, in a way that is unambiguous to the computer and also easy for
humans to understand!

This idea was first clearly articulated by Donald Knuth as [literate
programming](https://en.wikipedia.org/wiki/Literate_programming).

## Writing code

-   We can write code directly in R, via the bash shell.
-   This works fine - but it doesn’t (easily) leave a record of what
    we’ve done. What would future you say?

## Writing scripts and notebooks

A **script** is a containing code that you would like to execute. \* A
script is like, well, a script for a play: it is just words on paper,
but if you give it to actors they can turn it into a play \* Similarly,
you can write a script with any text editor you like, but as long as it
contains R code, you can execute it using R (or python code with
[python](https://www.python.org/), or julia code with
[julia](https://julialang.org/))

A **notebook** is a text document that contains text *and* code. We can
*knit* the notebook so that:

-   the code is executed
-   the results of the code are included in the document
-   the whole thing is formatted in some nice way

[Jupyter](https://ipython.org/notebook.html) notebooks are the most
famous example, and if you like you can use them to write R code - the
name stands for *JUlia - PYThon - R*. We’ll be using RMarkdown
notebooks, because the ability to produce these is built into RStudio.
In fact, this document itself is an RMarkdown notebook!

Look, we can use it to do math:

    ## [1] 4

# How to interact with R

As mentioned above, we have lots of ways of interacting with R. We can
work directly with R through the bash shell. We can use our favorite
text editor to write scripts, and then execute them in the bash shell.
But this all seems kind of clunky: isn’t there a better way? If only we
had some kind of environment, to integrate the process of code
development…

In fact, that kind of program is called an Integrated Development
Environment (IDE). There are many, but the one I recommend is
[RStudio](https://www.rstudio.com). RStudio is not R - it is just a
wrapper around R, a way to simplify the task of “talking” to R.
