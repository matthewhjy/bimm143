# Find A Gene Project
Matthew Huang (PID: A17978767 \| mah040@ucsd.edu)
2026-03-09

# Part 1:

## Q1.

*Tell me the name of a protein you are interested in. Include the
species, accession number and known function. This can be a human
protein or a protein from any other species as long as it’s function is
known. *

Name: Kinesin Family Number 11 (KIF11)

Accession: NP_004514

Species: Homo Sapiens

Function: A motor protein required for the establishment of a bipolar
mitotic spindle, thus facilitating chromosome congression during
mitosis.

## Q2.

*Perform a BLAST search against a DNA database, such as a database
consisting of genomic DNA or ESTs. The BLAST server can be at NCBI or
elsewhere. Include details of the BLAST method used, database searched
and any limits applied (e.g. Organism).*

Method: tblastn search against ESTS

Database: Expressed Sequence Tags (est)

<img src="BlastSearch.png" style="width:90.0%"
alt="tBLASTn setup for human KIF11 (NP_004514) against the NCBI EST nucleotide database" />

<img src="Catfishshow.png" style="width:90.0%"
alt="tBLASTn results summary showing a significant match between the human KIF11 protein (NP_004514) and an expressed sequence tag (CK418843.1) from Ictalurus punctatus identified in the NCBI EST database." />

<img src="Catfishseqalign.png" style="width:90.0%"
alt="Pairwise tBLASTn alignment between the human KIF11 protein (NP_004514) and the Ictalurus Punctatus EST FG708023.1, showing alignment score, E-value, and sequence similarity." />

## Q3.

*Gather information about this “novel” protein. At a minimum, show me
the protein sequence of the “novel” protein as displayed in your BLAST
results from \[Q2\] as FASTA format (you can copy and paste the aligned
sequence subject lines from your BLAST result page if necessary) or
translate your novel DNA sequence using a tool called EMBOSS Transeq at
the EBI. Don’t forget to translate all six reading frames; the ORF (open
reading frame) is likely to be the longest sequence without a stop
codon. It may not start with a methionine if you don’t have the complete
coding region. Make sure the sequence you provide includes a
header/subject line and is in traditional FASTA format. *

Chosen Sequence:

*\>Ictalurus_Punctatus
PRVRKRKTASTLMNAYSSRSHSVFSVTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRS
GAVDKRAREAGNINQSLLTLGRVITALVERGPHVPYRESKLTRILQDSLGGRTKTSIIAT
VSPASNNLEETLSTLDYANRAKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREK
HGVYLSAENYESMNSKLMSQEEQIIEYTERIAAAEEELKRVMELFTDSKQKLEQYSEDLQ
DKGQQLVEAHKDLQETKQKLTQEVFITTQLQTTREQLYNAAGQLLSTAEVSTSECGRPHX
NCXRGRPGXTQHRA*

Name: Putative KIF11-like Protein

Species: *Ictalurus Punctatus *

## Q4.

*Prove that this gene, and its corresponding protein, are novel. For the
purposes of this project, “novel” is defined as follows. Take the
protein sequence (your answer to \[Q3\]), and use it as a query in a
blastp search of the nr database at NCBI. *

A blastp search with a nr database yielded a top hit result is to an
Anolis Crolinesis protein with \<100% percent identity. See attached
screenshots for top hits and selected alignment details.

<img src="catfishblastsearch.png" style="width:95.0%"
alt="NCBI blastp search setup for the translated catfish candidate protein derived from EST accession CK418843.1, queried against the non-redundant protein sequence (nr) database to assess whether the predicted protein is novel." />

<img src="novelproteinss.png" style="width:95.0%"
alt="blastp results summary for the translated protein from CK418843.1 against the NCBI nr database. The top matches are KIF11-family proteins from catfish and related teleost species; the best hit to Ictalurus punctatus shows 96.67% amino-acid identity with 96% query coverage and E-value = 0.0, supporting homology to KIF11 while remaining below the project’s 100% same-species cutoff for a non-novel sequence" />

<img src="q4lma.png" style="width:95.0%"
alt="Top blastp pairwise alignment of the novel catfish protein against kinesin-like protein KIF11 from Ictalurus punctatus (XP_017332301.1). The alignment shows 97% identity, 97% positives, 0 gaps, and an E-value of 0.0, indicating the sequence is highly similar but not identical and is consistent with a novel KIF11-like gene." />

