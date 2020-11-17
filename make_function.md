make function
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1] -0.24370904  0.79035061 -0.49627317 -0.25526499 -0.93735463  0.67561844
    ##  [7] -1.51516985 -0.97648625  0.24644333 -0.56876158 -1.76173401 -1.65718792
    ## [13] -0.27808099 -0.71765784  0.78001454  1.79494853  0.18580966  0.20752559
    ## [19] -1.11625962  1.87912235 -0.01171033 -0.79441254  0.72353309  1.86033973
    ## [25]  0.31215512  0.37780571 -0.64511314  0.27772939  0.29053306  1.57324675

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

    ##  [1] -0.24370904  0.79035061 -0.49627317 -0.25526499 -0.93735463  0.67561844
    ##  [7] -1.51516985 -0.97648625  0.24644333 -0.56876158 -1.76173401 -1.65718792
    ## [13] -0.27808099 -0.71765784  0.78001454  1.79494853  0.18580966  0.20752559
    ## [19] -1.11625962  1.87912235 -0.01171033 -0.79441254  0.72353309  1.86033973
    ## [25]  0.31215512  0.37780571 -0.64511314  0.27772939  0.29053306  1.57324675

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
    ##      mean    sd
    ##     <dbl> <dbl>
    ## 1 0.00359 0.983

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
    ## 1  4.39  3.23

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
    ## 1  5.74  3.07

``` r
sim_mean_sd(miu = 6, sigma = 7, sample_size = 500) #miu = 6, sigma = 7 rewrite the default numbers
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.68  7.03

``` r
sim_mean_sd(sample_size = 500)#因为default miu = 3, sigma = 4, 所以function can still work
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  2.78  3.91

Let’s do the dynamite sample

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

dynamite_html = read_html(url)

review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()

review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>%
  as.numeric()

review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()

reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)

reviews
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, stars <dbl>, text <chr>

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"

swm_html = read_html(url)
```

Grab elements that I want

``` r
# use select gadget to determine what kind of movie title it use
title_vec = 
  swm_html %>% 
  html_nodes(css = ".lister-item-header a") %>% ###".lister-item-header a"这个东西是用gadget 查出来的，每次都不不同
  html_text()

gross_rev_vec = 
  swm_html %>% 
  html_nodes(css = ".a-text-bold span") %>% 
  html_text()

runtime_vec = 
  swm_html %>% 
  html_nodes(css = ".runtime") %>% 
  html_text()

swm_df = 
  tibble(
    title = title_vec,
    gross_rev = gross_rev_vec,
    runtime = runtime_vec
  )

swm_df
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, gross_rev <chr>, runtime <chr>

## Let’s review Napoleon Dynamite

``` r
url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=1"
dynamite_html = read_html(url)
review_titles = 
  dynamite_html %>%
  html_nodes(".a-text-bold span") %>%
  html_text()
review_stars = 
  dynamite_html %>%
  html_nodes("#cm_cr-review_list .review-rating") %>%
  html_text() %>%
  str_extract("^\\d") %>% # "\\d" means first digit number between 0 to9, "^\\d" mean first digit at the begining of whatever the string is
  as.numeric()
review_text = 
  dynamite_html %>%
  html_nodes(".review-text-content span") %>%
  html_text() %>% 
  str_replace_all("\n", "") %>% 
  str_trim()
reviews = tibble(
  title = review_titles,
  stars = review_stars,
  text = review_text
)
```

What about the next page of reviews…

Let’s turn that code into a function

``` r
read_page_reviews = function(url) {
  
  html = read_html(url)
  
  review_titles = 
    html %>%
    html_nodes(".a-text-bold span") %>%
    html_text()
  
  review_stars = 
    html %>%
    html_nodes("#cm_cr-review_list .review-rating") %>%
    html_text()
  
  review_text = 
    html %>%
    html_nodes(".review-text-content span") %>%
    html_text() %>% 
    str_replace_all("\n", "") %>% 
    str_trim()
  
  reviews = 
    tibble(
      title = review_titles,
      stars = review_stars,
      text = review_text
    )
  
  reviews
  
}
```

Let me try my function.

``` r
dynamite_url = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber=2"
read_page_reviews(dynamite_url)
```

    ## # A tibble: 0 x 3
    ## # … with 3 variables: title <chr>, stars <chr>, text <chr>

Let’s read a few pages of reviews.

``` r
dynamite_url_base = "https://www.amazon.com/product-reviews/B00005JNBQ/ref=cm_cr_arp_d_viewopt_rvwer?ie=UTF8&reviewerType=avp_only_reviews&sortBy=recent&pageNumber="
dynamite_urls = str_c(dynamite_url_base, 1:50)

all_reviews = 
  bind_rows(
    read_page_reviews(dynamite_urls[1]),
    read_page_reviews(dynamite_urls[2]),
    read_page_reviews(dynamite_urls[3]),
    read_page_reviews(dynamite_urls[4]),
    read_page_reviews(dynamite_urls[5])
  )

all_reviews
```

    ## # A tibble: 10 x 3
    ##    title                            stars      text                             
    ##    <chr>                            <chr>      <chr>                            
    ##  1 Hilarious                        5.0 out o… "Super funny! Loved the online r…
    ##  2 Love this movie                  5.0 out o… "We love this product.  It came …
    ##  3 Boo                              1.0 out o… "We rented this movie because ou…
    ##  4 Movie is still silly fun....ama… 1.0 out o… "We are getting really frustrate…
    ##  5 Brilliant and awkwardly funny.   5.0 out o… "I've watched this movie repeate…
    ##  6 Great purchase price for great … 5.0 out o… "Great movie and real good digit…
    ##  7 Movie for memories               5.0 out o… "I've been looking for this movi…
    ##  8 Love!                            5.0 out o… "Love this movie. Great quality" 
    ##  9 Hilarious!                       5.0 out o… "Such a funny movie, definitely …
    ## 10 napoleon dynamite                5.0 out o… "cool movie"

## Mean scoping example

``` r
f = function(x) {
  z = x + y
  z
}
x = 1
y = 2
f(x = y) # this means take x = 2(y=2) inside the function
```

    ## [1] 4

## Functions as arguments

``` r
my_summary = function(x, summ_func) {
  
  summ_func(x)
  
}
x_vec = rnorm(100, 3, 7)
mean(x_vec)
```

    ## [1] 3.313981

``` r
median(x_vec)
```

    ## [1] 3.185138

``` r
my_summary(x_vec, IQR)
```

    ## [1] 9.309237
