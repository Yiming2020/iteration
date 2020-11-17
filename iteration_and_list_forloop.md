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
    ## -3.15697 -0.63091  0.02494  0.01077  0.71960  2.72767

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
    ##  [1] 3.862231 5.384134 3.789726 2.113066 2.579906 3.526855 3.419771 3.494306
    ##  [9] 2.460289 2.903936 1.656323 3.368360 1.959953 1.822133 3.079759 2.091405
    ## [17] 5.563586 3.799629 3.890977 5.111362
    ## 
    ## $b
    ##  [1] -1.8030530  3.3694815  1.0356893 -1.3333497 -0.9605757  1.3212158
    ##  [7]  9.3669763 -7.6866779 -1.9416217 -0.1563309  2.5609309 -5.7542726
    ## [13]  4.0180609 -1.3693322 -5.7475751  9.5259335 -0.7903973 -6.1582437
    ## [19] -0.2470276 -1.3597515 -2.2468649 -0.3140679 -4.7493869  3.3356434
    ## [25]  2.5695713  0.7635898  0.9188399 -4.4864123 -4.4886293 -4.8310090
    ## 
    ## $c
    ##  [1]  9.932978 10.518281 10.423793  9.945816  9.885673  9.816294 10.046732
    ##  [8]  9.308880 10.138920 10.049167 10.135487 10.146946  9.792851  9.975239
    ## [15]  9.758250  9.701693  9.865082  9.936226  9.934084  9.911455  9.879300
    ## [22] 10.198555 10.172463  9.955034  9.990623  9.966245  9.860535 10.096291
    ## [29]  9.999128  9.933487  9.994564 10.211290 10.011810 10.023400 10.016745
    ## [36]  9.876971 10.202642  9.762163 10.068172  9.989125
    ## 
    ## $d
    ##  [1] -3.147004 -5.997755 -3.470393 -1.888890 -3.126954 -3.791568 -1.312225
    ##  [8] -4.255070 -3.585052 -3.174594 -3.267249 -3.588220 -4.368484 -2.074978
    ## [15] -2.739985 -2.048946 -3.069425 -2.281516 -2.555852 -3.926900

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
    ## 1  3.29  1.15

``` r
mean_and_sd(list_norms[[2]])
```

    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.588  4.12