# Part 2:

## Q5.

*Generate a multiple sequence alignment with your novel protein, your
original query protein, and a group of other members of this family from
different species.*

Re-labeled sequences for alignment

<pre style='font-family: Courier, monospace; font-size: 7pt; line-height: 1.1;'>
&#10;>Catfish_Novel
&#10;PRVRKRKTASTLMNAYSSRSHSVFSVTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREA
GNINQSLLTLGRVITALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDYANR
AKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQIIEYTER
IAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLVEAHKDLQETKQKLTQEVFITTQLQTTREQLYNA
AGQLLSTAEVSTSECGRPHXNCXRGRPGXTQHRA
----------------------------------------------------------------------
>Human KIF11
&#10;MASQPNSSAKKKEEKGKNIQVVVRCRPFNLAERKASAHSIVECDPVRKEVSVRTGGLADKSSRKTYTFDM
VFGASTKQIDVYRSVVCPILDEVIMGYNCTIFAYGQTGTGKTFTMEGERSPNEEYTWEEDPLAGIIPRTL
HQIFEKLTDNGTEFSVKVSLLEIYNEELFDLLNPSSDVSERLQMFDDPRNKRGVIIKGLEEITVHNKDEV
YQILEKGAAKRTTAATLMNAYSSRSHSVFSVTIHMKETTIDGEELVKIGKLNLVDLAGSENIGRSGAVDK
RAREAGNINQSLLTLGRVITALVERTPHVPYRESKLTRILQDSLGGRTRTSIIATISPASLNLEETLSTL
EYAHRAKNILNKPEVNQKLTKKALIKEYTEEIERLKRDLAAAREKNGVYISEENFRVMSGKLTVQEEQIV
ELIEKIGAVEEELNRVTELFMDNKNELDQCKSDLQNKTQELETTQKHLQETKLQLVKEEYITSALESTEE
KLHDAASKLLNTVEETTKDVSGLHSKLDRKKAVDQHNAEAQDIFGKNLNSLFNNMEELIKDGSSKQKAML
EVHKTLFGNLLSSSVSALDTITTVALGSLTSIPENVSTHVSQIFNMILKEQSLAAESKTVLQELINVLKT
DLLSSLEMILSPTVVSILKINSQLKHIFKTSLTVADKIEDQKKELDGFLSILCNNLHELQENTICSLVES
QKQCGNLTEDLKTIKQTHSQELCKLMNLWTERFCALEEKCENIQKPLSSVQENIQQKSKDIVNKMTFHSQ
KFCADSDGFSQELRNFNQEGTKLVEESVKHSDKLNGNLEKISQETEQRCESLNTRTVYFSEQWVSSLNER
EQELHNLLEVVSQCCEASSSDITEKSDGRKAAHEKQHNIFLDQMTIDEDKLIAQNLELNETIKIGLTKLN
CFLEQDLKLDIPTGTTPQRKSYLYPSTLVRTEPREHLLDQLKRKQPELLMMLNCSENNKEETIPDVDVEE
AVLGQYTEEPLSQEPSVDAGVDCSSIGGVPFFQHKKSHGKDKENRGINTLERSKVEETTEHLVTKSRLPL
RAQINL
----------------------------------------------------------------------
>Mouse KIF11
&#10;MASQPSSLKKKEEKGRNIQVVVRCRPFNLAERKANAHSVVECDHARKEVSVRTAGLTDKTSKKTYTFDMV
FGASTKQIDVYRSVVCPILDEVIMGYNCTIFAYGQTGTGKTFTMEGERSPNEVYTWEEDPLAGIIPRTLH
QIFEKLTDNGTEFSVKVSLLEIYNEELFDLLSPSSDVSERLQMFDDPRNKRGVIIKGLEEITVHNKDEVY
QILEKGAAKRTTAATLMNAYSSRSHSVFSVTIHMKETTIDGEELVKIGKLNLVDLAGSENIGRSGAVDKR
AREAGNINQSLLTLGRVITALVERTPHIPYRESKLTRILQDSLGGRTRTSIIATISPASFNLEETLSTLE
YAHRAKNIMNKPEVNQKLTKKALIKEYTEEIERLKRDLAAAREKNGVYISEESFRAMNGKVTVQEEQIVE
LVEKIAVLEEELSKATELFMDSKNELDQCKSDLQTKTQELETTQKHLQETKLQLVKEEYVSSALERTEKT
LHDTASKLLNTVKETTRAVSGLHSKLDRKRAIDEHNAEAQESFGKNLNSLFNNMEELIKDGSAKQKAMLD
VHKTLFGNLMSSSVSALDTITTTALESLVSIPENVSARVSQISDMILEEQSLAAQSKSVLQGLIDELVTD
LFTSLKTIVAPSVVSILNINKQLQHIFRASSTVAEKVEDQKREIDSFLSILCNNLHELRENTVSSLVESQ
KLCGDLTEDLKTIKETHSQELCQLSSSWAERFCALEKKYENIQKPLNSIQENTELRSTDIINKTTVHSKK
ILAESDGLLQELRHFNQEGTQLVEESVGHCSSLNSNLETVSQEITQKCGTLNTSTVHFSDQWASCLSKRK
EELENLMEFVNGCCKASSSEITKKVREQSAAVANQHSSFVAQMTSDEESCKAGSLELDKTIKTGLTKLNC
FLKQDLKLDIPTGMTPERKKYLYPTTLVRTEPREQLLDQLQKKQPPMMLNSSEASKETSQDMDEEREALE
QCTEELVSPETTEHPSADCSSSRGLPFFQRKKPHGKDKENRGLNPVEKYKVEEASDLSISKSRLPLHTSI
NL
----------------------------------------------------------------------
>Channel_Catfish KIF11 Full
&#10;MSNLIPTTKKDEKGRNIQVVVRCRPFNAVERKSNAYGVVDCDSNRKEVTVRTGSANDKTAKKMYTFDMVF
GPSAKQIDVYRSVVYPILDEVIMGYNCTIFAYGQTGTGKTFTMEGERSPNEEFTWEEDPLAGVIPRTLHQ
IFEKLTSNGTEFSVKVSLLEIYNEELFDLLSPSADVSERLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQ
ILERGAAKRKTASTLMNAYSSRSHSVFSVTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRA
REAGNINQSLLTLGRVITALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDY
ANRAKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQIIEY
TERIAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLVEAHKDLQETKQKLTQEVFITTQLQTTESQL
YNAAGQLLSTAEVSTSDVGGLHAKLQRVKAVEQHNTAAQSNFAQSMEQNCSSMQSALQEHSLKHAAMLDH
YRHATAELLGGTGRVFEEAVGSVLRSYSAVREAVEEGVDGCKAKLLLQEELTKESRDAMLQMLEEHKLHL
EEVLIAKAMPAIGSIMAVNEGLKQTLQRYNVLDGQMVGMKAEMTAFFSGHMAVLASMRETAVQGFTSLQA
EHHKLKGQMDQANRQHKTRVAQLLQCVQDQMNLLALDTQSDFKELQEASDAQGALLETLKNTVQSQCTEA
EQESSSVGARLSSCTDGVMKEMSGVAEEGRMTLEEGAGYCSLLQSSLESLAESSLQWCHAARDLTERRAK
EQLTLAEENKATVHKLLKHVEERGQAAVEECRTRLAPVQQEMEVLLKRVEVQTGEDQAVLQQHREELSTL
AAQALDTVNHFLSNELQQDLPTGTTPKRKEYMYPRVLVPLRSRTELEEEFRAQQEELMSVLGQCEVLMEG
EEEEKPVDQDSLEDDVCSSSDSNSTQQSMSDENLMCYEEGRVPFFKKKSKKENFSSKSLNRSKVAENETS
VTPPRSRLPLRTQNQ
----------------------------------------------------------------------
>Blue_Catfish KIF11
&#10;MSNLIPTTKKDEKGRNIQVVVRCRPFNAVERKSNAYGVVDCDSNRKEVTVRTGSANDKTGRKMYTFDMVF
GPSAKQIDVYRSVVYPILDEVIMGYNCTIFAYGQTGTGKTFTMEGERSPNEEFTWEEDPLAGVIPRTLHQ
IFEKLTSNGTEFSVKVSLLEIYNEELFDLLSPSADVSERLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQ
ILERGAAKRKTASTLMNAYSSRSHSVFSVTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRA
REAGNINQSLLTLGRVITALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDY
ANRAKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQIIEY
TERIAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLEEAHKDLQETKQKLTQEVFITTQLQTTESQL
YNAAGQLLSTAEVSTSDVGGLHAKLQRVQAVEQHNAAAQGNFAQSMEQNCSSMQSALQEHNLKHAAMLDH
YRHSTAELLGGTGRVFEEAVGSVLRSYGAVREAVEEGVDGCKAKLLQQEELTKESRDAMLQMLEEHKLHL
EDVLIAKAMPAIGSIMAVNEGLKQTLQRYNVLDGQMVGMKAEMTAFFSGHMAALASMRETAVQGFTSLQA
EHHKLKGQMDQANRQHKTRVAQLLQCVQDQMNLLALDTQSDFKELQEASDAQGALLETLKNTVQSQCTEA
EQESLSVGARLSSCTDGVMKEMSGVAEEGRVALEEGAGYCSLLQSSLESLAESSLQWCHAARDLTERRAK
EQLTLAEENKATVHELLKRVEERGQAAVEECRTRLAPVQQEMEVLLKGVEVQTGEDQAVLQQHREELSTL
AAQALDTVNHFLSNELQQDLPTGTTPKRKEYMYPRVLVPLRSRTELEEEFRAQQEELMSVLGQCEVLMEG
EEEEKPVDQDSLEDDVCSSSDSNSTQQSMSDENLMCYEEGRVPFFKKKSKKENFSSKSLNRSKVAENETS
VTPPRSRLPLRTQNQ
----------------------------------------------------------------------
>Zebrafish KIF11
&#10;MASSQVPAAKKDEKGRNIQVVVRCRPFNTVERKSGSHTVVECDQNRKEVIMRTGGATDKAARKTYTFDMV
FGPSAKQIEVYRSVVCPILDEVIMGYNCTIFAYGQTGTGKTFTMEGDRSPNEEFTWEEDPLAGIIPRTLH
QIFEKLSNNGTEFSVKVSLLEIYNEELFDLLSPAPDVTERLQLFDDPRNKRGVTIKGLEEITVHNKNEVY
QILERGAAKRKTASTLMNAYSSRSHSVFSVTIHMKEITLDGEELVKIGKLNLVDLAGSENIGRSGAVDKR
AREAGNINQSLLTLGRVIKALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASINLEETLSTLD
YANRAKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATRDKHGVYLSVDNYETLNGKIVSQEEQITE
YTERIAAMEDELKKIIDLFTDSKQKLEQCTEDLQDKNQRLEEAHKDLSETRHRLNQEEFISTQLQTNESH
LYNTADQLLSTAEASTQDVGGLHAKLQRKKDVELHNSKVQESFSQCMENCYNSMQTSLKEQSQKHAAMID
YYRSSVGELLNTNGKVFKETLGAVCESYSSIKGAVGEGVERCKEQVLNQEKLSQDAQNSILEILDEHKQH
LEEVLVAQAVPGIRSVMSMNDNLKQTLHKYHNLAEQMQGVKADMMTFFDAYTESLASMRECALQGFNTLR
AEHDKLKQQISQAGNSHQVRVAELVQCLQNQMNLLAVDTQNDFEGLSQAASAQIPPLETLQSSIESKCTV
AEEQAVSVRARLGSSVHGVISEMNSVTKEGERALEECAGYCGHLQTSLDSLAESGLKWCDEAKGLTESKA
QEQLKLIRQTDTAVQDLLKSVEEKSEKAVQDCEARLGQMQQEMEATLGCVEMQTSKDEATLQEHRETLSS
INTQALDTVHNFISSELRQDLPTGTTPQRKEYMYPRVLSRPRSREELEEEFRAQQEQLQSELKPCEIVME
VEEEKPVDQESLEDDVSVSSDGNNTEQSCSDENLICYENGRIPFFKKKSKKENGSKSLNRSKVENDSMST
PPRSKLPLRCQN
----------------------------------------------------------------------
>Black_Bullhead_Catfish KIF11
&#10;MSNLIPTTKKDEKGRNIQVVVRCRPFNAVERKSNAYGVVDCDSNRKEVTVRTGSTNDKTARKMYTFDMVF
GPSAKQIEVYRSVVYPILDEVIMGYNCTIFAYGQTGTGKTFTMEGERSPNEEFTWEEDPLAGVIPRTLHQ
IFEKLTSNGTEFSVKVSLLEIYNEELFDLLSPSADVSERLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQ
ILERGAAKRKTASTLMNAYSSRSHSVFSVTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRA
REAGNINQSLLTLGRVITALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETMSTLDY
ANRAKSIMNKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQILEY
TERIAAAEEDLKRVMELFTDSKQKLEQYSEDLQDKGQQLEEAHKDLQETKQKLTQEVFITTQLQTTESQL
YNAAGQLLSTAEVSTSDVGGLHAKLQRVQAVEQHNVAAQTNFAQSMEQNCSSMRSALQEHSVKHAAMLDH
YRHATAALLGGTGRVFEEAVGSVLRSYGAVREAVEEGVDGCKAKLLQQEELTKESRDAMLRMLDEHKLHL
EDALIAKAMPAIGSMMAVNEGLKQTLQRYHVLDGQMVGMKAEMTAFFSGHMAALASMRETVVQGFASLQA
EHHKLKAQMDQANRQHKTRVAQLLQCVQDQMNLLALDTQSDFKELQEASDAQGALLETLKNTMQSKCTEA
EQESSSVGARLSSCTDGVIKEMSGVAEEGRAALEEGAGYCSLLQRSLESLADSSLQWCHAARDLTERRAE
EQLTLAEENKATVDELLKRVEERGQAAVDECRTRLAPVQQEMEVLLKGVEVQTGEDQAVLQQHRDELSTL
TAQALDTVNHFLSNELQQDLPTGTTPKRKEYTYPRVLVPLRSRTELEEEFKAQQEELMSVLGQCEVLMEG
EEEKPVDQDSLEDDVCSSSDSNSTQQSISDENLMCYEEGRVPFFKKKSKKENFSSKSLNRSKVAENETSV
TPPRSRLPLRSQNQ</pre>

