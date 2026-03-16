# Class 8: Breast Cancer Mini Project
Matthew Huang (A17978767)
2026-01-29

## Background

Today we will employ all R techniques for data analysis we used so far
including machine learning methods of clustering and PCA to analyze an
actual breast cancer biopsy data.

## Data Import

The data is in CSV format:

``` r
wiscancer <- read.csv("WisconsinCancer.csv", row.names=1)

head(wiscancer,3)
```

             diagnosis radius_mean texture_mean perimeter_mean area_mean
    842302           M       17.99        10.38          122.8      1001
    842517           M       20.57        17.77          132.9      1326
    84300903         M       19.69        21.25          130.0      1203
             smoothness_mean compactness_mean concavity_mean concave.points_mean
    842302           0.11840          0.27760         0.3001             0.14710
    842517           0.08474          0.07864         0.0869             0.07017
    84300903         0.10960          0.15990         0.1974             0.12790
             symmetry_mean fractal_dimension_mean radius_se texture_se perimeter_se
    842302          0.2419                0.07871    1.0950     0.9053        8.589
    842517          0.1812                0.05667    0.5435     0.7339        3.398
    84300903        0.2069                0.05999    0.7456     0.7869        4.585
             area_se smoothness_se compactness_se concavity_se concave.points_se
    842302    153.40      0.006399        0.04904      0.05373           0.01587
    842517     74.08      0.005225        0.01308      0.01860           0.01340
    84300903   94.03      0.006150        0.04006      0.03832           0.02058
             symmetry_se fractal_dimension_se radius_worst texture_worst
    842302       0.03003             0.006193        25.38         17.33
    842517       0.01389             0.003532        24.99         23.41
    84300903     0.02250             0.004571        23.57         25.53
             perimeter_worst area_worst smoothness_worst compactness_worst
    842302             184.6       2019           0.1622            0.6656
    842517             158.8       1956           0.1238            0.1866
    84300903           152.5       1709           0.1444            0.4245
             concavity_worst concave.points_worst symmetry_worst
    842302            0.7119               0.2654         0.4601
    842517            0.2416               0.1860         0.2750
    84300903          0.4504               0.2430         0.3613
             fractal_dimension_worst
    842302                   0.11890
    842517                   0.08902
    84300903                 0.08758

> Q1 How many observations are in this dataset?

``` r
nrow(wiscancer)
```

    [1] 569

> Q2 How many of the observations have a malignant diagnoss?

``` r
sum(wiscancer$diagnosis == "M")
```

    [1] 212

``` r
table(wiscancer$diagnosis)
```


      B   M 
    357 212 

> Q3 How many variables/ features in the data are suffixed with \_mean

``` r
colnames(wiscancer)
```

     [1] "diagnosis"               "radius_mean"            
     [3] "texture_mean"            "perimeter_mean"         
     [5] "area_mean"               "smoothness_mean"        
     [7] "compactness_mean"        "concavity_mean"         
     [9] "concave.points_mean"     "symmetry_mean"          
    [11] "fractal_dimension_mean"  "radius_se"              
    [13] "texture_se"              "perimeter_se"           
    [15] "area_se"                 "smoothness_se"          
    [17] "compactness_se"          "concavity_se"           
    [19] "concave.points_se"       "symmetry_se"            
    [21] "fractal_dimension_se"    "radius_worst"           
    [23] "texture_worst"           "perimeter_worst"        
    [25] "area_worst"              "smoothness_worst"       
    [27] "compactness_worst"       "concavity_worst"        
    [29] "concave.points_worst"    "symmetry_worst"         
    [31] "fractal_dimension_worst"

``` r
length(grep("_mean",colnames(wiscancer)))
```

    [1] 10

We need to remove the `diagnosis` column before doing any more analysis
of this dataset since we don’t want to pass this to PCA etc. Save it as
a separate vector that we can later use to compare findings to those of
experts.

``` r
#saving diagnosis
diagnosis <- wiscancer [,1] 
#dataframe omitting first column
wisc <- wiscancer[,-1]  
```

## Principal Component Analysis (PCA)

The primary function in base R for performing PCA is `prcomp()`. We use
the optional argument `scale = TRUE` because the variables in the
dataset are measured on different scales.

``` r
wisc.pr <- prcomp(wisc,scale=T)
```

> Q4. From your results, what proportion of the original variance is
> captured by the first principal component (PC1)?

0.4427

> Q5. How many principal components (PCs) are required to describe at
> least 70% of the original variance in the data?

3

> Q6. How many principal components (PCs) are required to describe at
> least 90% of the original variance in the data?

7

## Interpreting PCA Plots