``` r
mean_and_sd(list_norms[[3]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.199

``` r
mean_and_sd(list_norms[[4]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.18  1.05

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
    ##  [1] 3.862231 5.384134 3.789726 2.113066 2.579906 3.526855 3.419771 3.494306
    ##  [9] 2.460289 2.903936 1.656323 3.368360 1.959953 1.822133 3.079759 2.091405
    ## [17] 5.563586 3.799629 3.890977 5.111362
    ## 
    ## $b
    ##  [1] -1.8030530  3.3694815  1.0356893 -1.3333497 -0.9605757  1.3212158
    ##  [7]  9.3669763 -7.6866779 -1.9416217 -0.1563309  2.5609309 -5.7542726
    ## [13]  4.0180609 -1.3693322 -5.7475751  9.5259335 -0.7903973 -6.1582437
    ## [19] -0.2470276 -1.3597515 -2.2468649 -0.3140679 -4.7493869  3.3356434
    ## [25]  2.5695713  0.7635898  0.9188399 -4.4864123 -4.4886293 -4.8310090
    ## 
    ## $c
    ##  [1]  9.932978 10.518281 10.423793  9.945816  9.885673  9.816294 10.046732
    ##  [8]  9.308880 10.138920 10.049167 10.135487 10.146946  9.792851  9.975239
    ## [15]  9.758250  9.701693  9.865082  9.936226  9.934084  9.911455  9.879300
    ## [22] 10.198555 10.172463  9.955034  9.990623  9.966245  9.860535 10.096291
    ## [29]  9.999128  9.933487  9.994564 10.211290 10.011810 10.023400 10.016745
    ## [36]  9.876971 10.202642  9.762163 10.068172  9.989125
    ## 
    ## $d
    ##  [1] -3.147004 -5.997755 -3.470393 -1.888890 -3.126954 -3.791568 -1.312225
    ##  [8] -4.255070 -3.585052 -3.174594 -3.267249 -3.588220 -4.368484 -2.074978
    ## [15] -2.739985 -2.048946 -3.069425 -2.281516 -2.555852 -3.926900

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

    ##  [1] 3.862231 5.384134 3.789726 2.113066 2.579906 3.526855 3.419771 3.494306
    ##  [9] 2.460289 2.903936 1.656323 3.368360 1.959953 1.822133 3.079759 2.091405
    ## [17] 5.563586 3.799629 3.890977 5.111362

``` r
mean_and_sd(listcol_df$samp[[1]])
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.29  1.15

Can i just map?

``` r
map(listcol_df$samp, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.29  1.15
    ## 
    ## $b
    ## # A tibble: 1 x 2
    ##     mean    sd
    ##    <dbl> <dbl>
    ## 1 -0.588  4.12
    ## 
    ## $c
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  9.99 0.199
    ## 
    ## $d
    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -3.18  1.05

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
    ## 1 a     <dbl [20]>   <tibble [1 × 2]>   3.39 
    ## 2 b     <dbl [30]>   <tibble [1 × 2]>  -0.875
    ## 3 c     <dbl [40]>   <tibble [1 × 2]>   9.98 
    ## 4 d     <dbl [20]>   <tibble [1 × 2]>  -3.16

## weather data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USC00519397", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2017-01-01",
    date_max = "2017-12-31") %>%
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Waikiki_HA",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) %>%
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/yiming/Library/Caches/R/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2020-10-07 16:20:31 (7.525)

    ## file min/max dates: 1869-01-01 / 2020-10-31

    ## using cached file: /Users/yiming/Library/Caches/R/noaa_ghcnd/USC00519397.dly

    ## date created (size, mb): 2020-10-07 16:20:49 (1.699)

    ## file min/max dates: 1965-01-01 / 2020-03-31

    ## using cached file: /Users/yiming/Library/Caches/R/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2020-10-07 16:21:03 (0.88)

    ## file min/max dates: 1999-09-01 / 2020-10-31

Get our list columns

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin) # create a new column called "data", is going to result from nesting everything between date and tmin inside each of park station 

weather_nest
```

    ## # A tibble: 3 x 3
    ##   name           id          data              
    ##   <chr>          <chr>       <list>            
    ## 1 CentralPark_NY USW00094728 <tibble [365 × 4]>
    ## 2 Waikiki_HA     USC00519397 <tibble [365 × 4]>
    ## 3 Waterhole_WA   USS0023B17S <tibble [365 × 4]>

``` r
weather_nest %>% pull(name)
```

    ## [1] "CentralPark_NY" "Waikiki_HA"     "Waterhole_WA"

``` r
weather_nest %>% pull(data)
```

    ## [[1]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows
    ## 
    ## [[2]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0  26.7  16.7
    ##  2 2017-01-02     0  27.2  16.7
    ##  3 2017-01-03     0  27.8  17.2
    ##  4 2017-01-04     0  27.2  16.7
    ##  5 2017-01-05     0  27.8  16.7
    ##  6 2017-01-06     0  27.2  16.7
    ##  7 2017-01-07     0  27.2  16.7
    ##  8 2017-01-08     0  25.6  15  
    ##  9 2017-01-09     0  27.2  15.6
    ## 10 2017-01-10     0  28.3  17.2
    ## # … with 355 more rows
    ## 
    ## [[3]]
    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01   432  -6.8 -10.7
    ##  2 2017-01-02    25 -10.5 -12.4
    ##  3 2017-01-03     0  -8.9 -15.9
    ##  4 2017-01-04     0  -9.9 -15.5
    ##  5 2017-01-05     0  -5.9 -14.2
    ##  6 2017-01-06     0  -4.4 -11.3
    ##  7 2017-01-07    51   0.6 -11.5
    ##  8 2017-01-08    76   2.3  -1.2
    ##  9 2017-01-09    51  -1.2  -7  
    ## 10 2017-01-10     0  -5   -14.2
    ## # … with 355 more rows

``` r
weather_nest$data[[1]]
```

    ## # A tibble: 365 x 4
    ##    date        prcp  tmax  tmin
    ##    <date>     <dbl> <dbl> <dbl>
    ##  1 2017-01-01     0   8.9   4.4
    ##  2 2017-01-02    53   5     2.8
    ##  3 2017-01-03   147   6.1   3.9
    ##  4 2017-01-04     0  11.1   1.1
    ##  5 2017-01-05     0   1.1  -2.7
    ##  6 2017-01-06    13   0.6  -3.8
    ##  7 2017-01-07    81  -3.2  -6.6
    ##  8 2017-01-08     0  -3.8  -8.8
    ##  9 2017-01-09     0  -4.9  -9.9
    ## 10 2017-01-10     0   7.8  -6  
    ## # … with 355 more rows

``` r
unnest(weather_nest, cols = data)
```

    ## # A tibble: 1,095 x 6
    ##    name           id          date        prcp  tmax  tmin
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl>
    ##  1 CentralPark_NY USW00094728 2017-01-01     0   8.9   4.4
    ##  2 CentralPark_NY USW00094728 2017-01-02    53   5     2.8
    ##  3 CentralPark_NY USW00094728 2017-01-03   147   6.1   3.9
    ##  4 CentralPark_NY USW00094728 2017-01-04     0  11.1   1.1
    ##  5 CentralPark_NY USW00094728 2017-01-05     0   1.1  -2.7
    ##  6 CentralPark_NY USW00094728 2017-01-06    13   0.6  -3.8
    ##  7 CentralPark_NY USW00094728 2017-01-07    81  -3.2  -6.6
    ##  8 CentralPark_NY USW00094728 2017-01-08     0  -3.8  -8.8
    ##  9 CentralPark_NY USW00094728 2017-01-09     0  -4.9  -9.9
    ## 10 CentralPark_NY USW00094728 2017-01-10     0   7.8  -6  
    ## # … with 1,085 more rows

``` r
weather_nest = 
  weather_df %>% 
  nest(data = date:tmin)
```

Suppose I want to regress tmax on tmin for each station

``` r
lm(tmax ~ tmin, data = weather_nest$data[[3]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = weather_nest$data[[3]])
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
weather_lm = function(df) {
  lm(tmax ~ tmin, data = df)
}

weather_lm(weather_nest$data[[1]])
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039

``` r
output = vector("list", 3)

for (i in 1:3){
  output[[i]] = weather_lm(weather_nest$data[[i]])
}
```

Since weather$data is a list, we can apply our weather\_lm function to
each data frame using map.

``` r
map(weather_nest$data, weather_lm)
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

``` r
map(weather_nest$data, ~lm(tmax ~ tmin, data = .x))
```

    ## [[1]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.209        1.039  
    ## 
    ## 
    ## [[2]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##     20.0966       0.4509  
    ## 
    ## 
    ## [[3]]
    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = .x)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.499        1.221

What about a map in a list column\!\!\!?

``` r
weather_nest = 
  weather_nest %>% 
  mutate(models = map(data, weather_lm)) # add a column called models to save map results

weather_nest
```

    ## # A tibble: 3 x 4
    ##   name           id          data               models
    ##   <chr>          <chr>       <list>             <list>
    ## 1 CentralPark_NY USW00094728 <tibble [365 × 4]> <lm>  
    ## 2 Waikiki_HA     USC00519397 <tibble [365 × 4]> <lm>  
    ## 3 Waterhole_WA   USS0023B17S <tibble [365 × 4]> <lm>
