listcols
================
2023-11-03

### Lists

``` r
vec_numeric = c(1:4) 
vec_character = c("my", "name", "is", "courtney")

tibble(
  num = vec_numeric,
  char = vec_character
)
```

    ## # A tibble: 4 × 2
    ##     num char    
    ##   <int> <chr>   
    ## 1     1 my      
    ## 2     2 name    
    ## 3     3 is      
    ## 4     4 courtney

This only works because they’re the same length. But we can circumvent
this by using lists instead of a tibble

``` r
l = 
  list(
    vec_numeric = 1:5,
    vec_char = LETTERS,
    matrix = matrix(1:10, nrow = 5, ncol = 2),
    summary = summary(rnorm(100))
  )
l
```

    ## $vec_numeric
    ## [1] 1 2 3 4 5
    ## 
    ## $vec_char
    ##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
    ## [20] "T" "U" "V" "W" "X" "Y" "Z"
    ## 
    ## $matrix
    ##      [,1] [,2]
    ## [1,]    1    6
    ## [2,]    2    7
    ## [3,]    3    8
    ## [4,]    4    9
    ## [5,]    5   10
    ## 
    ## $summary
    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.3804 -0.5901  0.4837  0.2452  0.9004  2.4771

Accessing lists

``` r
l$vec_char
```

    ##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
    ## [20] "T" "U" "V" "W" "X" "Y" "Z"

``` r
l[[2]]
```

    ##  [1] "A" "B" "C" "D" "E" "F" "G" "H" "I" "J" "K" "L" "M" "N" "O" "P" "Q" "R" "S"
    ## [20] "T" "U" "V" "W" "X" "Y" "Z"

``` r
l[["summary"]]
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ## -2.3804 -0.5901  0.4837  0.2452  0.9004  2.4771

## Loops

``` r
list_norm_samples = 
  list(
    a = rnorm(20, 1, 5),
    b = rnorm(20, 0, 7),
    c = rnorm(20, 20, 1),
    d = rnorm(20, -45, 13)
  )
```

``` r
mean_and_sd = function(x) {
  
  if (!is.numeric(x)) {
    
    stop("Argument should be numbers")
    
  }  else if (length(x) < 2) {
    
    stop("You need at least 2 numbers to get z scores")
    
  }
  
  mean_x = mean(x)
  
  sd_x = sd(x)
  
  tibble(
    mean = mean_x,
    sd = sd_x
  )
}
```

Could call the function for a:d, but that’s annoying.

``` r
output = vector("list", length = 4)

for (i in 1:4) {
  output[[i]] = mean_and_sd(list_norm_samples[[i]])
}

output
```

    ## [[1]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.25  4.92
    ## 
    ## [[2]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.690  9.30
    ## 
    ## [[3]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.8 0.910
    ## 
    ## [[4]]
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -44.1  14.0

## Now let’s try using a map function

``` r
output2 = map(list_norm_samples, mean_and_sd)
output2
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.25  4.92
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.690  9.30
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.8 0.910
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -44.1  14.0

## listcols in DFs

``` r
listcol_df = 
  tibble(
    name = c("a", "b", "c", "d"),
    sample = list_norm_samples
  )

listcol_df |> 
  pull(sample)
```

    ## $a
    ##  [1]  2.1196270 -4.7811167  3.1120926 -5.6237763  1.7054216 -1.6802400
    ##  [7] -0.5580304  8.7805482 -1.2401665  2.6056177 -5.1508612 -5.6202935
    ## [13]  7.3062114  7.5961586  0.5962312 -1.5254490  0.7392320  4.1443031
    ## [19] 11.9000120  0.6549135
    ## 
    ## $b
    ##  [1]  10.8140452   9.2501641   2.2550610  10.7166858  -2.9486779  -8.1117472
    ##  [7] -12.9175780   8.1012770 -14.8648492  -8.3722206  11.4953439   6.1855838
    ## [13]   3.6741312  -8.2926135  18.5905179  -7.3353960  -7.0778577   4.6824516
    ## [19]   0.9042411  -2.9580381
    ## 
    ## $c
    ##  [1] 18.85974 18.70628 19.40530 18.49919 20.01586 20.54017 18.45271 20.84965
    ##  [9] 20.89601 20.13869 18.38067 20.54840 20.19528 19.19350 19.89138 19.74905
    ## [17] 21.69935 19.65570 20.06777 19.34943
    ## 
    ## $d
    ##  [1] -51.33930 -41.05903 -48.14566 -51.26254 -57.89344 -48.64844 -36.77077
    ##  [8] -61.11764 -22.06392 -45.30784 -42.40103 -27.48649 -44.53104 -34.28045
    ## [15] -67.13473 -38.74765 -12.71385 -39.78226 -42.20270 -68.60426

