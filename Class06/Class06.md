# Class 6: R Functions
Matthew Huang (PID: A17978767)

## Background

Functions are central to working in R. Most tasks involve calling
functions, like reading data, analyzing results, and producing output.

In general, an R function has:

1.  A **name** (what you call)
2.  One or more input **arguments** (what you pass in)
3.  A **body** (the code inside `{ }` that runs)

## A first function

Here is a simple function that adds 1 to an input value:

``` r
add <- function(x) {
  x + 1
}
```

Try it out:

``` r
add(100)
```

    [1] 101

This also works on vectors (R is vectorized):

``` r
add(c(100, 200, 300))
```

    [1] 101 201 301

Now make it more flexible by allowing a custom amount to add. Here, y is
optional with a default of 1:

``` r
add <- function(x, y = 1) {
  x + y
}
```

``` r
add(100, 10)
```

    [1] 110

If you leave y blank, the default is used:

``` r
add(100)
```

    [1] 101

A couple examples of using built-in functions:

``` r
plot(1:10, col = "black", type = "b")
```

![](Class06_files/figure-commonmark/unnamed-chunk-7-1.png)

``` r
log(10, base = 10)
```

    [1] 1

> Note: Some arguments are required, while others are optional. Optional
> arguments usually have a default value like y = 1.

Named arguments can be supplied in any order:

``` r
# add(x = 100, y = 200)
```

A second function

Most R functions follow this general structure:

`name <- function(arguments) { body }`

The `sample()` function is useful for random sampling:

``` r
sample(1:10, size = 5)
```

    [1] 6 5 7 2 3

> Q: Return 12 numbers sampled from 1:10 (allow repeats)

``` r
sample(1:10, size = 12, replace = TRUE)
```

     [1]  8  8  6 10  3  2  3  5  6  6  9  8

> Q: Generate a 12-nucleotide random DNA sequence

``` r
bases <- c("A", "T", "C", "G")
dna_seq <- sample(bases, size = 12, replace = TRUE)
paste(dna_seq, collapse = "")
```

    [1] "AAGCGTTAAGTG"

> Q: Write a function called generate_dna() that returns a random DNA
> sequence of length n

``` r
generate_dna <- function(n = 25) {
  bases <- c("A", "T", "C", "G")
  dna_seq <- sample(bases, size = n, replace = TRUE)
  paste(dna_seq, collapse = "")
}
```

``` r
generate_dna(50)
```

    [1] "GCCAGCTAATTTACTTGCCCGATCCAAGCTGATGACTGAGCTGCATTACT"

Example of collapsing a character vector into a single string:

``` r
x <- c("H", "E", "L", "L", "O")
paste(x, collapse = "")
```

    [1] "HELLO"

Q: Give the user an option to return FASTA format or just the raw
sequence string

``` r
generate_dna <- function(n=50, fasta=T) {
  bases <- c("A","G","C","T")
  DNAseq <- sample(bases, size=n, replace=T)
  
if(fasta) {
  DNAseq <- paste(DNAseq, collapse="")
  cat("Bye")
} else {
  cat("Hello")
  }
  return(DNAseq)
}
```

``` r
generate_dna(5, fasta = FALSE)
```

    Hello

    [1] "C" "T" "C" "C" "G"

A new function: protein sequences

Q: Write a function called generate_protein() that returns a
user-specified length protein sequence

``` r
generate_protein <- function(n = 6, fasta = TRUE) {
  amino_acids <- c(
    "A","R","N","D","C","Q","E","G","H","I",
    "L","K","M","F","P","S","T","W","Y","V"
  )

  prot <- sample(amino_acids, size = n, replace = TRUE)
  prot_string <- paste(prot, collapse = "")

  if (fasta) {
    fasta_out <- paste0(">random_protein_", n, "\n", prot_string)
    return(fasta_out)
  }

  return(prot_string)
}
```

Q: Generate sequences of length 6 through 12

``` r
generate_protein <- function(n = 6) {
  aminoacids <- c("A", "R", "N", "D", "C", "Q", "E", "G", "H", "I",
                  "L", "K", "M", "F", "P", "S", "T", "W", "Y", "V")

  protein <- sample(aminoacids, size = n, replace = TRUE)
  protein <- paste(protein, collapse = "")
  return(protein)
}
```

VEGWCAEC matches with accession number NJD86855.1

``` r
generate_protein()
```

    [1] "LHTTYF"

``` r
generate_protein(6)
```

    [1] "VYSFIP"

``` r
generate_protein(7)
```

    [1] "NPWVIWS"

``` r
generate_protein(8)
```

    [1] "YQKHWPWM"

``` r
generate_protein(9)
```

    [1] "FFNEDYAMH"

``` r
generate_protein(10)
```

    [1] "GIPTGEGKCS"

``` r
generate_protein(11)
```

    [1] "ESHAPDWSPHH"

``` r
generate_protein(12)
```

    [1] "EVPASVNEKGAG"

Or use a for() loop:

``` r
for (i in 6:12) {
  cat(">", sep="", i, "\n")
  cat(generate_protein(i), "\n" )
}
```

    >6
    GDSELC 
    >7
    TCWEKWH 
    >8
    GDLRACMG 
    >9
    FDFTLKAHN 
    >10
    KAGDSAVLNC 
    >11
    VLHLAYWTWIN 
    >12
    HGFHYEQAPTIK 