``` r
biplot(wisc.pr)
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-7-1.png)

> Q7. What stands out to you about this plot? Is it easy or difficult to
> understand? Why?

It’s not a good plot, since not much information is discernable. It is
hard to understand due to there just being one large clump of data in
the middle, making it too crowded with insufficient scaling to observe
correlations visually.

## Scatter plot observations by components 1 and 2

``` r
library(ggplot2)
ggplot(wisc.pr$x) +
  aes(PC1, PC2, col=diagnosis) +
  geom_point()
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-8-1.png)

``` r
summary(wisc.pr)
```

    Importance of components:
                              PC1    PC2     PC3     PC4     PC5     PC6     PC7
    Standard deviation     3.6444 2.3857 1.67867 1.40735 1.28403 1.09880 0.82172
    Proportion of Variance 0.4427 0.1897 0.09393 0.06602 0.05496 0.04025 0.02251
    Cumulative Proportion  0.4427 0.6324 0.72636 0.79239 0.84734 0.88759 0.91010
                               PC8    PC9    PC10   PC11    PC12    PC13    PC14
    Standard deviation     0.69037 0.6457 0.59219 0.5421 0.51104 0.49128 0.39624
    Proportion of Variance 0.01589 0.0139 0.01169 0.0098 0.00871 0.00805 0.00523
    Cumulative Proportion  0.92598 0.9399 0.95157 0.9614 0.97007 0.97812 0.98335
                              PC15    PC16    PC17    PC18    PC19    PC20   PC21
    Standard deviation     0.30681 0.28260 0.24372 0.22939 0.22244 0.17652 0.1731
    Proportion of Variance 0.00314 0.00266 0.00198 0.00175 0.00165 0.00104 0.0010
    Cumulative Proportion  0.98649 0.98915 0.99113 0.99288 0.99453 0.99557 0.9966
                              PC22    PC23   PC24    PC25    PC26    PC27    PC28
    Standard deviation     0.16565 0.15602 0.1344 0.12442 0.09043 0.08307 0.03987
    Proportion of Variance 0.00091 0.00081 0.0006 0.00052 0.00027 0.00023 0.00005
    Cumulative Proportion  0.99749 0.99830 0.9989 0.99942 0.99969 0.99992 0.99997
                              PC29    PC30
    Standard deviation     0.02736 0.01153
    Proportion of Variance 0.00002 0.00000
    Cumulative Proportion  1.00000 1.00000

> Q8. Generate a similar plot for principal components 1 and 3. What do
> you notice about these plots?

The PC1 vs PC2 plot shows clearer separation between malignant and
benign samples than PC1 vs PC3

``` r
ggplot(wisc.pr$x) +
  aes(PC1, PC3, col=diagnosis) +
  geom_point()
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-9-1.png)

# Calculate variance of components.

``` r
pr.var <- wisc.pr$sdev^2
head(pr.var)
```

    [1] 13.281608  5.691355  2.817949  1.980640  1.648731  1.207357

# Variance explained by each principal component: pve

``` r
pve <- pr.var/ sum(pr.var)
```

# Plot variance explained for each principal component

``` r
plot(c(1,pve), xlab = "Principal Component", 
     ylab = "Proportion of Variance Explained", 
     ylim = c(0, 1), type = "o")
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-12-1.png)

# Alternative scree plot of same data, data driven y-axis

``` r
barplot(pve, ylab = "Explanation For Percent of Variance",
     names.arg=paste0("PC",1:length(pve)), las=2, axes = FALSE)
axis(2, at=pve, labels=round(pve,2)*100 )
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-13-1.png)

## ggplot based graph

``` r
library(factoextra)
```

    Welcome to factoextra!

    Want to learn more? See two factoextra-related books at https://www.datanovia.com/en/product/practical-guide-to-principal-component-methods-in-r/

``` r
fviz_eig(wisc.pr, addlabels = TRUE)
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-14-1.png)

## Communicating PCA Results

> Q9. For the first principal component, what is the component of the
> loading vector (i.e. wisc.pr\$rotation\[,1\]) for the feature
> concave.points_mean? This tells us how much this original feature
> contributes to the first PC. Are there any features with larger
> contributions than this one?

``` r
pc1 <- wisc.pr$rotation [, "PC1"]
wisc.pr$rotation["concave.points_mean",1]
```

    [1] -0.2608538

``` r
min(pc1)
```

    [1] -0.2608538

The component of the loading vector for `concave.points_mean` on PC1 is
−0.261. This feature has one of the largest absolute loadings on PC1,
indicating that it is a major contributor to this principal component.

## Hierarchial clustering

# Scale the wisc.data data using the “scale()” function

