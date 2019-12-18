Example notebook to read the CRS dataset
================

## GitHub Documents

This is an R Markdown format used for publishing markdown documents to
GitHub. When you click the **Knit** button all R code chunks are run and
a markdown file (.md) suitable for publishing to GitHub is generated.

``` r
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
library(readr)
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
library(skimr)
```

    ## 
    ## Attaching package: 'skimr'

    ## The following object is masked from 'package:stats':
    ## 
    ##     filter

``` r
library(here)
```

    ## here() starts at C:/R/ida-regression

``` r
## Relative path to the data set
crs_data_path = here("data", "crs.csv")
```

Load the CRS dataset.

``` r
crs_data = read_csv(crs_data_path)
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   sex = col_character(),
    ##   primaryproc = col_character()
    ## )

    ## See spec(...) for full column specifications.

``` r
crs_data %>% glimpse()
```

    ## Observations: 345
    ## Variables: 44
    ## $ id              <dbl> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14,...
    ## $ los             <dbl> 6, 4, 3, 4, 6, 4, 3, 8, 5, 5, 10, 4, 13, 4, 3,...
    ## $ agenew          <dbl> 68, 56, 69, 74, 76, 55, 77, 62, 75, 71, 76, 52...
    ## $ sex             <chr> "male", "male", "male", "female", "male", "mal...
    ## $ bmi             <dbl> 29.2, 27.5, 30.4, 31.8, 18.5, 23.5, 20.8, 42.2...
    ## $ asa             <dbl> 3, 2, 3, 2, 3, 2, 2, 4, 3, 3, 3, 2, 3, 2, 2, 3...
    ## $ priorabdsurgery <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0, 1, 0, 0...
    ## $ mis             <dbl> 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1...
    ## $ preopxrt        <dbl> 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0...
    ## $ preopchemo      <dbl> 0, 1, 1, 0, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0...
    ## $ readmit30       <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 0, 0, 0, 0...
    ## $ death30         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ anemiatransf    <dbl> 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0...
    ## $ afib            <dbl> 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ chf             <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ mi              <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ dvtpe           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ arf             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0...
    ## $ respfail        <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ ileus           <dbl> 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0...
    ## $ sboreop         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ reopbleed       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0...
    ## $ winfect         <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0...
    ## $ abscessleak     <dbl> 1, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0...
    ## $ pneumonia       <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ uti             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ day1stbm        <dbl> 2, 2, 2, 3, 6, 1, 3, 1, 5, 1, 4, 4, NA, 3, 1, ...
    ## $ primaryproc     <chr> "LAR", "LAR", "APR", "LAR", "APR", "LAR", "Pro...
    ## $ diabetes        <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 1, 0, 1, 0, 0, 0...
    ## $ hf              <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ pvd             <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ copd            <dbl> 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1...
    ## $ renal           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0...
    ## $ tobaccoever     <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ tobaccocurrent  <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ crohn           <dbl> 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ ulcercol        <dbl> 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ ibd             <dbl> 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0...
    ## $ t2comp          <dbl> 32, NA, NA, NA, 4, NA, NA, NA, 0, 1, 6, NA, 1,...
    ## $ comp            <dbl> 1, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0...
    ## $ t2comp_pre      <dbl> NA, NA, NA, NA, 4, NA, NA, NA, 0, 1, 6, NA, 1,...
    ## $ comp_pre        <dbl> 0, 0, 0, 0, 1, 0, 0, 0, 1, 1, 1, 0, 1, 0, 0, 0...
    ## $ num_comp        <dbl> 1, 0, 0, 0, 1, 0, 0, 0, 2, 2, 2, 0, 1, 0, 0, 0...
    ## $ dc2fu           <dbl> 502, 1942, 1425, 209, 743, 1284, 919, 1297, 36...

``` r
crs_data %>% 
  ggplot(aes(x = t2comp_pre, y = bmi)) + 
  geom_point()
