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
    ##      Min.   1st Qu.    Median      Mean   3rd Qu.      Max. 
    ## -2.736983 -0.660151  0.018398  0.009252  0.673348  3.057163

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
    ##  [1] 3.091398 3.335709 2.353932 2.406150 2.008655 1.992330 1.550193 2.349703
    ##  [9] 2.970643 3.100191 3.423916 3.540384 4.040406 3.981634 2.427986 3.137913
    ## [17] 2.914657 1.904183 2.933263 2.490173
    ## 
    ## $b
    ##  [1]   2.5714931   5.1459582 -11.5578836  -3.1035717   5.4022829   3.6810003
    ##  [7]   4.2069379  -6.1107527   1.2091062  -3.4593244  -3.1928052   4.0922104
    ## [13]   0.8194204  -1.0498358   6.3921402   6.7059501   6.3570390   7.5800366
    ## [19]   0.7393984   6.5486190  -1.6419604   8.3741877  -2.9986892   5.4788939
    ## [25]  -0.4697216   2.8351115  -2.1754069   2.6923255   9.5567801  -1.9747560
    ## 
    ## $c
    ##  [1]  9.947426 10.013355 10.015792  9.808066 10.102060  9.763794  9.880496
    ##  [8]  9.835059  9.783989  9.823580  9.657526 10.377657  9.730362 10.410551
    ## [15] 10.107588  9.699657  9.958159  9.962775  9.817953  9.842928 10.049398
    ## [22] 10.018186  9.611168  9.802383  9.940762 10.186815  9.954986  9.843584
    ## [29] 10.187257  9.575952 10.134644 10.145193  9.832391 10.381912  9.840709
    ## [36]  9.935693  9.766145 10.027872  9.892796 10.260629
    ## 
    ## $d
    ##  [1] -3.132089 -1.859306 -3.045862 -2.718962 -3.776507 -3.598298 -3.130737
    ##  [8] -3.834433 -4.469849 -2.014066 -3.226520 -3.726169 -3.657717 -3.175381
    ## [15] -2.169103 -2.954915 -4.517093 -2.125333 -4.301844 -2.601110

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
    ## 1  2.80 0.686

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.76  4.84

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.95 0.205

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.20 0.796

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
output = map(.x = list_norms, ~ mean_and_sd(.x))
```

What if you want a different function..?

``` r
output = map(list_norms, median)
output = map(list_norms, IQR)   # output as list
```

``` r
output = map_dbl(list_norms, median) # output 可以以不同格式输出，lec有提及
```

``` r
output = map_df(list_norms, mean_and_sd) # output 以dataframe格式输出
output = map_df(list_norms, mean_and_sd, .id = "input")   #put the abcd names into a column called "input"
```

## list columns

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    samp = list_norms
  )
```

``` r
listcol_df %>% pull(name)
```

    ## [1] "a" "b" "c" "d"

``` r
listcol_df %>% pull(samp)
```

    ## $a
    ##  [1] 3.091398 3.335709 2.353932 2.406150 2.008655 1.992330 1.550193 2.349703
    ##  [9] 2.970643 3.100191 3.423916 3.540384 4.040406 3.981634 2.427986 3.137913
    ## [17] 2.914657 1.904183 2.933263 2.490173
    ## 
    ## $b
    ##  [1]   2.5714931   5.1459582 -11.5578836  -3.1035717   5.4022829   3.6810003
    ##  [7]   4.2069379  -6.1107527   1.2091062  -3.4593244  -3.1928052   4.0922104
    ## [13]   0.8194204  -1.0498358   6.3921402   6.7059501   6.3570390   7.5800366
    ## [19]   0.7393984   6.5486190  -1.6419604   8.3741877  -2.9986892   5.4788939
    ## [25]  -0.4697216   2.8351115  -2.1754069   2.6923255   9.5567801  -1.9747560
    ## 
    ## $c
    ##  [1]  9.947426 10.013355 10.015792  9.808066 10.102060  9.763794  9.880496
    ##  [8]  9.835059  9.783989  9.823580  9.657526 10.377657  9.730362 10.410551
    ## [15] 10.107588  9.699657  9.958159  9.962775  9.817953  9.842928 10.049398
    ## [22] 10.018186  9.611168  9.802383  9.940762 10.186815  9.954986  9.843584
    ## [29] 10.187257  9.575952 10.134644 10.145193  9.832391 10.381912  9.840709
    ## [36]  9.935693  9.766145 10.027872  9.892796 10.260629
    ## 
    ## $d
    ##  [1] -3.132089 -1.859306 -3.045862 -2.718962 -3.776507 -3.598298 -3.130737
    ##  [8] -3.834433 -4.469849 -2.014066 -3.226520 -3.726169 -3.657717 -3.175381
    ## [15] -2.169103 -2.954915 -4.517093 -2.125333 -4.301844 -2.601110

``` r
listcol_df %>% 
  filter(name == "a")
```

    ## # A tibble: 1 x 2
    ##   name  samp        
    ##   <chr> <named list>
    ## 1 a     <dbl [20]>

Let’s try some operation

``` r
listcol_df$samp[[1]]
```

    ##  [1] 3.091398 3.335709 2.353932 2.406150 2.008655 1.992330 1.550193 2.349703
    ##  [9] 2.970643 3.100191 3.423916 3.540384 4.040406 3.981634 2.427986 3.137913
    ## [17] 2.914657 1.904183 2.933263 2.490173

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.80 0.686

Can i just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.80 0.686
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.76  4.84
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.95 0.205
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.20 0.796

So, can I add a list column???—-yes

``` r
listcol_df = 
  listcol_df %>% 
  mutate(summary = map(samp, mean_and_sd),
         medians = map_dbl(samp, median))

listcol_df
```

    ## # A tibble: 4 x 4
    ##   name  samp         summary          medians
    ##   <chr> <named list> <named list>       <dbl>
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>    2.92
    ## 2 b     <dbl [30]>   <tibble [1 × 2]>    2.63
    ## 3 c     <dbl [40]>   <tibble [1 × 2]>    9.94
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>   -3.15