``` r
wisc.data <- wisc
data.scaled <- scale(wisc.data)
data.dist <- dist(data.scaled)
wisc.hclust <- hclust(data.dist)
plot(wisc.hclust)
abline(h=20, col="pink", lty=2)
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-16-1.png)

> Q10. Using plot() and abline() functions, what is the height at which
> the clustering model has 4 clusters?

~20

## Combining methods

The idea is to use the new variables like the principal component scores
stored in `wisc.pr$x` to provide a more concise and informative
representation of the dataset than the original features, which were the
30 variables in `wisc.data`, as the basis for clustering analysis.

``` r
pc.dist <- dist(wisc.pr$x[, 1:3])
wisc.pr.hclust <- hclust(pc.dist, method = "ward.D2")
plot(wisc.pr.hclust)
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-17-1.png)

> Q12. Which method gives your favorite results for the same data.dist
> dataset? Explain your reasoning.

I prefer the ward d2 method, as it looks distributed the best. It is
more even, while the single method looks too convoluted, the average
method has weird clumps, and the complete method looks way too spread
apart.

``` r
grps <- cutree(wisc.pr.hclust, k=2)
table(grps)
```

    grps
      1   2 
    203 366 

``` r
table(grps, diagnosis)
```

        diagnosis
    grps   B   M
       1  24 179
       2 333  33

``` r
wisc.hclust.clusters <- cutree(wisc.hclust, k=4)
table(wisc.hclust.clusters, diagnosis)
```

                        diagnosis
    wisc.hclust.clusters   B   M
                       1  12 165
                       2   2   5
                       3 343  40
                       4   0   2

``` r
ggplot(wisc.pr$x) +
  aes(PC1, PC2) +
  geom_point(col=grps)
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-19-1.png)

Now run `table()` with both clustering `grps` and the expert `diagnosis`

``` r
table(grps, diagnosis)
```

        diagnosis
    grps   B   M
       1  24 179
       2 333  33

> Q14. How well do the hierarchical clustering models you created in the
> previous sections (i.e. without first doing PCA) do in terms of
> separating the diagnoses? Again, use the table() function to compare
> the output of each model (wisc.hclust.clusters and
> wisc.pr.hclust.clusters) with the vector containing the actual
> diagnoses.

Cluster “1” has 179 “M” Diagnosis Cluster “2” has 333 “B” Diagnosis

179 TP 24 FP

333 TN 33 FN

Sensitivity: TP / (TP+FN)

``` r
179/(179+33)
```

    [1] 0.8443396

Specificity: TN / (TN+FP)

``` r
333/(333+24)
```

    [1] 0.9327731

## Prediction

We can use our PCA model for prediction of

``` r
url <- "https://tinyurl.com/new-samples-CSV"
new <- read.csv(url)
npc <- predict(wisc.pr, newdata=new)
npc
```

               PC1       PC2        PC3        PC4       PC5        PC6        PC7
    [1,]  2.576616 -3.135913  1.3990492 -0.7631950  2.781648 -0.8150185 -0.3959098
    [2,] -4.754928 -3.009033 -0.1660946 -0.6052952 -1.140698 -1.2189945  0.8193031
                PC8       PC9       PC10      PC11      PC12      PC13     PC14
    [1,] -0.2307350 0.1029569 -0.9272861 0.3411457  0.375921 0.1610764 1.187882
    [2,] -0.3307423 0.5281896 -0.4855301 0.7173233 -1.185917 0.5893856 0.303029
              PC15       PC16        PC17        PC18        PC19       PC20
    [1,] 0.3216974 -0.1743616 -0.07875393 -0.11207028 -0.08802955 -0.2495216
    [2,] 0.1299153  0.1448061 -0.40509706  0.06565549  0.25591230 -0.4289500
               PC21       PC22       PC23       PC24        PC25         PC26
    [1,]  0.1228233 0.09358453 0.08347651  0.1223396  0.02124121  0.078884581
    [2,] -0.1224776 0.01732146 0.06316631 -0.2338618 -0.20755948 -0.009833238
                 PC27        PC28         PC29         PC30
    [1,]  0.220199544 -0.02946023 -0.015620933  0.005269029
    [2,] -0.001134152  0.09638361  0.002795349 -0.019015820

``` r
plot(wisc.pr$x[,1:2], col=grps)
points(npc[,1], npc[,2], col="green", pch=16, cex=3)
text(npc[,1], npc[,2], c(1,2), col="blue")
```

![](Class8MH_files/figure-commonmark/unnamed-chunk-23-1.png)

> Q16 Which of these new patients should we prioritize for follow up
> based on your results?

Patient 1 should be prioritized for a follow up, as their projected PCA
scores place them closer to the malignant cluster.
