-   [Set up](#set-up)
    -   [Read data](#read-data)
-   [Univariate data display](#univariate-data-display)
    -   [Medical history](#medical-history)
    -   [Patient characteristics](#patient-characteristics)
-   [Bivariate data display](#bivariate-data-display)

Set up
======

Assumptions:

-   Basic reporting
-   Code is simple, readable.
-   Less emphasis on modularity (i.e. functions) to aim at level 1 and 2

TODO: add variable to indicate measurement type: \* MH \* cormodoities
\* outcome Add additional metadata on measurement timing

Set up packages and path to the data set.

``` r
library(rmarkdown)
library(ggplot2)
library(dplyr)
```

    ## 
    ## Attaching package: 'dplyr'

    ## The following objects are masked from 'package:stats':
    ## 
    ##     filter, lag

    ## The following objects are masked from 'package:base':
    ## 
    ##     intersect, setdiff, setequal, union

``` r
library(Hmisc)
```

    ## Loading required package: lattice

    ## Loading required package: survival

    ## Loading required package: Formula

    ## 
    ## Attaching package: 'Hmisc'

    ## The following objects are masked from 'package:dplyr':
    ## 
    ##     src, summarize

    ## The following objects are masked from 'package:base':
    ## 
    ##     format.pval, units

``` r
library(here)
```

    ## here() starts at C:/R/ida-regression

``` r
library(patchwork)

## Relative path to the data set
crs_data_path = here("data", "crs.Rdata")
```

Read data
---------

Load the CRS dataset.

``` r
load(crs_data_path)
```

Univariate data display
=======================

Medical history
---------------

Patient characteristics
-----------------------

``` r
gg1 <- crs %>%
  ggplot(aes(y = bmi)) +
  geom_boxplot() +
  coord_flip() +
  theme_bw()
```

``` r
gg1 + gg1 + gg1 
```

![](01_process_crs_files/figure-markdown_github/unnamed-chunk-3-1.png)

Bivariate data display
======================
