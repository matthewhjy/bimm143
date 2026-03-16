# Class 7: Machine Learning 1
Matthew Huang (PID: A17978767)
2026-01-27

# Background

Today we begin our exploration of important machine learning methods,
focusing on **clustering** and **dimensional reduction.**

To start testing these methods, make up some sample data to cluster,
where we know what the answer should be.

``` r
hist( rnorm(3000, mean = 10) )
```

![](Class7_files/figure-commonmark/unnamed-chunk-1-1.png)

> Q. Can you generate 30 numbers centered at +3 and 30 @ -3 taken at
> random from a normal dist?

``` r
tmp <- c(rnorm(30, mean = 3),
         rnorm(30, mean = -3) )

x <- cbind(x = tmp, y = rev(tmp))
plot(x)
```

![](Class7_files/figure-commonmark/unnamed-chunk-2-1.png)

## K means clustering

The main function in “base R” for K-means clustering is called
`kmeans()`, lets try it

``` r
k <- kmeans(x, centers = 2)
```

> Q. What component of your kmeans result object has the cluster
> centers?

``` r
k$centers
```

              x         y
    1 -3.046730  3.083274
    2  3.083274 -3.046730

> Q. What component of your kmeans result has the cluster size? (how
> many points in cluster)

``` r
k$size
```

    [1] 30 30

> Q. What component of your kmeans result has the cluster membership
> vector? (which points are in cluster)

``` r
k$cluster
```

     [1] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 1 1 1 1 1 1 1 1
    [39] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

> Q. Plot the results of clustering (data colored by the clustering
> result)

``` r
plot(x, col = (k$cluster))
points(k$centers, col = "magenta", pch = 15, cex = 1.5)
```

![](Class7_files/figure-commonmark/unnamed-chunk-7-1.png)

> Q. Run ‘kmeans()’ again and cluster into 4 groups, plot results like
> above with coloring by cluster and cluster centers

``` r
k4 <- kmeans(x, centers = 4)
plot(x, col = k4$cluster)
points(k4$centers, col = "magenta", pch = 15, cex = 1.5)
```

![](Class7_files/figure-commonmark/unnamed-chunk-8-1.png)

> **Key point:** Kmenas will always return the clustering that we ask
> for (this is the “K” or “centers” in Kmeans).

``` r
k$tot.withinss
```

    [1] 92.4122

# Hierarchical Clustering

The main function for Hierarchical clustering in base R is called
`hclust()`. One of the main differences with respect to the `kmeans()`
function is that you can’t just pass your input data directly to
`hclust()` as it needs a “distance matrix.We can get this from lots of
places including the `dist()` function. `hclust()` more flexible.

``` r
d <- dist(x)
hc <- hclust(d)
plot(hc)
```

![](Class7_files/figure-commonmark/unnamed-chunk-10-1.png)

We can “cut the dendrogram or”tree” at a given height to yield our
“clusters”, for this we use `cutree()`

``` r
plot(hc)
abline(h = 7, col="red")
```

![](Class7_files/figure-commonmark/unnamed-chunk-11-1.png)

``` r
grps <- cutree(hc, h = 7)
```

``` r
grps
```

     [1] 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 2 2 2 2 2 2 2
    [39] 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2 2

> Q. Plot our data `x` colored by the clusterinv result from `hclust()`?

``` r
plot(x, col = grps)
```

![](Class7_files/figure-commonmark/unnamed-chunk-13-1.png)

# Principal Component Analysis (PCA)

PCA is a popular dimentionality reduction technique widely used in
bioinformatics.

## PCA of UK food data

Read data on food consumption in the UK