``` r
mean_and_sd(listcol_df$sample[[1]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.25  4.92

``` r
mean_and_sd(listcol_df$sample[[2]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.690  9.30

``` r
mean_and_sd(listcol_df$sample[[3]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.8 0.910

``` r
mean_and_sd(listcol_df$sample[[4]])
```

    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -44.1  14.0

``` r
map(listcol_df$sample, mean_and_sd)
```

    ## $a
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  1.25  4.92
    ## 
    ## $b
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 0.690  9.30
    ## 
    ## $c
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1  19.8 0.910
    ## 
    ## $d
    ## # A tibble: 1 × 2
    ##    mean    sd
    ##   <dbl> <dbl>
    ## 1 -44.1  14.0

``` r
listcol_df |> 
  mutate(mean_sd = map(sample, mean_and_sd)) |> 
  mutate(median = map(sample, median)) |> 
  select(name, mean_sd) |> 
  unnest(mean_sd)
```

    ## # A tibble: 4 × 3
    ##   name     mean     sd
    ##   <chr>   <dbl>  <dbl>
    ## 1 a       1.25   4.92 
    ## 2 b       0.690  9.30 
    ## 3 c      19.8    0.910
    ## 4 d     -44.1   14.0

## another example with NSDUH

``` r
nsduh_url = "http://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

nsduh_html = read_html(nsduh_url)
```

Last time it looked like this:

``` r
load_nsduh_data = function(html, table_number, outcome_name) {
  html |> 
  html_table() |> 
  nth(table_number) |>
  slice(-1) |> 
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_replace(percent, "[a-c]$", ""),
    percent = as.numeric(percent),
    outcome = outcome_name) |>
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
}

load_nsduh_data(html = nsduh_html, table_number = 1, outcome_name = "marj")
```

    ## # A tibble: 510 × 5
    ##    State   age   year      percent outcome
    ##    <chr>   <chr> <chr>       <dbl> <chr>  
    ##  1 Alabama 12+   2013-2014    9.98 marj   
    ##  2 Alabama 12+   2014-2015    9.6  marj   
    ##  3 Alabama 12-17 2013-2014    9.9  marj   
    ##  4 Alabama 12-17 2014-2015    9.71 marj   
    ##  5 Alabama 18-25 2013-2014   27.0  marj   
    ##  6 Alabama 18-25 2014-2015   26.1  marj   
    ##  7 Alabama 26+   2013-2014    7.1  marj   
    ##  8 Alabama 26+   2014-2015    6.81 marj   
    ##  9 Alabama 18+   2013-2014    9.99 marj   
    ## 10 Alabama 18+   2014-2015    9.59 marj   
    ## # ℹ 500 more rows

``` r
load_nsduh_data(html = nsduh_html, table_number = 4, outcome_name = "cocaine")
```

    ## # A tibble: 510 × 5
    ##    State   age   year      percent outcome
    ##    <chr>   <chr> <chr>       <dbl> <chr>  
    ##  1 Alabama 12+   2013-2014    1.23 cocaine
    ##  2 Alabama 12+   2014-2015    1.22 cocaine
    ##  3 Alabama 12-17 2013-2014    0.42 cocaine
    ##  4 Alabama 12-17 2014-2015    0.41 cocaine
    ##  5 Alabama 18-25 2013-2014    3.09 cocaine
    ##  6 Alabama 18-25 2014-2015    3.2  cocaine
    ##  7 Alabama 26+   2013-2014    1.01 cocaine
    ##  8 Alabama 26+   2014-2015    0.99 cocaine
    ##  9 Alabama 18+   2013-2014    1.31 cocaine
    ## 10 Alabama 18+   2014-2015    1.31 cocaine
    ## # ℹ 500 more rows

First let’s try using a for loop

``` r
output = vector("list", length = 3)
input_names = list("marj", "cocaine", "heroine")
input_tables = list(1, 4, 5)

for (i in 1:3) {
  
  output[[i]] = load_nsduh_data(nsduh_html, input_tables[[i]], input_names[[i]])
    
}

nsduh_df = bind_rows(output)

nsduh_df
```

    ## # A tibble: 1,530 × 5
    ##    State   age   year      percent outcome
    ##    <chr>   <chr> <chr>       <dbl> <chr>  
    ##  1 Alabama 12+   2013-2014    9.98 marj   
    ##  2 Alabama 12+   2014-2015    9.6  marj   
    ##  3 Alabama 12-17 2013-2014    9.9  marj   
    ##  4 Alabama 12-17 2014-2015    9.71 marj   
    ##  5 Alabama 18-25 2013-2014   27.0  marj   
    ##  6 Alabama 18-25 2014-2015   26.1  marj   
    ##  7 Alabama 26+   2013-2014    7.1  marj   
    ##  8 Alabama 26+   2014-2015    6.81 marj   
    ##  9 Alabama 18+   2013-2014    9.99 marj   
    ## 10 Alabama 18+   2014-2015    9.59 marj   
    ## # ℹ 1,520 more rows

Try again, using maps!

FIrst, update the load data function to accomoadate an input tibble that
has both the name of the table and the position of the table.

``` r
load_nsduh_data = function(html, table_number) {
  html |> 
  html_table() |> 
  nth(table_number) |>
  slice(-1) |> 
  select(-contains("P Value")) |>
  pivot_longer(
    -State,
    names_to = "age_year", 
    values_to = "percent") |>
  separate(age_year, into = c("age", "year"), sep = "\\(") |>
  mutate(
    year = str_replace(year, "\\)", ""),
    percent = str_replace(percent, "[a-c]$", ""),
    percent = as.numeric(percent)) |>
  filter(!(State %in% c("Total U.S.", "Northeast", "Midwest", "South", "West")))
}


nsduh_df = 
  tibble(
    name = c("marj", "cocaine", "heroin"),
    number = c(1, 4, 5)
  ) |> 
  mutate(table = map(number, load_nsduh_data, html = nsduh_html)) |> 
  filter(name == "marj") |> 
  unnest(table)

nsduh_df
```

    ## # A tibble: 510 × 6
    ##    name  number State   age   year      percent
    ##    <chr>  <dbl> <chr>   <chr> <chr>       <dbl>
    ##  1 marj       1 Alabama 12+   2013-2014    9.98
    ##  2 marj       1 Alabama 12+   2014-2015    9.6 
    ##  3 marj       1 Alabama 12-17 2013-2014    9.9 
    ##  4 marj       1 Alabama 12-17 2014-2015    9.71
    ##  5 marj       1 Alabama 18-25 2013-2014   27.0 
    ##  6 marj       1 Alabama 18-25 2014-2015   26.1 
    ##  7 marj       1 Alabama 26+   2013-2014    7.1 
    ##  8 marj       1 Alabama 26+   2014-2015    6.81
    ##  9 marj       1 Alabama 18+   2013-2014    9.99
    ## 10 marj       1 Alabama 18+   2014-2015    9.59
    ## # ℹ 500 more rows

## Operations in nested data

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USW00022534 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## Registered S3 method overwritten by 'hoardr':
    ##   method           from
    ##   print.cache_info httr

    ## using cached file: /Users/cjd2195/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-10-05 09:29:35.021583 (8.527)

    ## file min/max dates: 1869-01-01 / 2023-10-31

    ## using cached file: /Users/cjd2195/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-10-05 09:29:43.72746 (3.832)

    ## file min/max dates: 1949-10-01 / 2023-10-31

    ## using cached file: /Users/cjd2195/Library/Caches/org.R-project.R/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-10-05 09:29:46.685904 (0.997)

    ## file min/max dates: 1999-09-01 / 2023-10-31

``` r
weather_nest_df = 
  weather_df |> 
  nest(df = date:tmin)
```

Can I regress tmax on tmin for each of these?

``` r
central_park_df = 
  weather_nest_df |> 
  pull(df) |> 
  nth(1)
```

Let’s fit a linear regression for central park

``` r
lm(tmax ~ tmin, data = central_park_df)
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = central_park_df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

Cool. Now let’s do it for different dataframes!

``` r
weather_lm = function(df) {
  lm(tmax ~ tmin, data = df)
}

weather_lm(central_park_df)
```

    ## 
    ## Call:
    ## lm(formula = tmax ~ tmin, data = df)
    ## 
    ## Coefficients:
    ## (Intercept)         tmin  
    ##       7.514        1.034

Let’s do a for loop:

``` r
input_list = weather_nest_df |> pull(df)
output = vector("list", length = 3)

for (i in 1:3) {
  output[[i]] = weather_lm(input_list[[i]])
}

weather_nest_df |> 
  mutate(models = map(df, weather_lm)) |> 
  unnest(df)
```

    ## # A tibble: 2,190 × 7
    ##    name           id          date        prcp  tmax  tmin models
    ##    <chr>          <chr>       <date>     <dbl> <dbl> <dbl> <list>
    ##  1 CentralPark_NY USW00094728 2021-01-01   157   4.4   0.6 <lm>  
    ##  2 CentralPark_NY USW00094728 2021-01-02    13  10.6   2.2 <lm>  
    ##  3 CentralPark_NY USW00094728 2021-01-03    56   3.3   1.1 <lm>  
    ##  4 CentralPark_NY USW00094728 2021-01-04     5   6.1   1.7 <lm>  
    ##  5 CentralPark_NY USW00094728 2021-01-05     0   5.6   2.2 <lm>  
    ##  6 CentralPark_NY USW00094728 2021-01-06     0   5     1.1 <lm>  
    ##  7 CentralPark_NY USW00094728 2021-01-07     0   5    -1   <lm>  
    ##  8 CentralPark_NY USW00094728 2021-01-08     0   2.8  -2.7 <lm>  
    ##  9 CentralPark_NY USW00094728 2021-01-09     0   2.8  -4.3 <lm>  
    ## 10 CentralPark_NY USW00094728 2021-01-10     0   5    -1.6 <lm>  
    ## # ℹ 2,180 more rows
