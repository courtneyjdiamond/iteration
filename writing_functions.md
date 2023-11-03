Writing Functions
================
2023-11-03

Set the seed for reproducibility.

``` r
set.seed(12345)
```

### Z score function

Z scores subtract the mean and divid by the sd.

``` r
x_vec = rnorm(20, mean = 5, sd = 0.3)
```

Compute Z scores for `x_vec`.

``` r
(x_vec - mean(x_vec)) / sd(x_vec)
```

    ##  [1]  0.6103734  0.7589907 -0.2228232 -0.6355576  0.6347861 -2.2717259
    ##  [7]  0.6638185 -0.4229355 -0.4324994 -1.1941438 -0.2311505  2.0874460
    ## [13]  0.3526784  0.5320552 -0.9917420  0.8878182 -1.1546150 -0.4893597
    ## [19]  1.2521303  0.2664557

Write a function to do this!

``` r
z_score = function(x) {
  
  if (!is.numeric(x)) {
    
    stop("Argument should be numbers")
    
  }  else if (length(x) < 2) {
    
    stop("You need at least 2 numbers to get z scores")
    
  }
  
  z = (x-mean(x)) / sd(x) 
 
  z
 
}
```

Check that this works.

``` r
z_score(x = x_vec)
```

    ##  [1]  0.6103734  0.7589907 -0.2228232 -0.6355576  0.6347861 -2.2717259
    ##  [7]  0.6638185 -0.4229355 -0.4324994 -1.1941438 -0.2311505  2.0874460
    ## [13]  0.3526784  0.5320552 -0.9917420  0.8878182 -1.1546150 -0.4893597
    ## [19]  1.2521303  0.2664557

``` r
z_score(x = rnorm(10, mean = 5))
```

    ##  [1]  0.5952213  1.1732833 -0.6221352 -1.3990896 -1.4371950  1.4719158
    ##  [7] -0.4830567  0.4590828  0.4520244 -0.2100512

Keep checking.

``` r
#z_score(x = 3)

#z_score(c("my", "name", "is"))

#z_score(c(TRUE, TRUE, FALSE, TRUE))

#z_score(iris)
```
