Iteration and list document
================

## List

You can put anything in a list

``` r
l = list(
  vec_numeric = 5:8,
  vec_logical = c(TRUE,TRUE, FALSE, TRUE,FALSE,FALSE),
  mat         = matrix(1:8, nrow = 2, ncol = 4),
  summary     = summary(rnorm(1000)))

l
```

    ## $vec_numeric
    ## [1] 5 6 7 8
    ## 
    ## $vec_logical
    ## [1]  TRUE  TRUE FALSE  TRUE FALSE FALSE
    ## 
    ## $mat
    ##      [,1] [,2] [,3] [,4]
    ## [1,]    1    3    5    7
    ## [2,]    2    4    6    8
    ## 
    ## $summary
    ##     Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    ## -3.08459 -0.73074  0.03947 -0.01015  0.69409  3.26916

``` r
l$vec_numeric
```

    ## [1] 5 6 7 8

``` r
l[[1]]   #show the first vector in the list, 从结果上，跟l$vec_numeric是一样的
```

    ## [1] 5 6 7 8

``` r
l[[1]][1:3]
```

    ## [1] 5 6 7

``` r
mean(l[["vec_numeric"]])
```

    ## [1] 6.5

## For loop

Create a new list

``` r
list_norms = 
  list(
    a = rnorm(20, 3, 1),
    b = rnorm(30, 0, 5),
    c = rnorm(40, 10, .2),
    d = rnorm(20, -3, 1)
  )

is.list(list_norms)
```

    ## [1] TRUE

``` r
list_norms
```

    ## $a
    ##  [1] 0.3402488 2.6547973 4.3118761 1.6118306 3.6573590 4.3609045 4.3196352
    ##  [8] 4.1179013 1.9924903 2.5522422 3.7607226 3.9569873 2.4340251 2.6478637
    ## [15] 4.2218211 2.6762163 3.7705142 5.9074660 1.9577428 1.7878087
    ## 
    ## $b
    ##  [1]  -5.4359137   2.5173848  -1.3819520  -3.0052274  -2.7927608  -2.5602820
    ##  [7]   9.1270952  -6.3970241   1.8531010  -4.0742324   4.9242790   9.0475384
    ## [13]  -4.3568598  -4.7520786   2.8875297   1.3255778   4.1339620   3.0559829
    ## [19]   8.9313416 -11.0114988   1.8177288   0.5790390  -0.5745645   0.3327219
    ## [25]  -0.1219198   3.1003728   5.1687523   4.7003400  -7.1889683   3.3047124
    ## 
    ## $c
    ##  [1] 10.188330  9.514060 10.388436  9.763571  9.796609  9.536886 10.054468
    ##  [8]  9.870832 10.322563 10.127158  9.688534  9.942972  9.676962  9.845019
    ## [15] 10.110817 10.261236  9.681113  9.859688 10.154947 10.014649 10.041652
    ## [22]  9.949387 10.202383 10.150301 10.136052 10.077519 10.397635  9.877654
    ## [29]  9.991702  9.990564 10.118657 10.158241 10.122362 10.119570 10.156519
    ## [36] 10.258807  9.720709  9.963101 10.157245 10.207633
    ## 
    ## $d
    ##  [1] -1.5343770 -3.2100761 -2.7922542 -2.5259642 -2.3604534 -2.8540130
    ##  [7] -4.5338914 -3.1888684 -2.2512162 -2.3967565 -3.2542976 -2.9653681
    ## [13] -3.5969109 -3.6810484 -2.6814335 -0.5303605 -2.8283795 -3.1484692
    ## [19] -2.5120702 -2.6605082

Pause and get my old function

``` r
mean_and_sd = function(x) {
  
  if(!is.numeric(x)){
    stop("Input must be numeric")
  }
  
  if(length(x) < 3){
    stop("Inpur must have at least three numbers")
  }
  
  mean_x = mean(x)
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
 
}
```

I can apply that function to each list element

``` r
mean_and_sd(list_norms[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.15  1.30

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.438  4.95

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.220

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.78 0.817

Let’s use a for loop

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norms[[i]])
  
}
```