### Alignment

``` r
aln_lines <- readLines("MUSCLE Alignment.aln-clustalw")

cat(
  "<pre style='font-family: Courier, monospace; font-size: 7pt; line-height: 1.1;'>",
  paste(aln_lines, collapse = "\n"),
  "</pre>",
  sep = ""
)
```

    <pre style='font-family: Courier, monospace; font-size: 7pt; line-height: 1.1;'>
    CLUSTAL multiple sequence alignment by MUSCLE (3.8)
                                                                                       

    Zebrafish                   RLQLFDDPRNKRGVTIKGLEEITVHNKNEVYQILERGAAKRKTASTLMNAYSSRSHSVFS
    Catfish_Novel               -------PRVR----------------------------KRKTASTLMNAYSSRSHSVFS
    Black_Bullhead_Catfish      RLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQILERGAAKRKTASTLMNAYSSRSHSVFS
    Channel_Catfish             RLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQILERGAAKRKTASTLMNAYSSRSHSVFS
    Blue_Catfish                RLQLFDDPRNKRGVIVKGLEEITVHNKNEVYQILERGAAKRKTASTLMNAYSSRSHSVFS
    Human                       RLQMFDDPRNKRGVIIKGLEEITVHNKDEVYQILEKGAAKRTTAATLMNAYSSRSHSVFS
    Mouse                       RLQMFDDPRNKRGVIIKGLEEITVHNKDEVYQILEKGAAKRTTAATLMNAYSSRSHSVFS
                                       ** .                            **.**:***************

    Zebrafish                   VTIHMKEITLDGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIK
    Catfish_Novel               VTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
    Black_Bullhead_Catfish      VTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
    Channel_Catfish             VTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
    Blue_Catfish                VTIHMKEMTMEGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
    Human                       VTIHMKETTIDGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
    Mouse                       VTIHMKETTIDGEELVKIGKLNLVDLAGSENIGRSGAVDKRAREAGNINQSLLTLGRVIT
                                ******* *::************************************************.

    Zebrafish                   ALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASINLEETLSTLDYANRAKSIM
    Catfish_Novel               ALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDYANRAKSIM
    Black_Bullhead_Catfish      ALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETMSTLDYANRAKSIM
    Channel_Catfish             ALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDYANRAKSIM
    Blue_Catfish                ALVERGPHVPYRESKLTRILQDSLGGRTKTSIIATVSPASNNLEETLSTLDYANRAKSIM
    Human                       ALVERTPHVPYRESKLTRILQDSLGGRTRTSIIATISPASLNLEETLSTLEYAHRAKNIL
    Mouse                       ALVERTPHIPYRESKLTRILQDSLGGRTRTSIIATISPASFNLEETLSTLEYAHRAKNIM
                                ***** **:*******************.******:**** *****:***:**:***.*:

    Zebrafish                   NKPEVNQKLTKRTLIKEYTEEIERLKRDLAATRDKHGVYLSVDNYETLNGKIVSQEEQIT
    Catfish_Novel               NKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQII
    Black_Bullhead_Catfish      NKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQIL
    Channel_Catfish             NKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQII
    Blue_Catfish                NKPEVNQKLTKRTLIKEYTEEIERLKRDLAATREKHGVYLSAENYESMNSKLMSQEEQII
    Human                       NKPEVNQKLTKKALIKEYTEEIERLKRDLAAAREKNGVYISEENFRVMSGKLTVQEEQIV
    Mouse                       NKPEVNQKLTKKALIKEYTEEIERLKRDLAAAREKNGVYISEESFRAMNGKVTVQEEQIV
                                ***********.:******************:*:*:***:* :.:  :..*:  ***** 

    Zebrafish                   EYTERIAAMEDELKKIIDLFTDSKQKLEQCTEDLQDKNQRLEEAHKDLSETRHRLNQEEF
    Catfish_Novel               EYTERIAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLVEAHKDLQETKQKLTQEVF
    Black_Bullhead_Catfish      EYTERIAAAEEDLKRVMELFTDSKQKLEQYSEDLQDKGQQLEEAHKDLQETKQKLTQEVF
    Channel_Catfish             EYTERIAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLVEAHKDLQETKQKLTQEVF
    Blue_Catfish                EYTERIAAAEEELKRVMELFTDSKQKLEQYSEDLQDKGQQLEEAHKDLQETKQKLTQEVF
    Human                       ELIEKIGAVEEELNRVTELFMDNKNELDQCKSDLQNKTQELETTQKHLQETKLQLVKEEY
    Mouse                       ELVEKIAVLEEELSKATELFMDSKNELDQCKSDLQTKTQELETTQKHLQETKLQLVKEEY
                                *  *.*.. *::*..  :** *.*::*:* ..*** * * *  ::* *.**. .* :* :

    Zebrafish                   ISTQLQTNESHLYNTADQLLSTAEASTQDVGGLH
    Catfish_Novel               ITTQLQTTREQLYNAAGQLLSTAEVSTSECGRPH
    Black_Bullhead_Catfish      ITTQLQTTESQLYNAAGQLLSTAEVSTSDVGGLH
    Channel_Catfish             ITTQLQTTESQLYNAAGQLLSTAEVSTSDVGGLH
    Blue_Catfish                ITTQLQTTESQLYNAAGQLLSTAEVSTSDVGGLH
    Human                       ITSALESTEEKLHDAASKLLNTVEETTKDVSGLH
    Mouse                       VSSALERTEKTLHDTASKLLNTVKETTRAVSGLH
                                ::: *: . . *:::*.:**.*.: :*   .  *                          

                                                                                    </pre>