```

    ## Warning: Removed 268 rows containing missing values (geom_point).

![](example_read_crs_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

Examine the the contents of the data set.

``` r
Hmisc::describe(crs_data) 
```

    ## crs_data 
    ## 
    ##  44  Variables      345  Observations
    ## ---------------------------------------------------------------------------
    ## id 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0      345        1      173    115.3     18.2     35.4 
    ##      .25      .50      .75      .90      .95 
    ##     87.0    173.0    259.0    310.6    327.8 
    ## 
    ## lowest :   1   2   3   4   5, highest: 341 342 343 344 345
    ## ---------------------------------------------------------------------------
    ## los 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0       23    0.976    5.678    3.671        2        3 
    ##      .25      .50      .75      .90      .95 
    ##        3        5        6       10       13 
    ## 
    ## lowest :  2  3  4  5  6, highest: 21 23 24 26 32
    ## ---------------------------------------------------------------------------
    ## agenew 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0       62    0.999    59.12    15.52     37.0     41.4 
    ##      .25      .50      .75      .90      .95 
    ##     51.0     60.0     69.0     76.0     80.6 
    ## 
    ## lowest : 16 18 23 26 27, highest: 85 86 87 88 90
    ## ---------------------------------------------------------------------------
    ## sex 
    ##        n  missing distinct 
    ##      345        0        2 
    ##                         
    ## Value      female   male
    ## Frequency     110    235
    ## Proportion  0.319  0.681
    ## ---------------------------------------------------------------------------
    ## bmi 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0      156        1    27.27    5.425    20.60    21.34 
    ##      .25      .50      .75      .90      .95 
    ##    23.90    26.70    29.90    33.46    36.60 
    ## 
    ## lowest : 17.2 17.9 18.3 18.5 19.0, highest: 41.8 42.2 42.3 42.8 47.2
    ## ---------------------------------------------------------------------------
    ## asa 
    ##        n  missing distinct     Info     Mean      Gmd 
    ##      345        0        4    0.668    2.235   0.5005 
    ##                                   
    ## Value          1     2     3     4
    ## Frequency     16   234    93     2
    ## Proportion 0.046 0.678 0.270 0.006
    ## ---------------------------------------------------------------------------
    ## priorabdsurgery 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2     0.57       88   0.2551   0.3811 
    ## 
    ## ---------------------------------------------------------------------------
    ## mis 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2      0.7      128    0.371   0.4681 
    ## 
    ## ---------------------------------------------------------------------------
    ## preopxrt 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.743      156   0.4522   0.4969 
    ## 
    ## ---------------------------------------------------------------------------
    ## preopchemo 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.746      160   0.4638   0.4988 
    ## 
    ## ---------------------------------------------------------------------------
    ## readmit30 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.327       43   0.1246   0.2188 
    ## 
    ## ---------------------------------------------------------------------------
    ## death30 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.017        2 0.005797  0.01156 
    ## 
    ## ---------------------------------------------------------------------------
    ## anemiatransf 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.273       35   0.1014   0.1828 
    ## 
    ## ---------------------------------------------------------------------------
    ## afib 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.034        4  0.01159  0.02299 
    ## 
    ## ---------------------------------------------------------------------------
    ## chf 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.109       13  0.03768  0.07273 
    ## 
    ## ---------------------------------------------------------------------------
    ## mi 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.009        1 0.002899 0.005797 
    ## 
    ## ---------------------------------------------------------------------------
    ## dvtpe 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.068        8  0.02319  0.04543 
    ## 
    ## ---------------------------------------------------------------------------
    ## arf 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.076        9  0.02609  0.05096 
    ## 
    ## ---------------------------------------------------------------------------
    ## respfail 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.017        2 0.005797  0.01156 
    ## 
    ## ---------------------------------------------------------------------------
    ## ileus 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.539       81   0.2348   0.3604 
    ## 
    ## ---------------------------------------------------------------------------
    ## sboreop 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.034        4  0.01159  0.02299 
    ## 
    ## ---------------------------------------------------------------------------
    ## reopbleed 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.017        2 0.005797  0.01156 
    ## 
    ## ---------------------------------------------------------------------------
    ## winfect 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.109       13  0.03768  0.07273 
    ## 
    ## ---------------------------------------------------------------------------
    ## abscessleak 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.187       23  0.06667   0.1248 
    ## 
    ## ---------------------------------------------------------------------------
    ## pneumonia 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.034        4  0.01159  0.02299 
    ## 
    ## ---------------------------------------------------------------------------
    ## uti 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.133       16  0.04638  0.08871 
    ## 
    ## ---------------------------------------------------------------------------
    ## day1stbm 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      305       40       12    0.934    2.672    1.728        1        1 
    ##      .25      .50      .75      .90      .95 
    ##        2        2        3        5        6 
    ##                                                                       
    ## Value          0     1     2     3     4     5     6     7     8     9
    ## Frequency      2    69   109    59    33     9    12     3     5     1
    ## Proportion 0.007 0.226 0.357 0.193 0.108 0.030 0.039 0.010 0.016 0.003
    ##                       
    ## Value         10    12
    ## Frequency      2     1
    ## Proportion 0.007 0.003
    ## ---------------------------------------------------------------------------
    ## primaryproc 
    ##        n  missing distinct 
    ##      345        0        3 
    ##                                                           
    ## Value                  APR             LAR Proctocolectomy
    ## Frequency               68             249              28
    ## Proportion           0.197           0.722           0.081
    ## ---------------------------------------------------------------------------
    ## diabetes 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.321       42   0.1217   0.2145 
    ## 
    ## ---------------------------------------------------------------------------
    ## hf 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.043        5  0.01449  0.02865 
    ## 
    ## ---------------------------------------------------------------------------
    ## pvd 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.034        4  0.01159  0.02299 
    ## 
    ## ---------------------------------------------------------------------------
    ## copd 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.245       31  0.08986    0.164 
    ## 
    ## ---------------------------------------------------------------------------
    ## renal 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.093       11  0.03188  0.06191 
    ## 
    ## ---------------------------------------------------------------------------
    ## tobaccoever 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2     0.52       77   0.2232   0.3478 
    ## 
    ## ---------------------------------------------------------------------------
    ## tobaccocurrent 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.252       32  0.09275   0.1688 
    ## 
    ## ---------------------------------------------------------------------------
    ## crohn 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.017        2 0.005797  0.01156 
    ## 
    ## ---------------------------------------------------------------------------
    ## ulcercol 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.068        8  0.02319  0.04543 
    ## 
    ## ---------------------------------------------------------------------------
    ## ibd 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.084       10  0.02899  0.05645 
    ## 
    ## ---------------------------------------------------------------------------
    ## t2comp 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      101      244       26    0.981     8.01     9.01        1        2 
    ##      .25      .50      .75      .90      .95 
    ##        3        4        7       19       30 
    ## 
    ## lowest :  0  1  2  3  4, highest: 32 36 41 44 96
    ## ---------------------------------------------------------------------------
    ## comp 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2    0.621      101   0.2928   0.4153 
    ## 
    ## ---------------------------------------------------------------------------
    ## t2comp_pre 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##       77      268       10    0.962    3.649    2.079        1        1 
    ##      .25      .50      .75      .90      .95 
    ##        3        3        5        6        7 
    ##                                                                       
    ## Value          0     1     2     3     4     5     6     7     8     9
    ## Frequency      3     6     9    22    17     8     5     4     2     1
    ## Proportion 0.039 0.078 0.117 0.286 0.221 0.104 0.065 0.052 0.026 0.013
    ## ---------------------------------------------------------------------------
    ## comp_pre 
    ##        n  missing distinct     Info      Sum     Mean      Gmd 
    ##      345        0        2     0.52       77   0.2232   0.3478 
    ## 
    ## ---------------------------------------------------------------------------
    ## num_comp 
    ##        n  missing distinct     Info     Mean      Gmd 
    ##      345        0        4    0.634   0.3681   0.5574 
    ##                                   
    ## Value          0     1     2     3
    ## Frequency    244    78    20     3
    ## Proportion 0.707 0.226 0.058 0.009
    ## ---------------------------------------------------------------------------
    ## dc2fu 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0      318        1    792.3    692.1     29.4     68.4 
    ##      .25      .50      .75      .90      .95 
    ##    255.0    663.0   1297.0   1690.4   1895.6 
    ## 
    ## lowest :    0    1    2   14   15, highest: 2181 2191 2229 2257 2281
    ## ---------------------------------------------------------------------------

``` r
sum <- Hmisc::describe(crs_data) 
sum %>% glimpse()
```

    ## List of 44
    ##  $ id             :List of 6
    ##   ..$ descript: chr "id"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "345" "0" "345" "1" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:70] 0 5 10 15 20 25 30 35 40 45 ...
    ##   .. ..$ frequency: num [1:70(1d)] 2 5 5 5 5 5 5 5 5 5 ...
    ##   ..$ extremes: Named num [1:10] 1 2 3 4 5 341 342 343 344 345
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ los            :List of 6
    ##   ..$ descript: chr "los"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "345" "0" "23" "0.976" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:23] 2 3 4 5 6 7 8 9 10 11 ...
    ##   .. ..$ frequency: num [1:23(1d)] 28 82 60 54 36 19 14 12 8 5 ...
    ##   ..$ extremes: Named num [1:10] 2 3 4 5 6 21 23 24 26 32
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ agenew         :List of 6
    ##   ..$ descript: chr "agenew"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "345" "0" "62" "0.999" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:62] 16 18 23 26 27 28 32 33 34 36 ...
    ##   .. ..$ frequency: num [1:62(1d)] 1 1 3 1 1 1 2 4 1 2 ...
    ##   ..$ extremes: Named num [1:10] 16 18 23 26 27 85 86 87 88 90
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ sex            :List of 5
    ##   ..$ descript: chr "sex"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named num [1:3] 345 0 2
    ##   .. ..- attr(*, "names")= chr [1:3] "n" "missing" "distinct"
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : chr [1:2] "female" "male"
    ##   .. ..$ frequency: num [1:2(1d)] 110 235
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ bmi            :List of 6
    ##   ..$ descript: chr "bmi"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "345" "0" "156" "1" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:50] 17 18 18.5 19 19.5 20 20.5 21 21.5 22 ...
    ##   .. ..$ frequency: num [1:50(1d)] 1 2 3 1 5 4 8 8 11 8 ...
    ##   ..$ extremes: Named num [1:10] 17.2 17.9 18.3 18.5 19 41.8 42.2 42.3 42.8 47.2
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ asa            :List of 5
    ##   ..$ descript: chr "asa"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:6] "345" "0" "4" "0.668" ...
    ##   .. ..- attr(*, "names")= chr [1:6] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:4] 1 2 3 4
    ##   .. ..$ frequency: num [1:4(1d)] 16 234 93 2
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ priorabdsurgery:List of 4
    ##   ..$ descript: chr "priorabdsurgery"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.57" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ mis            :List of 4
    ##   ..$ descript: chr "mis"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.7" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ preopxrt       :List of 4
    ##   ..$ descript: chr "preopxrt"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.743" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ preopchemo     :List of 4
    ##   ..$ descript: chr "preopchemo"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.746" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ readmit30      :List of 4
    ##   ..$ descript: chr "readmit30"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.327" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ death30        :List of 4
    ##   ..$ descript: chr "death30"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.017" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ anemiatransf   :List of 4
    ##   ..$ descript: chr "anemiatransf"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.273" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ afib           :List of 4
    ##   ..$ descript: chr "afib"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.034" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ chf            :List of 4
    ##   ..$ descript: chr "chf"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.109" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ mi             :List of 4
    ##   ..$ descript: chr "mi"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.009" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ dvtpe          :List of 4
    ##   ..$ descript: chr "dvtpe"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.068" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ arf            :List of 4
    ##   ..$ descript: chr "arf"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.076" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ respfail       :List of 4
    ##   ..$ descript: chr "respfail"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.017" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ ileus          :List of 4
    ##   ..$ descript: chr "ileus"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.539" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ sboreop        :List of 4
    ##   ..$ descript: chr "sboreop"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.034" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ reopbleed      :List of 4
    ##   ..$ descript: chr "reopbleed"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.017" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ winfect        :List of 4
    ##   ..$ descript: chr "winfect"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.109" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ abscessleak    :List of 4
    ##   ..$ descript: chr "abscessleak"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.187" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ pneumonia      :List of 4
    ##   ..$ descript: chr "pneumonia"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.034" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ uti            :List of 4
    ##   ..$ descript: chr "uti"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.133" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ day1stbm       :List of 6
    ##   ..$ descript: chr "day1stbm"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "305" "40" "12" "0.934" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:12] 0 1 2 3 4 5 6 7 8 9 ...
    ##   .. ..$ frequency: num [1:12(1d)] 2 69 109 59 33 9 12 3 5 1 ...
    ##   ..$ extremes: Named num [1:10] 0 1 2 3 4 7 8 9 10 12
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ primaryproc    :List of 5
    ##   ..$ descript: chr "primaryproc"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named num [1:3] 345 0 3
    ##   .. ..- attr(*, "names")= chr [1:3] "n" "missing" "distinct"
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : chr [1:3] "APR" "LAR" "Proctocolectomy"
    ##   .. ..$ frequency: num [1:3(1d)] 68 249 28
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ diabetes       :List of 4
    ##   ..$ descript: chr "diabetes"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.321" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ hf             :List of 4
    ##   ..$ descript: chr "hf"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.043" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ pvd            :List of 4
    ##   ..$ descript: chr "pvd"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.034" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ copd           :List of 4
    ##   ..$ descript: chr "copd"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.245" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ renal          :List of 4
    ##   ..$ descript: chr "renal"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.093" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ tobaccoever    :List of 4
    ##   ..$ descript: chr "tobaccoever"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.52" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ tobaccocurrent :List of 4
    ##   ..$ descript: chr "tobaccocurrent"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.252" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ crohn          :List of 4
    ##   ..$ descript: chr "crohn"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.017" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ ulcercol       :List of 4
    ##   ..$ descript: chr "ulcercol"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.068" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ ibd            :List of 4
    ##   ..$ descript: chr "ibd"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.084" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ t2comp         :List of 6
    ##   ..$ descript: chr "t2comp"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "101" "244" "26" "0.981" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:26] 0 1 2 3 4 5 6 7 8 9 ...
    ##   .. ..$ frequency: num [1:26(1d)] 3 6 9 22 19 9 6 4 3 2 ...
    ##   ..$ extremes: Named num [1:10] 0 1 2 3 4 32 36 41 44 96
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ comp           :List of 4
    ##   ..$ descript: chr "comp"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.621" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ t2comp_pre     :List of 6
    ##   ..$ descript: chr "t2comp_pre"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "77" "268" "10" "0.962" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:10] 0 1 2 3 4 5 6 7 8 9
    ##   .. ..$ frequency: num [1:10(1d)] 3 6 9 22 17 8 5 4 2 1
    ##   ..$ extremes: Named num [1:10] 0 1 2 3 4 5 6 7 8 9
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ comp_pre       :List of 4
    ##   ..$ descript: chr "comp_pre"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:7] "345" "0" "2" "0.52" ...
    ##   .. ..- attr(*, "names")= chr [1:7] "n" "missing" "distinct" "Info" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ num_comp       :List of 5
    ##   ..$ descript: chr "num_comp"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:6] "345" "0" "4" "0.634" ...
    ##   .. ..- attr(*, "names")= chr [1:6] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:4] 0 1 2 3
    ##   .. ..$ frequency: num [1:4(1d)] 244 78 20 3
    ##   ..- attr(*, "class")= chr "describe"
    ##  $ dc2fu          :List of 6
    ##   ..$ descript: chr "dc2fu"
    ##   ..$ units   : NULL
    ##   ..$ format  : NULL
    ##   ..$ counts  : Named chr [1:13] "345" "0" "318" "1" ...
    ##   .. ..- attr(*, "names")= chr [1:13] "n" "missing" "distinct" "Info" ...
    ##   ..$ values  :List of 2
    ##   .. ..$ value    : num [1:104] 0 20 40 60 80 100 120 140 160 180 ...
    ##   .. ..$ frequency: num [1:104(1d)] 5 13 9 9 5 2 3 6 5 5 ...
    ##   ..$ extremes: Named num [1:10] 0 1 2 14 15 ...
    ##   .. ..- attr(*, "names")= chr [1:10] "L1" "L2" "L3" "L4" ...
    ##   ..- attr(*, "class")= chr "describe"
    ##  - attr(*, "descript")= chr "crs_data"
    ##  - attr(*, "dimensions")= int [1:2] 345 44
    ##  - attr(*, "class")= chr "describe"

``` r
skim(crs_data)
```

    ## Skim summary statistics
    ##  n obs: 345 
    ##  n variables: 44 
    ## 
    ## -- Variable type:character -------------------------------------------------------------
    ##     variable missing complete   n min max empty n_unique
    ##  primaryproc       0      345 345   3  15     0        3
    ##          sex       0      345 345   4   6     0        2
    ## 
    ## -- Variable type:numeric ---------------------------------------------------------------
    ##         variable missing complete   n     mean      sd   p0   p25   p50
    ##      abscessleak       0      345 345   0.067    0.25   0     0     0  
    ##             afib       0      345 345   0.012    0.11   0     0     0  
    ##           agenew       0      345 345  59.12    13.72  16    51    60  
    ##     anemiatransf       0      345 345   0.1      0.3    0     0     0  
    ##              arf       0      345 345   0.026    0.16   0     0     0  
    ##              asa       0      345 345   2.23     0.53   1     2     2  
    ##              bmi       0      345 345  27.27     4.95  17.2  23.9  26.7
    ##              chf       0      345 345   0.038    0.19   0     0     0  
    ##             comp       0      345 345   0.29     0.46   0     0     0  
    ##         comp_pre       0      345 345   0.22     0.42   0     0     0  
    ##             copd       0      345 345   0.09     0.29   0     0     0  
    ##            crohn       0      345 345   0.0058   0.076  0     0     0  
    ##         day1stbm      40      305 345   2.67     1.75   0     2     2  
    ##            dc2fu       0      345 345 792.35   610.94   0   255   663  
    ##          death30       0      345 345   0.0058   0.076  0     0     0  
    ##         diabetes       0      345 345   0.12     0.33   0     0     0  
    ##            dvtpe       0      345 345   0.023    0.15   0     0     0  
    ##               hf       0      345 345   0.014    0.12   0     0     0  
    ##              ibd       0      345 345   0.029    0.17   0     0     0  
    ##               id       0      345 345 173       99.74   1    87   173  
    ##            ileus       0      345 345   0.23     0.42   0     0     0  
    ##              los       0      345 345   5.68     3.99   2     3     5  
    ##               mi       0      345 345   0.0029   0.054  0     0     0  
    ##              mis       0      345 345   0.37     0.48   0     0     0  
    ##         num_comp       0      345 345   0.37     0.63   0     0     0  
    ##        pneumonia       0      345 345   0.012    0.11   0     0     0  
    ##       preopchemo       0      345 345   0.46     0.5    0     0     0  
    ##         preopxrt       0      345 345   0.45     0.5    0     0     0  
    ##  priorabdsurgery       0      345 345   0.26     0.44   0     0     0  
    ##              pvd       0      345 345   0.012    0.11   0     0     0  
    ##        readmit30       0      345 345   0.12     0.33   0     0     0  
    ##            renal       0      345 345   0.032    0.18   0     0     0  
    ##        reopbleed       0      345 345   0.0058   0.076  0     0     0  
    ##         respfail       0      345 345   0.0058   0.076  0     0     0  
    ##          sboreop       0      345 345   0.012    0.11   0     0     0  
    ##           t2comp     244      101 345   8.01    12.38   0     3     4  
    ##       t2comp_pre     268       77 345   3.65     1.89   0     3     3  
    ##   tobaccocurrent       0      345 345   0.093    0.29   0     0     0  
    ##      tobaccoever       0      345 345   0.22     0.42   0     0     0  
    ##         ulcercol       0      345 345   0.023    0.15   0     0     0  
    ##              uti       0      345 345   0.046    0.21   0     0     0  
    ##          winfect       0      345 345   0.038    0.19   0     0     0  
    ##     p75   p100     hist
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##    69     90   <U+2581><U+2581><U+2583><U+2587><U+2587><U+2587><U+2585><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3      4   <U+2581><U+2581><U+2587><U+2581><U+2581><U+2583><U+2581><U+2581>
    ##    29.9   47.2 <U+2582><U+2585><U+2587><U+2583><U+2582><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3     12   <U+2583><U+2587><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##  1297   2281   <U+2587><U+2586><U+2583><U+2583><U+2583><U+2583><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##   259    345   <U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     6     32   <U+2587><U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2585>
    ##     1      3   <U+2587><U+2581><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2586>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     7     96   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     5      9   <U+2583><U+2583><U+2587><U+2586><U+2583><U+2582><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>

``` r
skim(crs_data) 
```

    ## Skim summary statistics
    ##  n obs: 345 
    ##  n variables: 44 
    ## 
    ## -- Variable type:character -------------------------------------------------------------
    ##     variable missing complete   n min max empty n_unique
    ##  primaryproc       0      345 345   3  15     0        3
    ##          sex       0      345 345   4   6     0        2
    ## 
    ## -- Variable type:numeric ---------------------------------------------------------------
    ##         variable missing complete   n     mean      sd   p0   p25   p50
    ##      abscessleak       0      345 345   0.067    0.25   0     0     0  
    ##             afib       0      345 345   0.012    0.11   0     0     0  
    ##           agenew       0      345 345  59.12    13.72  16    51    60  
    ##     anemiatransf       0      345 345   0.1      0.3    0     0     0  
    ##              arf       0      345 345   0.026    0.16   0     0     0  
    ##              asa       0      345 345   2.23     0.53   1     2     2  
    ##              bmi       0      345 345  27.27     4.95  17.2  23.9  26.7
    ##              chf       0      345 345   0.038    0.19   0     0     0  
    ##             comp       0      345 345   0.29     0.46   0     0     0  
    ##         comp_pre       0      345 345   0.22     0.42   0     0     0  
    ##             copd       0      345 345   0.09     0.29   0     0     0  
    ##            crohn       0      345 345   0.0058   0.076  0     0     0  
    ##         day1stbm      40      305 345   2.67     1.75   0     2     2  
    ##            dc2fu       0      345 345 792.35   610.94   0   255   663  
    ##          death30       0      345 345   0.0058   0.076  0     0     0  
    ##         diabetes       0      345 345   0.12     0.33   0     0     0  
    ##            dvtpe       0      345 345   0.023    0.15   0     0     0  
    ##               hf       0      345 345   0.014    0.12   0     0     0  
    ##              ibd       0      345 345   0.029    0.17   0     0     0  
    ##               id       0      345 345 173       99.74   1    87   173  
    ##            ileus       0      345 345   0.23     0.42   0     0     0  
    ##              los       0      345 345   5.68     3.99   2     3     5  
    ##               mi       0      345 345   0.0029   0.054  0     0     0  
    ##              mis       0      345 345   0.37     0.48   0     0     0  
    ##         num_comp       0      345 345   0.37     0.63   0     0     0  
    ##        pneumonia       0      345 345   0.012    0.11   0     0     0  
    ##       preopchemo       0      345 345   0.46     0.5    0     0     0  
    ##         preopxrt       0      345 345   0.45     0.5    0     0     0  
    ##  priorabdsurgery       0      345 345   0.26     0.44   0     0     0  
    ##              pvd       0      345 345   0.012    0.11   0     0     0  
    ##        readmit30       0      345 345   0.12     0.33   0     0     0  
    ##            renal       0      345 345   0.032    0.18   0     0     0  
    ##        reopbleed       0      345 345   0.0058   0.076  0     0     0  
    ##         respfail       0      345 345   0.0058   0.076  0     0     0  
    ##          sboreop       0      345 345   0.012    0.11   0     0     0  
    ##           t2comp     244      101 345   8.01    12.38   0     3     4  
    ##       t2comp_pre     268       77 345   3.65     1.89   0     3     3  
    ##   tobaccocurrent       0      345 345   0.093    0.29   0     0     0  
    ##      tobaccoever       0      345 345   0.22     0.42   0     0     0  
    ##         ulcercol       0      345 345   0.023    0.15   0     0     0  
    ##              uti       0      345 345   0.046    0.21   0     0     0  
    ##          winfect       0      345 345   0.038    0.19   0     0     0  
    ##     p75   p100     hist
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##    69     90   <U+2581><U+2581><U+2583><U+2587><U+2587><U+2587><U+2585><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3      4   <U+2581><U+2581><U+2587><U+2581><U+2581><U+2583><U+2581><U+2581>
    ##    29.9   47.2 <U+2582><U+2585><U+2587><U+2583><U+2582><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3     12   <U+2583><U+2587><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##  1297   2281   <U+2587><U+2586><U+2583><U+2583><U+2583><U+2583><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##   259    345   <U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     6     32   <U+2587><U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2585>
    ##     1      3   <U+2587><U+2581><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2586>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     7     96   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     5      9   <U+2583><U+2583><U+2587><U+2586><U+2583><U+2582><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>

``` r
set.seed(22)
data <- data.frame(age = floor(rnorm(6,25,10)), 
                   sex = gl(2,1,6, labels = c("f","m")))
