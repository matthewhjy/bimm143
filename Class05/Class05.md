# Lab 5: Data Visualization with ggplot
Matthew Huang (A17978767)

## Background

R has multiple plotting systems. This is a brief comparison of base R
graphics and **ggplot2** using the built-in `cars` dataset.

``` r
head(cars)
```

      speed dist
    1     4    2
    2     4   10
    3     7    4
    4     7   22
    5     8   16
    6     9   10

Base R

``` r
plot(cars)
```

![](Class05_files/figure-commonmark/unnamed-chunk-2-1.png)

ggplot2

Install once in the Console: install.packages(“ggplot2”) Load each
session with library():

``` r
library(ggplot2)
ggplot(cars)
```

![](Class05_files/figure-commonmark/unnamed-chunk-3-1.png)

A ggplot typically includes: 1. data 2. aesthetics (aes()) 3. one or
more geoms

``` r
ggplot(cars) +
  aes(x = speed, y = dist) +
  geom_point(alpha = 0.7, size = 2) +
  geom_smooth(method = "lm", se = FALSE) +
  labs(
    title = "Speed and Stopping Distance",
    x = "Speed (mph)",
    y = "Stopping distance (ft)"
  ) +
  theme_bw()
```

    `geom_smooth()` using formula = 'y ~ x'

![](Class05_files/figure-commonmark/unnamed-chunk-4-1.png)

## Gene Expression Plot

Load gene expression data comparing control and drug-treated samples:

``` r
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt"
genes <- read.delim(url)
head(genes)
```

            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging

Simple scatter plot

``` r
ggplot(genes) +
  aes(x = Condition1, y = Condition2) +
  geom_point(color = "navy", alpha = 0.25, size = 1.5) +
  labs(
    title = "Expression Levels: Control vs Drug",
    x = "Condition 1 (untreated)",
    y = "Condition 2 (treated)"
  ) +
  theme_minimal()
```

![](Class05_files/figure-commonmark/unnamed-chunk-6-1.png)

Check counts for each expression state:

``` r
table(genes$State)
```


          down unchanging         up 
            72       4997        127 

Color points by expression state:

``` r
ggplot(genes) +
  aes(x = Condition1, y = Condition2, col = State) +
  geom_point(alpha = 0.6, size = 1.7) +
  scale_color_manual(values = c("purple", "magenta", "pink")) +
  labs(
    title = "Gene Expression Changes After Drug Treatment",
    x = "Control (no drug)",
    y = "Drug treatment"
  ) +
  theme_bw()
```

![](Class05_files/figure-commonmark/unnamed-chunk-8-1.png)

## Gapminder exploration

Exploring the gapminder dataset:

``` r
url <- "https://raw.githubusercontent.com/jennybc/gapminder/master/inst/extdata/gapminder.tsv"
gapminder <- read.delim(url)
head(gapminder)
```

          country continent year lifeExp      pop gdpPercap
    1 Afghanistan      Asia 1952  28.801  8425333  779.4453
    2 Afghanistan      Asia 1957  30.332  9240934  820.8530
    3 Afghanistan      Asia 1962  31.997 10267083  853.1007
    4 Afghanistan      Asia 1967  34.020 11537966  836.1971
    5 Afghanistan      Asia 1972  36.088 13079460  739.9811
    6 Afghanistan      Asia 1977  38.438 14880372  786.1134

How many rows are in the dataset?

``` r
nrow(gapminder)
```

    [1] 1704

How many continents are represented?

``` r
table(gapminder$continent)
```


      Africa Americas     Asia   Europe  Oceania 
         624      300      396      360       24 

GDP per capita vs life expectancy

``` r
ggplot(gapminder) +
  aes(x = gdpPercap, y = lifeExp, col = continent) +
  geom_point(alpha = 0.6, size = 1.7) +
  labs(
    title = "Life Expectancy vs GDP per Capita",
    x = "GDP per capita",
    y = "Life expectancy (years)"
  ) +
  theme_bw()
```

![](Class05_files/figure-commonmark/unnamed-chunk-12-1.png)

Facet by continent:

``` r
ggplot(gapminder) +
  aes(x = gdpPercap, y = lifeExp, col = continent) +
  geom_point(alpha = 0.6, size = 1.7) +
  facet_wrap(~ continent) +
  labs(
    title = "Life Expectancy vs GDP per Capita by Continent",
    x = "GDP per capita",
    y = "Life expectancy (years)"
  ) +
  theme_bw()
```

![](Class05_files/figure-commonmark/unnamed-chunk-13-1.png)

## First look at dplyr

The dplyr package provides functions for data manipulation, including
filter().

``` r
library(dplyr)
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

Example: Ireland in 2007

``` r
filter(gapminder, year == 2007, country == "Ireland")
```

      country continent year lifeExp     pop gdpPercap
    1 Ireland    Europe 2007  78.885 4109086     40676

Compare two years using faceting:

``` r
input <- filter(gapminder, year == 2007 | year == 1977)

ggplot(input) +
  aes(x = gdpPercap, y = lifeExp, col = continent) +
  geom_point(alpha = 0.65, size = 1.9) +
  facet_wrap(~ year) +
  labs(
    title = "GDP and Life Expectancy in 1977 vs 2007",
    x = "GDP per capita",
    y = "Life expectancy (years)"
  ) +
  theme_bw()
```

![](Class05_files/figure-commonmark/unnamed-chunk-16-1.png)