## Q6.

*Create a phylogenetic tree, using either a parsimony or distance-based
approach.*

<img src="phylogeny.png" style="width:90.0%"
alt="Phylogenetic tree generated from the trimmed KIF11-family multiple sequence alignment using EBI Simple Phylogeny (neighbour-joining), illustrating clustering of the novel catfish sequence with other fish KIF11 homologs." />

The tree shows the expected clustering pattern for homologous KIF11
sequences, with the novel catfish protein grouping with catfish/fish
homologs rather than with the mammalian sequences.

## Q7.

*Generate a sequence identity based heatmap of your aligned sequences
using R. *

<img src="Q7_heatmap.png" style="width:95.0%"
alt="Figure X. Sequence identity heatmap generated in R from the trimmed KIF11-family alignment using Bio3D." />

## Q8.

*Using R/Bio3D (or an online blast server if you prefer), search the
main protein structure database for the most similar atomic resolution
structures to your aligned sequences. List the top 3 unique hits
(i.e. not hits representing different chains from the same structure)
along with their Evalue and sequence identity to your query. Please also
add annotation details of these structures. For example include the
annotation terms PDB identifier (structureId), Method used to solve the
structure (experimentalTechnique), resolution (resolution), and source
organism (source).*

Searching … please wait (updates every 5 seconds) RID = VFKEC8PY014 .
Reporting 150 hits

