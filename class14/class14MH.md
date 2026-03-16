# Class 14: RNASeq Mini Project
Matthew Huang (A17978767)

- [Background](#background)
- [Data Import](#data-import)
  - [Cleanup (data tidying)](#cleanup-data-tidying)
- [DESeq Analysis](#deseq-analysis)
  - [Setting up the input object](#setting-up-the-input-object)
  - [Running DESeq](#running-deseq)
  - [Getting results](#getting-results)
- [Volcano Plot](#volcano-plot)
- [Add Annotation (gene symbols and entrez
  ids)](#add-annotation-gene-symbols-and-entrez-ids)
- [Pathway Analysis](#pathway-analysis)
  - [KEGG](#kegg)
  - [GO](#go)
  - [Reactome](#reactome)

## Background

The data for today’s mini-project comes from a knock-down study of an
important Hox gene

## Data Import

``` r
countFile <- "GSE37704_featurecounts.csv"
metaFile  <- "GSE37704_metadata.csv"

countData <- read.csv(countFile, row.names = 1)
colData   <- read.csv(metaFile,  row.names = 1)
```

### Cleanup (data tidying)

``` r
countData <- countData[, -1]

to.keep <- rowSums(countData) > 10

countData <- countData[to.keep, ]
```

We need to remove the length column from our `countData` to make the
columns match the rows in `colData`

``` r
head(countData)
```

                    SRR493366 SRR493367 SRR493368 SRR493369 SRR493370 SRR493371
    ENSG00000279457        23        28        29        29        28        46
    ENSG00000187634       124       123       205       207       212       258
    ENSG00000188976      1637      1831      2383      1226      1326      1504
    ENSG00000187961       120       153       180       236       255       357
    ENSG00000187583        24        48        65        44        48        64
    ENSG00000187642         4         9        16        14        16        16

``` r
head(colData)
```

                  condition
    SRR493366 control_sirna
    SRR493367 control_sirna
    SRR493368 control_sirna
    SRR493369      hoxa1_kd
    SRR493370      hoxa1_kd
    SRR493371      hoxa1_kd

``` r
ncol(countData) == nrow(colData)
```

    [1] TRUE

## DESeq Analysis

``` r
library(DESeq2)
```

### Setting up the input object

``` r
dds <- DESeqDataSetFromMatrix(
  countData = countData,
  colData   = colData,
  design    = ~ condition
)
```

    Warning in DESeqDataSet(se, design = design, ignoreRank): some variables in
    design formula are characters, converting to factors

### Running DESeq

``` r
dds <- DESeq(dds)
```

    estimating size factors

    estimating dispersions

    gene-wise dispersion estimates

    mean-dispersion relationship

    final dispersion estimates

    fitting model and testing

### Getting results

``` r
res <- results(dds)
head(res)
```

    log2 fold change (MLE): condition hoxa1 kd vs control sirna 
    Wald test p-value: condition hoxa1 kd vs control sirna 
    DataFrame with 6 rows and 6 columns
                     baseMean log2FoldChange     lfcSE       stat      pvalue
                    <numeric>      <numeric> <numeric>  <numeric>   <numeric>
    ENSG00000279457   29.9120      0.1800352 0.3146287   0.572215 5.67176e-01
    ENSG00000187634  183.2182      0.4259359 0.1364245   3.122137 1.79543e-03
    ENSG00000188976 1651.1025     -0.6927959 0.0548876 -12.622082 1.59547e-36
    ENSG00000187961  209.6244      0.7298671 0.1285341   5.678394 1.35965e-08
    ENSG00000187583   47.2512      0.0395066 0.2628513   0.150300 8.80528e-01
    ENSG00000187642   11.9786      0.5401497 0.5043073   1.071073 2.84137e-01
                           padj
                      <numeric>
    ENSG00000279457 6.52317e-01
    ENSG00000187634 3.65952e-03
    ENSG00000188976 1.79740e-35
    ENSG00000187961 4.65764e-08
    ENSG00000187583 9.11673e-01
    ENSG00000187642 3.67241e-01

``` r
write.csv(res, "results.csv")
```

## Volcano Plot

``` r
library(ggplot2)

res_df <- as.data.frame(res)
res_df <- res_df[!is.na(res_df$padj), , drop = FALSE]

ggplot(res_df, aes(x = log2FoldChange, y = -log10(padj))) +
  geom_point(alpha = 0.6) +
  geom_vline(xintercept = c(-2, 2)) +
  geom_hline(yintercept = -log10(0.05)) +
  labs(
    title = "Hox gene knockdown: differential expression",
    x = "Log2 fold change",
    y = "-Log10 adjusted p-value"
  ) +
  theme_minimal()
```

![](class14_files/figure-commonmark/unnamed-chunk-8-1.png)

``` r
res_df$group <- "not_sig"
res_df$group[res_df$padj < 0.05 & res_df$log2FoldChange >  2] <- "up"
res_df$group[res_df$padj < 0.05 & res_df$log2FoldChange < -2] <- "down"

ggplot(res_df, aes(x = log2FoldChange, y = -log10(padj), color = group)) +
  geom_point(alpha = 0.7) +
  geom_vline(xintercept = c(-2, 2)) +
  geom_hline(yintercept = -log10(0.05)) +
  labs(
    title = "Hox gene knockdown: volcano plot",
    x = "Log2 fold change",
    y = "-Log10 adjusted p-value"
  ) +
  theme_minimal()
```

![](class14_files/figure-commonmark/unnamed-chunk-9-1.png)

## Add Annotation (gene symbols and entrez ids)

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
                    column = "SYMBOL") # translating to
```

    'select()' returned 1:many mapping between keys and columns

``` r
res$entrez <- mapIds(org.Hs.eg.db,
                    keys = rownames(res), # ids
                    keytype = "ENSEMBL",# format
                    column = "ENTREZID") # translating to
```

    'select()' returned 1:many mapping between keys and columns

``` r
res$name <- mapIds(org.Hs.eg.db,
                    keys = rownames(res), # ids
                    keytype = "ENSEMBL",# format
                    column = "GENENAME") # translating to
```

    'select()' returned 1:many mapping between keys and columns

``` r
res = res[order(res$pvalue),]
write.csv(res, file="deseq_results.csv")

head(res)
```

    log2 fold change (MLE): condition hoxa1 kd vs control sirna 
    Wald test p-value: condition hoxa1 kd vs control sirna 
    DataFrame with 6 rows and 9 columns
                     baseMean log2FoldChange     lfcSE      stat    pvalue
                    <numeric>      <numeric> <numeric> <numeric> <numeric>
    ENSG00000117519   4483.44       -2.42277 0.0609018  -39.7816         0
    ENSG00000183508   2053.71        3.20181 0.0727131   44.0335         0
    ENSG00000159176   5692.21       -2.31380 0.0585392  -39.5256         0
    ENSG00000116016   4423.77       -1.88809 0.0440078  -42.9034         0
    ENSG00000164251   2348.58        3.34442 0.0694188   48.1775         0
    ENSG00000124766   2576.46        2.39219 0.0621624   38.4829         0
                         padj      symbol      entrez                   name
                    <numeric> <character> <character>            <character>
    ENSG00000117519         0        CNN3        1266             calponin 3
    ENSG00000183508         0      TENT5C       54855 terminal nucleotidyl..
    ENSG00000159176         0       CSRP1        1465 cysteine and glycine..
    ENSG00000116016         0       EPAS1        2034 endothelial PAS doma..
    ENSG00000164251         0       F2RL1        2150 F2R like trypsin rec..
    ENSG00000124766         0        SOX4        6659 SRY-box transcriptio..

## Pathway Analysis

``` r
library(gage)
library(gageData)
library(pathview)
```

``` r
foldchanges <- res$log2FoldChange
names(foldchanges) <- res$symbol
head(foldchanges)
```

         CNN3    TENT5C     CSRP1     EPAS1     F2RL1      SOX4 
    -2.422773  3.201809 -2.313796 -1.888087  3.344422  2.392190 

### KEGG

``` r
data(kegg.sets.hs)

keggres <- gage(foldchanges, gsets = kegg.sets.hs)
head(keggres$less, 5)
```

                                             p.geomean stat.mean p.val q.val
    hsa00232 Caffeine metabolism                    NA       NaN    NA    NA
    hsa00983 Drug metabolism - other enzymes        NA       NaN    NA    NA
    hsa01100 Metabolic pathways                     NA       NaN    NA    NA
    hsa00230 Purine metabolism                      NA       NaN    NA    NA
    hsa05340 Primary immunodeficiency               NA       NaN    NA    NA
                                             set.size exp1
    hsa00232 Caffeine metabolism                    0   NA
    hsa00983 Drug metabolism - other enzymes        0   NA
    hsa01100 Metabolic pathways                     0   NA
    hsa00230 Purine metabolism                      0   NA
    hsa05340 Primary immunodeficiency               0   NA

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa04110")
```

    Warning: None of the genes or compounds mapped to the pathway!
    Argument gene.idtype or cpd.idtype may be wrong.

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/class14

    Info: Writing image file hsa04110.pathview.png

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa04114")
```

    Warning: None of the genes or compounds mapped to the pathway!
    Argument gene.idtype or cpd.idtype may be wrong.

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/class14

    Info: Writing image file hsa04114.pathview.png

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa03030")
```

    Warning: None of the genes or compounds mapped to the pathway!
    Argument gene.idtype or cpd.idtype may be wrong.

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/class14

    Info: Writing image file hsa03030.pathview.png

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa05130")
```

    Warning: None of the genes or compounds mapped to the pathway!
    Argument gene.idtype or cpd.idtype may be wrong.

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/class14

    Info: Writing image file hsa05130.pathview.png

``` r
pathview(gene.data = foldchanges, pathway.id = "hsa03440")
```

    Warning: None of the genes or compounds mapped to the pathway!
    Argument gene.idtype or cpd.idtype may be wrong.

    'select()' returned 1:1 mapping between keys and columns

    Info: Working in directory /Users/matthewhuang/BIMM 143 correct/BIMM 143/class14

    Info: Writing image file hsa03440.pathview.png

![](hsa04110.pathview.png) ![](hsa04114.pathview.png)
![](hsa03030.pathview.png) ![](hsa05130.pathview.png)
![](hsa03440.pathview.png)

### GO

``` r
data(go.sets.hs)
data(go.subs.hs)

gobpsets <- go.sets.hs[go.subs.hs$BP]
gobpres  <- gage(foldchanges, gsets = gobpsets)

head(gobpres$less)
```

                                                   p.geomean stat.mean p.val q.val
    GO:0000002 mitochondrial genome maintenance           NA       NaN    NA    NA
    GO:0000003 reproduction                               NA       NaN    NA    NA
    GO:0000012 single strand break repair                 NA       NaN    NA    NA
    GO:0000018 regulation of DNA recombination            NA       NaN    NA    NA
    GO:0000019 regulation of mitotic recombination        NA       NaN    NA    NA
    GO:0000022 mitotic spindle elongation                 NA       NaN    NA    NA
                                                   set.size exp1
    GO:0000002 mitochondrial genome maintenance           0   NA
    GO:0000003 reproduction                               0   NA
    GO:0000012 single strand break repair                 0   NA
    GO:0000018 regulation of DNA recombination            0   NA
    GO:0000019 regulation of mitotic recombination        0   NA
    GO:0000022 mitotic spindle elongation                 0   NA

### Reactome

``` r
sig_genes <- res[!is.na(res$padj) & res$padj <= 0.05, "symbol"]
sig_genes <- sig_genes[!is.na(sig_genes)]

cat("Total number of significant genes:", length(sig_genes), "\n")
```

    Total number of significant genes: 8262 

``` r
write.table(
  sig_genes,
  file = "significant_genes.txt",
  row.names = FALSE,
  col.names = FALSE,
  quote = FALSE
)
```

> Q: What pathway has the most significant “Entities p-value”? Do the
> most significant pathways listed match your previous KEGG results?
> What factors could cause differences between the two methods?

Cell Cycle (Mitotic) was the most significant pathway. Overall, the most
significant pathways largely align with the earlier KEGG results.
Differences between the two methods likely come from how each database
organizes and defines pathways. KEGG tends to group processes more
broadly, like listing the cell cycle as a single pathway, while Reactome
breaks the same biological processes into multiple, more specific and
detailed pathways.
