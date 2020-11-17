make function
================

## Do something simple

``` r
x_vec = rnorm(30, mean = 5, sd = 3)

(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  1.5402581 -0.5297896 -0.5767672 -0.8565913 -1.8446595  0.4618644
    ##  [7] -0.5421714  0.2800615 -0.3288947  1.9488745 -0.2745493 -1.0430889
    ## [13] -0.9344828 -0.1399383 -0.4694438 -1.3854395  0.9336195 -0.5832629
    ## [19]  0.3206256  0.5071296  0.4556579 -1.8457539  1.7624344  1.0149519
    ## [25]  0.9600225  0.1553747 -0.7077540  1.5067619 -0.1445186  0.3594691

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

    ##  [1]  1.5402581 -0.5297896 -0.5767672 -0.8565913 -1.8446595  0.4618644
    ##  [7] -0.5421714  0.2800615 -0.3288947  1.9488745 -0.2745493 -1.0430889
    ## [13] -0.9344828 -0.1399383 -0.4694438 -1.3854395  0.9336195 -0.5832629
    ## [19]  0.3206256  0.5071296  0.4556579 -1.8457539  1.7624344  1.0149519
    ## [25]  0.9600225  0.1553747 -0.7077540  1.5067619 -0.1445186  0.3594691

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
    ## 1 -0.0373  1.00

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
    ## 1  4.55  2.71

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
    ## 1  6.29  2.77

``` r
sim_mean_sd(miu = 6, sigma = 7, sample_size = 500) #miu = 6, sigma = 7 rewrite the default numbers
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  5.80  7.04

``` r
sim_mean_sd(sample_size = 500)#因为default miu = 3, sigma = 4, 所以function can still work
```

    ## # A tibble: 1 x 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  3.19  3.90

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
    ##    title                stars        text                                       
    ##    <chr>                <chr>        <chr>                                      
    ##  1 One big yawn         1.0 out of … Stupid movie.                              
    ##  2 Vote for Pedro       5.0 out of … Good quality American format and my wife h…
    ##  3 NAP-MITE             5.0 out of … I like this movie and recomend it.         
    ##  4 Vote for Pedro!      5.0 out of … Just watch the movie. Gosh!                
    ##  5 Just watch the frea… 5.0 out of … Its a great movie, gosh!!                  
    ##  6 Great Value          5.0 out of … Great Value                                
    ##  7 I LOVE THIS MOVIE    5.0 out of … THIS MOVIE IS SO FUNNY ONE OF MY FAVORITES 
    ##  8 Don't you wish you … 5.0 out of … Watch it 100 times. Never. Gets. Old.      
    ##  9 Stupid, but very fu… 5.0 out of … If you like stupidly funny '90s teenage mo…
    ## 10 The beat             5.0 out of … The best