| PDB_ID |  E.value | Identity | Method | Resolution | Source                   |
|:-------|---------:|---------:|:-------|:-----------|:-------------------------|
| 1BG2   | 4.24e-40 |   49.390 | X-ray  | 1.8        | Homo sapiens             |
| 1CZ7   | 9.77e-26 |   38.674 | X-ray  | 2.9        | Drosophila melanogaster  |
| 1F9T   | 1.02e-27 |   46.000 | X-ray  | 1.5        | Saccharomyces cerevisiae |

Top 3 PDB structure hits for the Channel Catfish KIF11 query fragment
from three different source species.

The PDB database was searched using a representative 300-amino-acid
Channel_catfish KIF11 aligned sequence fragment with Bio3D blast.pdb(),
and the top 3 unique structure hits were annotated using pdb.annotate()

The top structural matches were kinesin-family proteins solved by X-ray
crystallography from *Homo sapiens*, consistent with the query sequence
belonging to the conserved KIF11 motor protein family.

## Q9.

A structural model of the novel catfish KIF11-like protein was generated
using AlphaFold with default parameters. The model was visualized in
Mol\*, where the protein was displayed as a cartoon colored by
uncertainty score and conserved residues identified from the multiple
sequence alignment were highlighted as spacefill. They were made a
little opaque and smaller in order to be able to better visualize the
protein, as the structure is somewhat compact.

