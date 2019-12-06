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

    ## Warning: Missing column names filled in: 'X1' [1]

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double(),
    ##   sex = col_character(),
    ##   primaryproc = col_character()
    ## )

    ## See spec(...) for full column specifications.

Examine the the contents of the data set.

``` r
Hmisc::describe(crs_data) 
```

    ## crs_data 
    ## 
    ##  45  Variables      345  Observations
    ## ---------------------------------------------------------------------------
    ## X1 
    ##        n  missing distinct     Info     Mean      Gmd      .05      .10 
    ##      345        0      345        1      173    115.3     18.2     35.4 
    ##      .25      .50      .75      .90      .95 
    ##     87.0    173.0    259.0    310.6    327.8 
    ## 
    ## lowest :   1   2   3   4   5, highest: 341 342 343 344 345
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
    ## [1] here_0.1          Hmisc_4.2-0       Formula_1.2-3     survival_2.44-1.1
    ## [5] lattice_0.20-38   readr_1.3.1       dplyr_0.8.3       ggplot2_3.2.1    
    ## 
    ## loaded via a namespace (and not attached):
    ##  [1] tidyselect_0.2.5    xfun_0.6            purrr_0.3.2        
    ##  [4] splines_3.5.3       colorspace_1.4-1    htmltools_0.3.6    
    ##  [7] yaml_2.2.0          base64enc_0.1-3     rlang_0.4.0        
    ## [10] pillar_1.4.2        foreign_0.8-71      glue_1.3.1         
    ## [13] withr_2.1.2         RColorBrewer_1.1-2  stringr_1.4.0      
    ## [16] munsell_0.5.0       gtable_0.3.0        htmlwidgets_1.3    
    ## [19] evaluate_0.13       latticeExtra_0.6-28 knitr_1.22         
    ## [22] htmlTable_1.13.1    Rcpp_1.0.1          acepack_1.4.1      
    ## [25] scales_1.0.0        backports_1.1.3     checkmate_1.9.1    
    ## [28] gridExtra_2.3       hms_0.4.2           digest_0.6.20      
    ## [31] stringi_1.4.3       rprojroot_1.3-2     grid_3.5.3         
    ## [34] tools_3.5.3         magrittr_1.5        lazyeval_0.2.2     
    ## [37] tibble_2.1.3        cluster_2.0.7-1     crayon_1.3.4       
    ## [40] pkgconfig_2.0.2     Matrix_1.2-17       data.table_1.12.2  
    ## [43] assertthat_0.2.1    rmarkdown_1.12      rstudioapi_0.10    
    ## [46] R6_2.4.0            rpart_4.1-13        nnet_7.3-12        
    ## [49] compiler_3.5.3