``` r
url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url)
x
```

                         X England Wales Scotland N.Ireland
    1               Cheese     105   103      103        66
    2        Carcass_meat      245   227      242       267
    3          Other_meat      685   803      750       586
    4                 Fish     147   160      122        93
    5       Fats_and_oils      193   235      184       209
    6               Sugars     156   175      147       139
    7      Fresh_potatoes      720   874      566      1033
    8           Fresh_Veg      253   265      171       143
    9           Other_Veg      488   570      418       355
    10 Processed_potatoes      198   203      220       187
    11      Processed_Veg      360   365      337       334
    12        Fresh_fruit     1102  1137      957       674
    13            Cereals     1472  1582     1462      1494
    14           Beverages      57    73       53        47
    15        Soft_drinks     1374  1256     1572      1506
    16   Alcoholic_drinks      375   475      458       135
    17      Confectionery       54    64       62        41

Looks like row names aren’t set properly, let’s fix this

``` r
rownames(x) <- x[,1]
x <- x[,-1]
x
```

                        England Wales Scotland N.Ireland
    Cheese                  105   103      103        66
    Carcass_meat            245   227      242       267
    Other_meat              685   803      750       586
    Fish                    147   160      122        93
    Fats_and_oils           193   235      184       209
    Sugars                  156   175      147       139
    Fresh_potatoes          720   874      566      1033
    Fresh_Veg               253   265      171       143
    Other_Veg               488   570      418       355
    Processed_potatoes      198   203      220       187
    Processed_Veg           360   365      337       334
    Fresh_fruit            1102  1137      957       674
    Cereals                1472  1582     1462      1494
    Beverages                57    73       53        47
    Soft_drinks            1374  1256     1572      1506
    Alcoholic_drinks        375   475      458       135
    Confectionery            54    64       62        41

A better way to do this is to fix the row name assignment at import
time.

``` r
x <- read.csv(url, row.names = 1)
```

> Q1. How many rows and columns are in your new data frame named x? What
> R functions could you use to answer this questions?

``` r
x <- dim(x)
x
```

    [1] 17  4

> Q2. Which approach to solving the ‘row-names problem’ mentioned above
> do you prefer and why? Is one approach more robust than another under
> certain circumstances?

I would rather fix the row name assignment at import time, as it appears
to be more flexible for usage, and also less complex/convoluted

> Q3. Changing what optional argument in the above barplot() function
> results in the following plot?

The stacked barplot is produced by setting the optional argument beside
= FALSE in the barplot() function. When beside = FALSE, the bars are
stacked rather than displayed side by side.

> Q4. Changing what optional argument in the above ggplot() code results
> in a stacked barplot figure?

The stacked barplot is made by changing the position argument in
geom_col() from dodge to stack, since stacking is the default behavior,
removing the position argument also results in a stacked barplot.

> Q5. We can use the pairs() function to generate all pairwise plots for
> our countries. Can you make sense of the following code and resulting
> figure? What does it mean if a given point lies on the diagonal for a
> given plot?

The points show that if the countries had similar values of the food
categories, the points would lie on the diagonal, if the two countries
were being compared have different values for certain food categories
then the point will not be on the diagonal.

> Q6. Based on the pairs and heatmap figures, which countries cluster
> together and what does this suggest about their food consumption
> patterns? Can you easily tell what the main differences between N.
> Ireland and the other countries of the UK in terms of this data-set?

### Heatmap

We can install **pheatmap** package with the `install.packages()`
command we used before. Make sure to run in consol and not code chunk in
quarto.

``` r
library(pheatmap)

url <- "https://tinyurl.com/UK-foods"
x <- read.csv(url, row.names = 1)

pheatmap(as.matrix(x))
```

![](Class7_files/figure-commonmark/unnamed-chunk-18-1.png)

``` r
library(pheatmap)
pheatmap( as.matrix(x) )
```

![](Class7_files/figure-commonmark/unnamed-chunk-18-2.png)

Of all these plots ontly the `pairs()` plot was really useful, but this
took a bit of work to interpret and will ot scale when I am looking at
much bigger databases.

## PCA the rescue

The main function in “base R” for PCA is called `prcomp()`

``` r
pca <- prcomp( t(x) )
summary(pca)
```

    Importance of components:
                                PC1      PC2      PC3     PC4
    Standard deviation     324.1502 212.7478 73.87622 2.7e-14
    Proportion of Variance   0.6744   0.2905  0.03503 0.0e+00
    Cumulative Proportion    0.6744   0.9650  1.00000 1.0e+00

