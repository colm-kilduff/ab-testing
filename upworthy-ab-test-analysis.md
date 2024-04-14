Upworthy research archive analysis
================

``` r
#knit(getwd())
```

### Introduction

In this report, we will be analysing the [upworthy research
archive](https://osf.io/jd64p/). The upworthy research archive is a
dataset of thousands of A/B tests of headlines conducted by Upworthy
from January 2013 to April 2015.

### Analysis

We load in the dataset and show a sample of the dataset below.

``` r
exploratory_data <-
  read_csv(glue('{getwd()}/datasets/upworthy-archive-exploratory-packages-03.12.2020.csv'), show_col_types = FALSE) 
```

    ## New names:
    ## • `` -> `...1`

``` r
exploratory_data <-
  exploratory_data |>
  rename(id = `...1`)

exploratory_data |>
  head(10)
```

    ## # A tibble: 10 × 17
    ##       id created_at          updated_at          clickability_test_id    excerpt headline lede 
    ##    <dbl> <dttm>              <dttm>              <chr>                   <chr>   <chr>    <chr>
    ##  1     0 2014-11-20 06:43:16 2016-04-02 16:33:38 546d88fb84ad38b2ce0000… Things… They're… "<p>…
    ##  2     1 2014-11-20 06:43:44 2016-04-02 16:25:54 546d88fb84ad38b2ce0000… Things… They're… "<p>…
    ##  3     2 2014-11-20 06:44:59 2016-04-02 16:25:54 546d88fb84ad38b2ce0000… Things… They're… "<p>…
    ##  4     3 2014-11-20 06:54:36 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ##  5     4 2014-11-20 06:54:57 2016-04-02 16:31:45 546d902c26714c6c440000… Things… This Is… "<p>…
    ##  6     5 2014-11-20 06:55:07 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ##  7     6 2014-11-20 06:55:20 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ##  8     7 2014-11-20 06:55:43 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ##  9     8 2014-11-20 06:55:57 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ## 10     9 2014-11-20 06:56:08 2016-04-02 16:25:54 546d902c26714c6c440000… Things… This Is… "<p>…
    ## # ℹ 10 more variables: slug <chr>, eyecatcher_id <chr>, impressions <dbl>, clicks <dbl>,
    ## #   significance <dbl>, first_place <lgl>, winner <lgl>, share_text <chr>, square <chr>,
    ## #   test_week <dbl>

We want to understand more about the dataset before we get into an
analysis of the A/B test results.

``` r
exploratory_data |>
  summary()
```

    ##        id           created_at                       updated_at                    
    ##  Min.   :     0   Min.   :2013-01-26 20:10:36.00   Min.   :2016-04-02 16:24:06.83  
    ##  1st Qu.: 37068   1st Qu.:2014-01-14 03:10:22.58   1st Qu.:2016-04-02 16:26:25.11  
    ##  Median : 74988   Median :2014-07-27 21:19:58.41   Median :2016-04-02 16:28:29.98  
    ##  Mean   : 75219   Mean   :2014-06-08 23:39:50.42   Mean   :2016-04-02 18:07:37.68  
    ##  3rd Qu.:112953   3rd Qu.:2014-11-04 14:01:38.69   3rd Qu.:2016-04-02 16:30:33.35  
    ##  Max.   :150816   Max.   :2015-04-29 22:46:43.61   Max.   :2016-10-03 16:07:22.97  
    ##  clickability_test_id   excerpt            headline             lede          
    ##  Length:22666         Length:22666       Length:22666       Length:22666      
    ##  Class :character     Class :character   Class :character   Class :character  
    ##  Mode  :character     Mode  :character   Mode  :character   Mode  :character  
    ##                                                                               
    ##                                                                               
    ##                                                                               
    ##      slug           eyecatcher_id       impressions        clicks        significance   
    ##  Length:22666       Length:22666       Min.   :   13   Min.   :  0.00   Min.   :  0.00  
    ##  Class :character   Class :character   1st Qu.: 2740   1st Qu.: 24.00   1st Qu.:  2.80  
    ##  Mode  :character   Mode  :character   Median : 3122   Median : 42.00   Median : 25.70  
    ##                                        Mean   : 3575   Mean   : 54.32   Mean   : 40.73  
    ##                                        3rd Qu.: 4091   3rd Qu.: 70.00   3rd Qu.: 85.40  
    ##                                        Max.   :25314   Max.   :975.00   Max.   :100.00  
    ##  first_place       winner         share_text           square            test_week     
    ##  Mode :logical   Mode :logical   Length:22666       Length:22666       Min.   :201303  
    ##  FALSE:17843     FALSE:21514     Class :character   Class :character   1st Qu.:201402  
    ##  TRUE :4823      TRUE :1152      Mode  :character   Mode  :character   Median :201430  
    ##                                                                        Mean   :201418  
    ##                                                                        3rd Qu.:201444  
    ##                                                                        Max.   :201517

``` r
exploratory_data |>
  summarise(num_unique_ab_tests = n_distinct(clickability_test_id), num_winners = sum(winner))
```

    ## # A tibble: 1 × 2
    ##   num_unique_ab_tests num_winners
    ##                 <int>       <int>
    ## 1                4873        1152