<img src="AlphaFold2%20PTM%20Model%203%20(1).png" style="width:120.0%"
alt="Figure X. AlphaFold model of the novel catfish KIF11-like protein shown as a cartoon colored by pLDDT confidence, with conserved residues identified from the multiple sequence alignment displayed as semi-transparent spacefill on a white background." />

## Q10.

*Provide an image or screen-shot of your largest predicted pockets
“negative volume” and provide it’s area and volume. Are there any Target
Associated Assays and ligand efficiency data reported that may be useful
starting points for exploring potential inhibition of your novel
protein? Briefly discuss (100 words max) the druggability of your novel
protein based on: - Presence of well-defined pockets (output of tools
like CASTpFold), - Existence of known inhibitors for related proteins
(your search of ChEMBEL), - Conservation of binding sites across
homologs (your conservation analysis in Q10), - Potential therapeutic
applications if this protein were targeted (you can use ChatGPT, Claude
etc. backed up by your reading of the literature here).*

### i.

The AlphaFold structure of the novel catfish KIF11-like protein was
analyzed with CASTpFold to identify potential ligand-binding pockets.
The largest predicted pocket is shown below. The corresponding pocket
area was 1529.785  ^2 and the pocket volume was 6314.739  ^3.

<img src="CASTpFold%20Screenshot.png" style="width:50.0%"
alt="Figure X. CASTpFold analysis of the AlphaFold model of the novel catfish KIF11-like protein showing the largest predicted binding pocket." />

### ii.

A ChEMBL Target search identified CHEMBL4581 (kinesin-like protein
KIF11) as a relevant target entry for the KIF11 family. The target
record includes associated assays, with 184 assays listed and most
classified as binding assays, as well as ligand efficiency data shown in
the ligand-efficiency plot. In addition, the target page lists
inhibitory compounds and clinical candidates such as ispinesib,
litronesib, and filanesib. Together, these results indicate that related
KIF11-family proteins have substantial assay coverage and known
small-molecule inhibitors that could inform exploration of inhibition of
the novel catfish KIF11-like protein.

### iii.

The novel catfish KIF11-like protein appears potentially druggable
because CASTpFold identified a well-defined pocket with an area of
1529.785 ^2 and a volume of 6314.739 ^3. ChEMBL data for the related
KIF11 target show associated assays, ligand-efficiency data, and known
inhibitory compounds such as ispinesib, litronesib, and filanesib,
supporting chemical tractability of this protein family. In addition,
the aligned region is strongly conserved across homologs, suggesting
functional importance of the binding region. Because KIF11-family
proteins are involved in mitotic spindle formation, inhibitors targeting
this site could have therapeutic relevance in controlling abnormal cell
proliferation.