> Q. How much variance is captured in the first PC?

67.44%

> Q. How many PCs do I need to capture at least 90% of the total
> variance in the dataset?

2 PCs, that will capture 96.5% of the total variance

> Q. Plot our main PCA results, people can call this different things
> depending on their field of study, like PC plot, ordienation plot,
> score plot, PC1 vs PC2 plot, etc…

``` r
attributes(pca)
```

    $names
    [1] "sdev"     "rotation" "center"   "scale"    "x"       

    $class
    [1] "prcomp"

To generate our PCA score plot, we want the `pca$x` component of the
result object

``` r
pca$x
```

                     PC1         PC2        PC3           PC4
    England   -144.99315   -2.532999 105.768945  1.612425e-14
    Wales     -240.52915 -224.646925 -56.475555  4.751043e-13
    Scotland   -91.86934  286.081786 -44.415495 -6.044349e-13
    N.Ireland  477.39164  -58.901862  -4.877895  1.145386e-13

``` r
my_cols <- c("orange", "red", "blue", "darkgreen")
plot(pca$x[,1], pca$x[,2], col=my_cols, pch = 16)
```

![](Class7_files/figure-commonmark/unnamed-chunk-22-1.png)

``` r
library(ggplot2)

ggplot(pca$x) + 
  aes(PC1, PC2) +
  geom_point(cols = my_cols)
```

    Warning in geom_point(cols = my_cols): Ignoring unknown parameters: `cols`

![](Class7_files/figure-commonmark/unnamed-chunk-23-1.png)

> Q7. Complete the code below to generate a plot of PC1 vs PC2. The
> second line adds text labels over the data points.

``` r
# Create a data frame for plotting
df <- as.data.frame(pca$x)
df$Country <- rownames(df)

# Plot PC1 vs PC2 with ggplot
ggplot(pca$x) +
  aes(x = PC1, y = PC2, label = rownames(pca$x)) +
  geom_point(size = 3) +
  geom_text(vjust = -0.5) +
  xlim(-270, 500) +
  xlab("PC1") +
  ylab("PC2") +
  theme_bw()
```

![](Class7_files/figure-commonmark/unnamed-chunk-24-1.png)

> Q8. Customize your plot so that the colors of the country names match
> the colors in our UK and Ireland map and table at start of this
> document.

``` r
df <- as.data.frame(pca$x)
df$Country <- rownames(df)

ggplot(pca$x) +
  aes(x = PC1, y = PC2, label = rownames(pca$x)) +
  geom_point(size = 3) +
  geom_text(vjust = -0.5) +
  xlim(-270, 500) +
  xlab("PC1") +
  ylab("PC2") +
  theme_bw()
```

![](Class7_files/figure-commonmark/unnamed-chunk-25-1.png)

## Digging Deeper: Variable Loadings

How do the original variables (i.e. the 17 differend foods) contribute
to our new PCs

``` r
ggplot(pca$rotation) +
  aes(x = PC1, 
      y = reorder(rownames(pca$rotation), PC1)) +
  geom_col(fill = "steelblue") +
  xlab("PC1 Loading Score") +
  ylab("") +
  theme_bw() +
  theme(axis.text.y = element_text(size = 9))
```

![](Class7_files/figure-commonmark/unnamed-chunk-26-1.png)

> Q9. Generate a similar ‘loadings plot’ for PC2. What two food groups
> feature prominantely and what does PC2 maninly tell us about?

``` r
ggplot(pca$rotation) +
  aes(x = PC2, 
      y = reorder(rownames(pca$rotation), PC2)) +
  geom_col(fill = "steelblue") +
  xlab("PC2 Loading Score") +
  ylab("") +
  theme_bw() +
  theme(axis.text.y = element_text(size = 9))
```

![](Class7_files/figure-commonmark/unnamed-chunk-27-1.png)
