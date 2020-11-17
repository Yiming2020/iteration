make function
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.693379553  0.826801671  0.673205054 -1.991774039  0.263995523
    ##  [6] -1.267179194  1.077727179  0.502503782  1.138159507 -0.001696264
    ## [11]  0.383415880  1.169675830  0.011627981  0.758690225 -0.671967788
    ## [16] -0.010059895 -1.345166343 -1.576382156  0.076791558 -1.738002756
    ## [21]  0.400971863  0.471377510 -0.120524549  0.866572126  0.457959606
    ## [26]  0.236890785  1.397957549 -2.155010854 -0.976936504  0.446997161

I want a function to compute z-scores

``` r
z_scores = function(x) {
  
  if(!is.numeric(x)){
    stop("Input must be numeric")
  }
  
  if(length(x) < 3){
    stop("Inpur must have at least three numbers")
  }
  
  z = (x - mean(x))/sd(x)
  
  return(z)
 
}

z_scores(x_vec)
```

    ##  [1]  0.693379553  0.826801671  0.673205054 -1.991774039  0.263995523
    ##  [6] -1.267179194  1.077727179  0.502503782  1.138159507 -0.001696264
    ## [11]  0.383415880  1.169675830  0.011627981  0.758690225 -0.671967788
    ## [16] -0.010059895 -1.345166343 -1.576382156  0.076791558 -1.738002756
    ## [21]  0.400971863  0.471377510 -0.120524549  0.866572126  0.457959606
    ## [26]  0.236890785  1.397957549 -2.155010854 -0.976936504  0.446997161

Try my function on some other things. These should give errors

``` r
z_scores(3) #return NA because sd(3) is NA
```

    ## Error in z_scores(3): Inpur must have at least three numbers

``` r
z_scores("my name is jeff") # cannot return anything because those are characters
```

    ## Error in z_scores("my name is jeff"): Input must be numeric

``` r
z_scores(mycars) #cannot take a mean of a dataset
```

    ## Error in z_scores(mycars): object 'mycars' not found

``` r
z_scores(c(TRUE, TRUE, FALSE, FALSE)) # It works becase R consider them as 1100
```

    ## Error in z_scores(c(TRUE, TRUE, FALSE, FALSE)): Input must be numeric

## Multiple outputs

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
  
  list(
    mean = mean_x,
    sd = sd_x
  )
 
}
```

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

Check that the function works

``` r
x_vec = rnorm(1000)
mean_and_sd(x_vec)
```

    ## # A tibble: 1 x 2
    ##       mean    sd
    ##      <dbl> <dbl>
    ## 1 -0.00662 0.974

## multiple inputs

``` r
sim_data = 
  tibble(
    x = rnorm(n = 100, mean = 4, sd = 3)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.71  2.75

I’d like to do this with function

``` r
sim_mean_sd = function(sample_size, miu = 3, sigma = 4){
  sim_data = 
    tibble(
    x = rnorm(sample_size, mean = miu, sd = sigma)
  )

sim_data %>% 
  summarize(
    mean = mean(x),
    sd = sd(x)
  )
  
}

sim_mean_sd(100, 6, 3)
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.99  2.71

``` r
sim_mean_sd(miu = 6, sigma = 7, sample_size = 500) #miu = 6, sigma = 7 rewrite the default numbers
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.95  7.20

``` r
sim_mean_sd(sample_size = 500)#因为default miu = 3, sigma = 4, 所以function can still work
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.82  3.97
