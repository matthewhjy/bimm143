---
layout: page
title: "Class 12"
permalink: /Class12/
---

Matthew Huang (A17978767)

``` r
dat <- read.table("rs8067378_ENSG00000172057.6.csv.txt",
                  header = TRUE, stringsAsFactors = FALSE)
head(dat)
```

       sample geno      exp
    1 HG00367  A/G 28.96038
    2 NA20768  A/G 20.24449
    3 HG00361  A/A 31.32628
    4 HG00135  A/A 34.11169
    5 NA18870  G/G 18.25141
    6 NA11993  A/A 32.89721

## Q13: Sample size per genotype + median expression

# Sample size per genotype

``` r
n_per_geno <- table(dat$geno)
n_per_geno
```


    A/A A/G G/G 
    108 233 121 

# Median expression per genotype

``` r
median_per_geno <- tapply(dat$exp, dat$geno, median, na.rm = TRUE)
median_per_geno
```

         A/A      A/G      G/G 
    31.24847 25.06486 20.07363 

## Q14: make a boxplot (one box per genotype)

``` r
boxplot(exp ~ geno, data = dat,
        xlab = "Genotype (rs8067378)",
        ylab = "ORMDL3 expression",
        main = "ORMDL3 expression by rs8067378 genotype")


bp_obj <- boxplot(exp ~ geno, data = dat, plot = FALSE)
bp_medians <- bp_obj$stats[3, ]   

text(x = 1:length(bp_medians),
     y = bp_medians,
     labels = round(bp_medians, 2),
     pos = 3)
```

![](Class12_files/figure-commonmark/unnamed-chunk-4-1.png)
