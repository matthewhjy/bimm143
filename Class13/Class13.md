# Class 13: Transcriptomics and the analysis of RNA-Seq data
Matthew Huang (A17978767)

- [Background](#background)
- [Data Import](#data-import)
- [Check metadata counts
  correspondance](#check-metadata-counts-correspondance)
- [Analysis Plan](#analysis-plan)
- [Log 2 units and fold change](#log-2-units-and-fold-change)
- [Remove zero count genes](#remove-zero-count-genes)
- [DESeq analysis](#deseq-analysis)
- [Volcano plot](#volcano-plot)
- [Add some plot annotation](#add-some-plot-annotation)
- [Save our results to a CSV file](#save-our-results-to-a-csv-file)
- [Add Annotation Data](#add-annotation-data)
- [Pathway analysis](#pathway-analysis)

## Background

We will perform an RNAseq analysis on the effects of dexamethasone
(“dex”), a common steroid on airway smooth muscle (ASM) cell lines.

## Data Import

We need two things for this analysis.

- **countData**: a table with genes as rows and samples/experiments as
  columns
- **colData**: metadata about the columns( ie samples) in the main
  countData object

``` r
counts <- read.csv("airway_scaledcounts.csv", row.names=1)
metadata <- read.csv("airway_metadata.csv") 

head(counts)
```

                    SRR1039508 SRR1039509 SRR1039512 SRR1039513 SRR1039516
    ENSG00000000003        723        486        904        445       1170
    ENSG00000000005          0          0          0          0          0
    ENSG00000000419        467        523        616        371        582
    ENSG00000000457        347        258        364        237        318
    ENSG00000000460         96         81         73         66        118
    ENSG00000000938          0          0          1          0          2
                    SRR1039517 SRR1039520 SRR1039521
    ENSG00000000003       1097        806        604
    ENSG00000000005          0          0          0
    ENSG00000000419        781        417        509
    ENSG00000000457        447        330        324
    ENSG00000000460         94        102         74
    ENSG00000000938          0          0          0

``` r
head(metadata)
```

              id     dex celltype     geo_id
    1 SRR1039508 control   N61311 GSM1275862
    2 SRR1039509 treated   N61311 GSM1275863
    3 SRR1039512 control  N052611 GSM1275866
    4 SRR1039513 treated  N052611 GSM1275867
    5 SRR1039516 control  N080611 GSM1275870
    6 SRR1039517 treated  N080611 GSM1275871

## Check metadata counts correspondance

We need to check that the metadata matches the samples in our count
data.

``` r
ncol(counts) == nrow(metadata)
```

    [1] TRUE

``` r
colnames(counts) == metadata$id
```

    [1] TRUE TRUE TRUE TRUE TRUE TRUE TRUE TRUE

> Q1. How many genes are in this dataset?

``` r
nrow(counts)
```

    [1] 38694

> Q2. How many “control” cell lines do we have in the dataset?

``` r
sum(metadata$dex == "control")
```

    [1] 4

## Analysis Plan

> Q3.

We have 4 replicates per condition (“control” and “treated”) We want to
compare the control vs the treated to see which genes expression levels
change when we have the drug present.

We will go row by row gene by gene) and see if the average value in
control columns is different than the average value in treated columns

- Step 1. Find which columns in `counts` correspond to “control”
  samples.

- Step 2. Extract these columns

- Step 3. Calculate an average value for each gene (e.g. each row).

``` r
control.inds <- metadata$dex == "control"
control.counts <- counts[, control.inds, drop = FALSE]
control.mean <- rowMeans(control.counts, na.rm = TRUE)
```

> Q4 Do the same for “treated” samples - find the mean count value per
> gene.

``` r
treated.inds <- metadata$dex == "treated"
treated.counts <- counts[, treated.inds, drop = FALSE]
treated.mean <- rowMeans(treated.counts, na.rm = TRUE)
```

Lets put these two mean values into a new dataframe `meancounts` for
easy book keeping and plotting.

``` r
meancounts <- data.frame(
  control.mean = control.mean,
  treated.mean = treated.mean
)

head(meancounts)
```

                    control.mean treated.mean
    ENSG00000000003       900.75       658.00
    ENSG00000000005         0.00         0.00
    ENSG00000000419       520.50       546.00
    ENSG00000000457       339.75       316.50
    ENSG00000000460        97.25        78.75
    ENSG00000000938         0.75         0.00

> Q5. Make a `ggplot` of average counts of control vs treated.

``` r
library(ggplot2)

ggplot(meancounts, aes(x = control.mean, y = treated.mean)) +
  geom_point(alpha = 0.3)
```

![](Class13_files/figure-commonmark/unnamed-chunk-7-1.png)

> Q6. Log Transform

It’s better to log transform because counts are highly skewed.

``` r
ggplot(meancounts, aes(x = control.mean, y = treated.mean)) +
  geom_point(alpha = 0.3) +
  scale_x_log10() +
  scale_y_log10()
```

    Warning in scale_x_log10(): log-10 transformation introduced infinite values.

    Warning in scale_y_log10(): log-10 transformation introduced infinite values.

![](Class13_files/figure-commonmark/unnamed-chunk-8-1.png)

## Log 2 units and fold change

If we consider “treated” / “control” counts we will get a number that
tells us the change

``` r
log2(20 / 20)
```

    [1] 0

``` r
# A doubling in treated versus control
log2(40 / 20)
```

    [1] 1

``` r
# A halving in treated versus control
log2(10 / 20)
```

    [1] -1

> Add a new column `log2fc` for log2 fold change of treated/ control to
> our `meancounts` object.

``` r
meancounts$log2fc <- log2(meancounts$treated.mean / meancounts$control.mean)
head(meancounts)
```

                    control.mean treated.mean      log2fc
    ENSG00000000003       900.75       658.00 -0.45303916
    ENSG00000000005         0.00         0.00         NaN
    ENSG00000000419       520.50       546.00  0.06900279
    ENSG00000000457       339.75       316.50 -0.10226805
    ENSG00000000460        97.25        78.75 -0.30441833
    ENSG00000000938         0.75         0.00        -Inf

## Remove zero count genes

Typically we would not consider zero count genes - as we have no data
about them and they should be excluded from further consideration. These
lead to “funky” log2 fold change values (eg divide by zero errors etc.)

## DESeq analysis

We are missing any measure of significance from the work we have so far.
Let’s do this properly with the DESeq2 package

``` r
library(DESeq2)
```

The DESeq2 package like many bioconductor packages, wants it’s input in
a very specific way - a data structure setup with all the info it needs
for the calculation.

``` r
dds <- DESeqDataSetFromMatrix(
  countData = counts,
  colData = metadata,
  design = ~ dex
)
```

    converting counts to integer mode

    Warning in DESeqDataSet(se, design = design, ignoreRank): some variables in
    design formula are characters, converting to factors

``` r
dds <- dds[rowSums(counts(dds)) > 0, ]
```

The main function in this package is called `DESeq()` it will run the
full analysis for us on our `dds` input object:

``` r
dds <- DESeq(dds)
```

    estimating size factors

    estimating dispersions

    gene-wise dispersion estimates

    mean-dispersion relationship

    final dispersion estimates

    fitting model and testing

Extract our results:

``` r
res <- results(dds)
head(res)
```

    log2 fold change (MLE): dex treated vs control 
    Wald test p-value: dex treated vs control 
    DataFrame with 6 rows and 6 columns
                       baseMean log2FoldChange     lfcSE      stat    pvalue
                      <numeric>      <numeric> <numeric> <numeric> <numeric>
    ENSG00000000003  747.194195      -0.350703  0.168242 -2.084514 0.0371134
    ENSG00000000419  520.134160       0.206107  0.101042  2.039828 0.0413675
    ENSG00000000457  322.664844       0.024527  0.145134  0.168996 0.8658000
    ENSG00000000460   87.682625      -0.147143  0.256995 -0.572550 0.5669497
    ENSG00000000938    0.319167      -1.732289  3.493601 -0.495846 0.6200029
    ENSG00000000971 5760.148362       0.459282  0.234318  1.960081 0.0499864
                         padj
                    <numeric>
    ENSG00000000003  0.164745
    ENSG00000000419  0.177814
    ENSG00000000457  0.962202
    ENSG00000000460  0.818588
    ENSG00000000938        NA
    ENSG00000000971  0.203083

## Volcano plot

A useful summary figure of our results is often called a volcano plot.
It is basically a plot of log2 fold change values vs adjusted P-values

> Q. Use ggplot to make a first version “volcano plot” of
> `log2FoldChange` vs `padj`

``` r
ggplot(as.data.frame(res), aes(x = log2FoldChange, y = padj)) +
  geom_point()
```

    Warning: Removed 9917 rows containing missing values or values outside the scale range
    (`geom_point()`).

![](Class13_files/figure-commonmark/unnamed-chunk-16-1.png)

This is not very useful because the y axis (p-values) is not really
helpful - we want to focus on low p-values

``` r
ggplot(as.data.frame(res), aes(x = log2FoldChange, y = -log10(padj))) +
  geom_point()
```

    Warning: Removed 9917 rows containing missing values or values outside the scale range
    (`geom_point()`).

![](Class13_files/figure-commonmark/unnamed-chunk-17-1.png)

Adding some threshold lines:

``` r
ggplot(as.data.frame(res), aes(x = log2FoldChange, y = -log10(padj))) +
  geom_point(alpha = 0.6) +
  geom_vline(xintercept = c(-2, 2)) +
  geom_hline(yintercept = -log10(0.05))
```

    Warning: Removed 9917 rows containing missing values or values outside the scale range
    (`geom_point()`).

![](Class13_files/figure-commonmark/unnamed-chunk-18-1.png)

## Add some plot annotation

> Q. Add color to the points (genes) we care about, nice axis labels, a
> useful title and a nice theme.

``` r
res_df <- as.data.frame(res)

sig <- !is.na(res_df$padj) & (res_df$padj < 0.05)
up <- sig & (res_df$log2FoldChange > 2)
down <- sig & (res_df$log2FoldChange < -2)

res_df$group <- "not_sig"
res_df$group[up] <- "up"
res_df$group[down] <- "down"

ggplot(res_df, aes(x = log2FoldChange, y = -log10(padj), color = group)) +
  geom_point(alpha = 0.7) +
  geom_vline(xintercept = c(-2, 2)) +
  geom_hline(yintercept = -log10(0.05)) +
  labs(
    title = "Differential expression: dexamethasone vs control",
    x = "Log2 fold change",
    y = "-Log10 adjusted p-value"
  ) +
  theme_minimal()
```

    Warning: Removed 9917 rows containing missing values or values outside the scale range
    (`geom_point()`).

![](Class13_files/figure-commonmark/unnamed-chunk-19-1.png)

## Save our results to a CSV file

``` r
write.csv(res, "results.csv")
```

## Add Annotation Data

To make sense of our results we need to know what the differentially
expressed genes are and what biological pathways and processes they are
involved in.

``` r
head(res)
```

    log2 fold change (MLE): dex treated vs control 
    Wald test p-value: dex treated vs control 
    DataFrame with 6 rows and 6 columns
                       baseMean log2FoldChange     lfcSE      stat    pvalue
                      <numeric>      <numeric> <numeric> <numeric> <numeric>
    ENSG00000000003  747.194195      -0.350703  0.168242 -2.084514 0.0371134
    ENSG00000000419  520.134160       0.206107  0.101042  2.039828 0.0413675
    ENSG00000000457  322.664844       0.024527  0.145134  0.168996 0.8658000
    ENSG00000000460   87.682625      -0.147143  0.256995 -0.572550 0.5669497
    ENSG00000000938    0.319167      -1.732289  3.493601 -0.495846 0.6200029
    ENSG00000000971 5760.148362       0.459282  0.234318  1.960081 0.0499864
                         padj
                    <numeric>
    ENSG00000000003  0.164745
    ENSG00000000419  0.177814
    ENSG00000000457  0.962202
    ENSG00000000460  0.818588
    ENSG00000000938        NA
    ENSG00000000971  0.203083

Lets start by mapping our ENSEMBLE ids to the more conventional gene
SYMBOL. We will use two bioconductor packages for this mapping
**AnnotationDbi** and **org.Hs.eg.db**.

We will first need to install these from bioconductor with
`BiocManager::install("AnnotationDbi")`

``` r
library(AnnotationDbi)
library(org.Hs.eg.db)
```

``` r
columns(org.Hs.eg.db)
```

     [1] "ACCNUM"       "ALIAS"        "ENSEMBL"      "ENSEMBLPROT"  "ENSEMBLTRANS"
     [6] "ENTREZID"     "ENZYME"       "EVIDENCE"     "EVIDENCEALL"  "GENENAME"    
    [11] "GENETYPE"     "GO"           "GOALL"        "IPI"          "MAP"         
    [16] "OMIM"         "ONTOLOGY"     "ONTOLOGYALL"  "PATH"         "PFAM"        
    [21] "PMID"         "PROSITE"      "REFSEQ"       "SYMBOL"       "UCSCKG"      
    [26] "UNIPROT"     

``` r
res$symbol <- mapIds(org.Hs.eg.db,
                    keys = rownames(res), # ids
                    keytype = "ENSEMBL",# format
                    column = "SYMBOL") # Translation target
```

    'select()' returned 1:many mapping between keys and columns

``` r
head(res)
```

    log2 fold change (MLE): dex treated vs control 
    Wald test p-value: dex treated vs control 
    DataFrame with 6 rows and 7 columns
                       baseMean log2FoldChange     lfcSE      stat    pvalue
                      <numeric>      <numeric> <numeric> <numeric> <numeric>
    ENSG00000000003  747.194195      -0.350703  0.168242 -2.084514 0.0371134
    ENSG00000000419  520.134160       0.206107  0.101042  2.039828 0.0413675
    ENSG00000000457  322.664844       0.024527  0.145134  0.168996 0.8658000
    ENSG00000000460   87.682625      -0.147143  0.256995 -0.572550 0.5669497
    ENSG00000000938    0.319167      -1.732289  3.493601 -0.495846 0.6200029
    ENSG00000000971 5760.148362       0.459282  0.234318  1.960081 0.0499864
                         padj      symbol
                    <numeric> <character>
    ENSG00000000003  0.164745      TSPAN6
    ENSG00000000419  0.177814        DPM1
    ENSG00000000457  0.962202       SCYL3
    ENSG00000000460  0.818588       FIRRM
    ENSG00000000938        NA         FGR
    ENSG00000000971  0.203083         CFH

> Q. Can you add “GENENAME” and “ENTREZID” as new columns to `res` named
> “name” and “entrez”

``` r
res$name <- mapIds(org.Hs.eg.db,
                    keys = rownames(res), # our ids
                    keytype = "ENSEMBL",# their format
                    column = "GENENAME") # what i want to translate to
```

    'select()' returned 1:many mapping between keys and columns

``` r
res$entrez <- mapIds(org.Hs.eg.db,
                    keys = rownames(res), # our ids
                    keytype = "ENSEMBL",# their format
                    column = "ENTREZID") # what i want to translate to
```

    'select()' returned 1:many mapping between keys and columns

``` r
head(res)
```

    log2 fold change (MLE): dex treated vs control 
    Wald test p-value: dex treated vs control 
    DataFrame with 6 rows and 9 columns
                       baseMean log2FoldChange     lfcSE      stat    pvalue
                      <numeric>      <numeric> <numeric> <numeric> <numeric>
    ENSG00000000003  747.194195      -0.350703  0.168242 -2.084514 0.0371134
    ENSG00000000419  520.134160       0.206107  0.101042  2.039828 0.0413675
    ENSG00000000457  322.664844       0.024527  0.145134  0.168996 0.8658000
    ENSG00000000460   87.682625      -0.147143  0.256995 -0.572550 0.5669497
    ENSG00000000938    0.319167      -1.732289  3.493601 -0.495846 0.6200029
    ENSG00000000971 5760.148362       0.459282  0.234318  1.960081 0.0499864
                         padj      symbol                   name      entrez
                    <numeric> <character>            <character> <character>
    ENSG00000000003  0.164745      TSPAN6          tetraspanin 6        7105
    ENSG00000000419  0.177814        DPM1 dolichyl-phosphate m..        8813
    ENSG00000000457  0.962202       SCYL3 SCY1 like pseudokina..       57147
    ENSG00000000460  0.818588       FIRRM FIGNL1 interacting r..       55732
    ENSG00000000938        NA         FGR FGR proto-oncogene, ..        2268
    ENSG00000000971  0.203083         CFH    complement factor H        3075

``` r
write.csv(res, file="results_annotated.csv")
```

## Pathway analysis

Now we know the gene names (gene symbols) and their entrez IDs we can
find out what pathways they are involved in. This is called “pathway
analysis” or “gene set enrichment”

We will use the **gage** package and the **pathviewer** package to do
this analysis (but there are loads of others)

``` r
library(gage)
library(gageData)
library(pathview)
```

Let’s see what is in gageData, specifically KEGG pathways:

``` r
data(kegg.sets.hs)
head(kegg.sets.hs, 2)
```

    $`hsa00232 Caffeine metabolism`
    [1] "10"   "1544" "1548" "1549" "1553" "7498" "9"   

    $`hsa00983 Drug metabolism - other enzymes`
     [1] "10"     "1066"   "10720"  "10941"  "151531" "1548"   "1549"   "1551"  
     [9] "1553"   "1576"   "1577"   "1806"   "1807"   "1890"   "221223" "2990"  
    [17] "3251"   "3614"   "3615"   "3704"   "51733"  "54490"  "54575"  "54576" 
    [25] "54577"  "54578"  "54579"  "54600"  "54657"  "54658"  "54659"  "54963" 
    [33] "574537" "64816"  "7083"   "7084"   "7172"   "7363"   "7364"   "7365"  
    [41] "7366"   "7367"   "7371"   "7372"   "7378"   "7498"   "79799"  "83549" 
    [49] "8824"   "8833"   "9"      "978"   

To run our pathway analysis we will use the **gage()** function. It
wants two main inputs: a vector of importance (in our case the log2 fold
change values); and the gene sets to check overlap for.

``` r
foldchanges <- res$log2FoldChange
names(foldchanges) <- res$symbol
head(foldchanges)
```

         TSPAN6        DPM1       SCYL3       FIRRM         FGR         CFH 
    -0.35070296  0.20610728  0.02452701 -0.14714263 -1.73228897  0.45928190 

KEGG speaks entrez (ie uses ENTREZID format) not gene symbol format

``` r
names(foldchanges) <- res$entrez
keggres = gage(foldchanges, gsets=kegg.sets.hs)
head(keggres$less, 5)
```

                                                            p.geomean stat.mean
    hsa05332 Graft-versus-host disease                    0.005911355 -2.606341
    hsa04940 Type I diabetes mellitus                     0.008931867 -2.431362
    hsa05310 Asthma                                       0.014628499 -2.267572
    hsa04672 Intestinal immune network for IgA production 0.016584119 -2.169561
    hsa04340 Hedgehog signaling pathway                   0.019100696 -2.103322
                                                                p.val     q.val
    hsa05332 Graft-versus-host disease                    0.005911355 0.7953988
    hsa04940 Type I diabetes mellitus                     0.008931867 0.7953988
    hsa05310 Asthma                                       0.014628499 0.7953988
    hsa04672 Intestinal immune network for IgA production 0.016584119 0.7953988
    hsa04340 Hedgehog signaling pathway                   0.019100696 0.7953988
                                                          set.size        exp1
    hsa05332 Graft-versus-host disease                          28 0.005911355
    hsa04940 Type I diabetes mellitus                           33 0.008931867
    hsa05310 Asthma                                             21 0.014628499
    hsa04672 Intestinal immune network for IgA production       39 0.016584119
    hsa04340 Hedgehog signaling pathway                         49 0.019100696

Lets make a figure of one of these pathways with our DEGs highlighted:

``` r
pathview(foldchanges, pathway.id = "hsa05310")
```

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/Class13

    Info: Writing image file hsa05310.pathview.png

![](hsa05310.pathview.png) \>Q Generate and insert a pathway figure for
“Graft-versus-host diseases” and “Type I Diabetes”?

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa05332")  # Graft-versus-host disease
```

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/Class13

    Info: Writing image file hsa05332.pathview.png

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa04940")  # Type I diabetes mellitus
```

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/Class13

    Info: Writing image file hsa04940.pathview.png

![](hsa05332.pathview.png) ![](hsa04940.pathview.png)
