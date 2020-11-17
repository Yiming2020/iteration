make function
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  1.96842252  1.05592724 -0.49571412  1.36766514 -0.56281994 -0.48103997
    ##  [7]  0.58254808 -0.40499515  1.70846057 -0.87398291 -0.65345275  0.40486435
    ## [13] -0.01567534 -0.22322217  0.44913887  0.36280287 -0.46006176 -0.14948786
    ## [19] -1.11536386 -1.05308441 -1.33912162  0.68302718 -0.06733838  1.36500956
    ## [25]  0.07829542 -1.61207419 -0.22133130 -1.09703286  2.02675409 -1.22711730

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

    ##  [1]  1.96842252  1.05592724 -0.49571412  1.36766514 -0.56281994 -0.48103997
    ##  [7]  0.58254808 -0.40499515  1.70846057 -0.87398291 -0.65345275  0.40486435
    ## [13] -0.01567534 -0.22322217  0.44913887  0.36280287 -0.46006176 -0.14948786
    ## [19] -1.11536386 -1.05308441 -1.33912162  0.68302718 -0.06733838  1.36500956
    ## [25]  0.07829542 -1.61207419 -0.22133130 -1.09703286  2.02675409 -1.22711730

Try my function on some other things

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
