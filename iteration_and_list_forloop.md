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
    ## -2.86470 -0.62087  0.01240  0.04542  0.70472  2.48404

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
    ##  [1] 3.6019939 3.3547561 2.8449214 3.7340973 3.5394793 3.0691584 4.2879289
    ##  [8] 3.2361091 3.5248649 0.8551785 3.9019185 2.9964546 4.6368463 2.5406683
    ## [15] 3.4005787 2.4652631 2.3302186 4.5984648 2.8631812 3.1380073
    ## 
    ## $b
    ##  [1]  0.9930442  1.5310790 -1.4325290  0.1201040  3.2608407  4.2724588
    ##  [7]  4.0746426 -0.3610392 -3.4018892  3.5923025 -0.2330064 -2.5721618
    ## [13] -1.0318643 -4.3900554  1.6856697 -0.2347350 -2.1889104  3.0584534
    ## [19] -4.5475692  0.8292103 -3.5415865 -4.9964143  3.0417026  6.5805985
    ## [25] -2.8603338  4.2261694  3.9801355 12.2253585 -6.8372957  2.5952097
    ## 
    ## $c
    ##  [1] 10.197557  9.783447 10.006805  9.401182  9.958433 10.286380  9.959148
    ##  [8] 10.105246 10.232772  9.973465 10.389060  9.885994 10.035126 10.146736
    ## [15] 10.031792 10.062903  9.754425  9.824866  9.960108  9.688200 10.143688
    ## [22] 10.097748  9.886658 10.138227 10.299994  9.935457 10.234779  9.591482
    ## [29] 10.131428 10.352678  9.750017  9.923653 10.114528 10.196368 10.352934
    ## [36]  9.870973 10.141792 10.015177  9.930770 10.000990
    ## 
    ## $d
    ##  [1] -2.6024912 -3.8626005 -2.7786736 -3.6007562 -2.2369229 -3.6027301
    ##  [7] -3.1280059 -2.0442333 -4.5806491 -3.4360943 -3.4345226 -3.3664253
    ## [13] -2.8896296 -0.2072406 -3.2604910 -3.5361942 -3.7818756 -2.0309653
    ## [19] -3.4040886 -1.9135854

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
    ## 1  3.25 0.856

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.581  4.01

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  10.0 0.213

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -2.98 0.953

Let’s use a for loop

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  
  output[[i]] = mean_and_sd(list_norms[[i]])
  
}
```

## Let’s try map

``` r
output = map(list_norms, mean_and_sd)   # 跟for loop效果一样，格式：map(input, function)
```

What if you want a different function..?

``` r
output = map(list_norms, median)
output = map(list_norms, IQR)
```