var.labels <- c(age = "Age in Years", 
                sex = "Sex of the participant")
dplyr::as.tbl(data) # as tibble ---------------------------------------------
```

    ## # A tibble: 6 x 2
    ##     age sex  
    ##   <dbl> <fct>
    ## 1    19 f    
    ## 2    49 m    
    ## 3    35 f    
    ## 4    27 m    
    ## 5    22 f    
    ## 6    43 m

``` r
#> # A tibble: 6 × 2
#>     age    sex
#>   <dbl> <fctr>
#> 1    19      f
#> 2    49      m
#> 3    35      f
#> 4    27      m
#> 5    22      f
#> 6    43      m
data <- Hmisc::upData(data, labels = var.labels) # update data --------------
```

    ## Input object size:    1440 bytes;     2 variables     6 observations
    ## New object size: 2272 bytes; 2 variables 6 observations

``` r
#> Input object size:    1328 bytes;     2 variables     6 observations
#> New object size: 2096 bytes; 2 variables 6 observations
Hmisc::label(data) # check new labels ---------------------------------------
```

    ##                      age                      sex 
    ##           "Age in Years" "Sex of the participant"

``` r
#>                      age                      sex 
#>           "Age in Years" "Sex of the participant"
Hmisc::contents(data) # data dictionary -------------------------------------
```

    ## 
    ## Data frame:data  6 observations and 2 variables    Maximum # NAs:0
    ## 
    ## 
    ##                     Labels Levels   Class Storage
    ## age           Age in Years        integer integer
    ## sex Sex of the participant      2         integer
    ## 
    ## +--------+------+
    ## |Variable|Levels|
    ## +--------+------+
    ## |   sex  |  f,m |
    ## +--------+------+

``` r
skim(crs_data)
```

    ## Skim summary statistics
    ##  n obs: 345 
    ##  n variables: 44 
    ## 
    ## -- Variable type:character -------------------------------------------------------------
    ##     variable missing complete   n min max empty n_unique
    ##  primaryproc       0      345 345   3  15     0        3
    ##          sex       0      345 345   4   6     0        2
    ## 
    ## -- Variable type:numeric ---------------------------------------------------------------
    ##         variable missing complete   n     mean      sd   p0   p25   p50
    ##      abscessleak       0      345 345   0.067    0.25   0     0     0  
    ##             afib       0      345 345   0.012    0.11   0     0     0  
    ##           agenew       0      345 345  59.12    13.72  16    51    60  
    ##     anemiatransf       0      345 345   0.1      0.3    0     0     0  
    ##              arf       0      345 345   0.026    0.16   0     0     0  
    ##              asa       0      345 345   2.23     0.53   1     2     2  
    ##              bmi       0      345 345  27.27     4.95  17.2  23.9  26.7
    ##              chf       0      345 345   0.038    0.19   0     0     0  
    ##             comp       0      345 345   0.29     0.46   0     0     0  
    ##         comp_pre       0      345 345   0.22     0.42   0     0     0  
    ##             copd       0      345 345   0.09     0.29   0     0     0  
    ##            crohn       0      345 345   0.0058   0.076  0     0     0  
    ##         day1stbm      40      305 345   2.67     1.75   0     2     2  
    ##            dc2fu       0      345 345 792.35   610.94   0   255   663  
    ##          death30       0      345 345   0.0058   0.076  0     0     0  
    ##         diabetes       0      345 345   0.12     0.33   0     0     0  
    ##            dvtpe       0      345 345   0.023    0.15   0     0     0  
    ##               hf       0      345 345   0.014    0.12   0     0     0  
    ##              ibd       0      345 345   0.029    0.17   0     0     0  
    ##               id       0      345 345 173       99.74   1    87   173  
    ##            ileus       0      345 345   0.23     0.42   0     0     0  
    ##              los       0      345 345   5.68     3.99   2     3     5  
    ##               mi       0      345 345   0.0029   0.054  0     0     0  
    ##              mis       0      345 345   0.37     0.48   0     0     0  
    ##         num_comp       0      345 345   0.37     0.63   0     0     0  
    ##        pneumonia       0      345 345   0.012    0.11   0     0     0  
    ##       preopchemo       0      345 345   0.46     0.5    0     0     0  
    ##         preopxrt       0      345 345   0.45     0.5    0     0     0  
    ##  priorabdsurgery       0      345 345   0.26     0.44   0     0     0  
    ##              pvd       0      345 345   0.012    0.11   0     0     0  
    ##        readmit30       0      345 345   0.12     0.33   0     0     0  
    ##            renal       0      345 345   0.032    0.18   0     0     0  
    ##        reopbleed       0      345 345   0.0058   0.076  0     0     0  
    ##         respfail       0      345 345   0.0058   0.076  0     0     0  
    ##          sboreop       0      345 345   0.012    0.11   0     0     0  
    ##           t2comp     244      101 345   8.01    12.38   0     3     4  
    ##       t2comp_pre     268       77 345   3.65     1.89   0     3     3  
    ##   tobaccocurrent       0      345 345   0.093    0.29   0     0     0  
    ##      tobaccoever       0      345 345   0.22     0.42   0     0     0  
    ##         ulcercol       0      345 345   0.023    0.15   0     0     0  
    ##              uti       0      345 345   0.046    0.21   0     0     0  
    ##          winfect       0      345 345   0.038    0.19   0     0     0  
    ##     p75   p100     hist
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##    69     90   <U+2581><U+2581><U+2583><U+2587><U+2587><U+2587><U+2585><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3      4   <U+2581><U+2581><U+2587><U+2581><U+2581><U+2583><U+2581><U+2581>
    ##    29.9   47.2 <U+2582><U+2585><U+2587><U+2583><U+2582><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     3     12   <U+2583><U+2587><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##  1297   2281   <U+2587><U+2586><U+2583><U+2583><U+2583><U+2583><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##   259    345   <U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587><U+2587>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     6     32   <U+2587><U+2583><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2585>
    ##     1      3   <U+2587><U+2581><U+2582><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2587>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2586>
    ##     1      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2583>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     7     96   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     5      9   <U+2583><U+2583><U+2587><U+2586><U+2583><U+2582><U+2582><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2582>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>
    ##     0      1   <U+2587><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581><U+2581>

## Session info

Print out the session info.

``` r
sessionInfo()
```

    ## R version 3.5.3 (2019-03-11)
    ## Platform: x86_64-w64-mingw32/x64 (64-bit)
    ## Running under: Windows 10 x64 (build 16299)
    ## 
    ## Matrix products: default
    ## 
    ## locale:
    ## [1] LC_COLLATE=English_United States.1252 
    ## [2] LC_CTYPE=English_United States.1252   
    ## [3] LC_MONETARY=English_United States.1252
    ## [4] LC_NUMERIC=C                          
    ## [5] LC_TIME=English_United States.1252    
    ## 
    ## attached base packages:
    ## [1] stats     graphics  grDevices utils     datasets  methods   base     
    ## 
    ## other attached packages:
    ## [1] here_0.1          skimr_1.0.7       Hmisc_4.2-0       Formula_1.2-3    
    ## [5] survival_2.44-1.1 lattice_0.20-38   readr_1.3.1       dplyr_0.8.3      
    ## [9] ggplot2_3.2.1    
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] tidyselect_0.2.5    xfun_0.6            purrr_0.3.2        
    ##  [4] splines_3.5.3       vctrs_0.2.0         colorspace_1.4-1   
    ##  [7] htmltools_0.3.6     yaml_2.2.0          base64enc_0.1-3    
    ## [10] utf8_1.1.4          rlang_0.4.0         pillar_1.4.2       
    ## [13] foreign_0.8-71      glue_1.3.1          withr_2.1.2        
    ## [16] RColorBrewer_1.1-2  stringr_1.4.0       munsell_0.5.0      
    ## [19] gtable_0.3.0        htmlwidgets_1.3     evaluate_0.13      
    ## [22] labeling_0.3        latticeExtra_0.6-28 knitr_1.22         
    ## [25] fansi_0.4.0         htmlTable_1.13.1    Rcpp_1.0.1         
    ## [28] acepack_1.4.1       scales_1.0.0        backports_1.1.3    
    ## [31] checkmate_1.9.1     gridExtra_2.3       hms_0.4.2          
    ## [34] digest_0.6.20       stringi_1.4.3       rprojroot_1.3-2    
    ## [37] grid_3.5.3          cli_1.1.0           tools_3.5.3        
    ## [40] magrittr_1.5        lazyeval_0.2.2      tibble_2.1.3       
    ## [43] cluster_2.0.7-1     tidyr_0.8.3         zeallot_0.1.0      
    ## [46] crayon_1.3.4        pkgconfig_2.0.2     Matrix_1.2-17      
    ## [49] data.table_1.12.2   assertthat_0.2.1    rmarkdown_1.12     
    ## [52] rstudioapi_0.10     R6_2.4.0            rpart_4.1-13       
    ## [55] nnet_7.3-12         compiler_3.5.3
