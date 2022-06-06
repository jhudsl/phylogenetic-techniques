# (PART\*) GETTING SEQUENCES {-}



# What sequences should I choose?

Phylogenies are only as good as the data used to infer them, so it's worth it to the spend some time carefully choosing the genomic regions and samples you will use. Good planning from the beginning will save you headaches farther downstream. 

First, you should ask yourself: **what information are you hoping to gain from the tree?** Are you hoping to reconstruct the history of organisms, or the history of a region of DNA, or the history of a protein? The answer will guide your choice of sequence and samples. 

_For closely related species in which you might hope to deduce information about the divergence between the species_, including the timing of the divergence, you would use areas of the genome known to accumulate changes rapidly (non-coding regions that also do not have functionality, or whose functionality is not easily changed by changes in the DNA base sequence).   Some examples of rapidly changing genetic regions include the mitochondrial control region, the wobble base on mitochondrial coding regions, and nuclear introns. It is also important to use “dense taxon sampling” among closely related species because any small change can seem disproportionately important in a recent divergence. Having multiple individuals sampled from each phylogenetic unit of interest (could be species, subspecies, or populations) helps to compensate by showing the genetic divergence within a group. This within-group divergence can then be accurately compared to the genetic divergence between two groups.

_For more divergent species and comparisons_, you use areas of the genome that do not change as rapidly.  For example, if you wanted to do a survey of the placental mammals, you could choose a gene region that is under enough selection pressure that it mutates more slowly than the regions you would choose for closely related species. 

_If you are examining the relationships among deeply divergent species, or when the base-pair signal is completely swamped out over time_, you might search for amino acid sequence similarities instead of DNA sequence similarities. Because of wobble, amino acid sequence can remain the same even when bases change. Sometimes amino acids of similar size/charge/shape can be substituted for others, which would result in a complete change in base pair sequence (and loss of ability to find similar sequences), but allows for finding similarities through amino acid sequence. Even when using protein sequence, it is often helpful to extract the coding sequence (once similar protein sequences have been found), because that adds extra information to fine tune the phylogenetic analysis.  This technique can also be used when the primary goal is to trace the history of a particular gene (when the changes in the gene itself are of interest.)

::: {.fyi}
To attempt a reconstruction of the evolutionary history of organisms, you really should use multiple lines of evidence and not rely solely on genomic data. For example to reconstruct primate evolution, one looks at the fossil record, molecular divergence, and also phylogeographic evidence (how these things map onto our understanding of the geography of the earth at various crucial time points along primate evolution). Examples of phylogeographic evidence include understanding when terrestrial (land-based) organisms might have been cut off from each other due to the formation of a river or lake, the eradication of a land bridge by melting glaciers and a rise in the earth’s temperature (which raises the sea level).
:::


# Finding sequences in GenBank

## Identifying a query sequence on GenBank

For this book, we will use _Glu-1_ sequences from a variety of species to infer our tree. _Glu-1_ is a gene that encodes one of the subunits used to make gluten in plants like wheat. We will use this gene to reconstruct some of the deeper phylogenetic relationships among the grasses.

We're going to temporarily leave AnVIL and RStudio and head to [NCBI's](https://www.ncbi.nlm.nih.gov/) website.

<img src="resources/images/04-ncbi_1.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />

We start by searching for _Glu-1_ sequences in the NCBI nucleotide database. At the top of the website, use the pulldown menu to choose "Nucleotide" and enter "glu-1" in the search bar.

<img src="resources/images/04-ncbi_2.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />

You might notice that this returns thousands upon thousands of possible sequences. While it's nice having choices, having _too_ many results makes it difficult to know where to start. Instead, we're going to narrow down our sequence choices by specifying that we want _Glu-1_ sequences from common wheat, or _Triticum aestivium_. This is a good starting point, since we know that common wheat plants make the gluten protein, so the genome should contain _Glu-1_.

<img src="resources/images/04-ncbi_3.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />

The first hit (at least from when this guide was created) is exactly what we're looking for - the complete coding sequence for the high molecular weight glutenin subunit, the _Glu-1_ gene. If we click on the link at the top of the entry, we can go to the GenBank page for this particular entry. 

<img src="resources/images/04-ncbi_4.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />

This page contains a lot of information about the sequence, including which research group generated it, if the sequence was used in published research, and the full taxonomy of the sample. At the top, we also find the _accession number_, or the unique ID assigned to this particular sequence. Highlight and copy the accession number - this is what we will use for our next step, a BLAST search.

## blastn

NCBI created a tool that allows us to use the **basic local alignment search tool** (**BLAST**) algorithm to find sequences similar to our query sequence (in this case, the _Triticum aestivium_ sequence we identified above). Here's a link for NCBI's web tool: [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PROGRAM=blastn&PAGE_TYPE=BlastSearch&LINK_LOC=blasthome).

There are many tutorials on how to use BLAST (including NCBI's [own](https://www.ncbi.nlm.nih.gov/Class/BLAST/blast_course.short.html)), so this section is going to focus primarily on the logic behind choosing sequences for phylogenetic analysis, not just the steps.

Once you open the BLAST webpage, you have five options for searching (the tabs at the top of the page). Which method you choose depends on your query sequence. We're going to work with two of them: _blastn_, which identifies DNA sequences that are most similar to the DNA (or nucleotide) query sequence; and _blastp_, which does the same for protein sequences.

<img src="resources/images/04-ncbi_5.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


For the blastn search, all we need to do is paste the accession code from earlier into the search box and change our program selection to somewhat similar sequences (blastn). Next, let's go down to the bottom of the page to the algorithm parameters section.


<img src="resources/images/04-ncbi_6.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


We need to change the max number of target sequences (the maximum number of sequences for our search to return). Given how rapidly the size of the GenBank databases are growing, leaving this value at 100 means we will miss a lot of sequences that we might otherwise want to see. For now, we can leave the other parameters as the default settings. The click the BLAST button on the bottom left.


<img src="resources/images/04-ncbi_7.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


It can take a couple of minutes for the blastn search to finish. When it does, a webpage similar to the figure above will open. On the right side of the screen, we have the option of applying additional filters to our search. Because we are interested in looking at the deeper phylogenetic relationships among the grass family, we don't necessarily want any additional _Triticum aestivium_ sequences, so we will filter them out. That leaves us with over 2,000 other sequences from which to choose our taxa. (If you were interested in more shallow phylogenetic relationships, choosing multiple sequences from the same taxa, or _dense taxon sampling_, is a good decision.)


<img src="resources/images/04-ncbi_8.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


There are three quality-control statistics at which we want to look. 
  *_query cover_: the amount of overlap between our query sequence and the newly-aligned sequence; larger is better
  *_E value_ (expect value): the number of hits expected by chance; like _p_-values, a lower number is better
  *_per ident_ (percent identity): the percent similarity between the two sequences; larger is better

We can filter or sort on any of these statistics. At this point we need to really look at the aligned sequences and decide which ones we want to use. 

There are quite a few samples from a variety of grass species that show good overlap, low E values, and high percent identities. Since we have options, we will prioritize choosing samples with complete coding sequence whenever possible (and avoid any sample labeled "pseudogene", since that isn't the actual _Glu-1_ gene sequence).

We will focus on these 9 sequences (in addition to the common wheat sequence we identified earlier):

  * EF105403.1, _Thinopyrum intermedium_ (intermediate wheatgrass)
  * DQ073553.1, _Leymus racemosus_ (mammoth wild rye)
  * EF204545.1, _Lophopyrum elongatum_ (tall wheatgrass)
  * AJ314771.1, _Secale cereale_ (rye)
  * FJ481569.1, _Henrardia_ (a genus of Asiatic wheatgrass)
  * DQ073533.1, _Agropyron cristatum_ (crested wheatgrass)
  * AY804128.1, _Aegilops tauschii_ (Tausch's goatgrass)
  * AY303125.2, _Taeniathetum caput_ (medusahead rye)
  * KF887414.1, _Dasypyrum villosum_ (mosquito grass)
  
A quick check of the taxonomy confirms that all of these samples are from the grass family, family Poaceae.

## Identifying an outgroup 

We have two approaches we could take for identifying an outgroup - we could use a more distantly related taxon, or we could use a homologous gene sequence from a more closely-related taxon. When we look up information about the _Poaceae_, we find there are three clades within the family - cereal grasses (like wheat), bamboos, and grasses (such as those species found in natural grasslands or cultivated for lawns and pastures). In the list of 10 related sequences above, we don't have any sequences from the bamboos (subfamily Bambusoideae). _Glu-1_ from a bamboo species might make a nice outgroup, if we can find a sequence for it.

First, we'll try another blastn search, this time setting the program selection to more dissimilar sequences (discontiguous megablast)


<img src="resources/images/04-ncbi_8.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />



When we get those results back, we can filter for samples within the subfamily Bambusoideae. Alas, we have no sequences that match.

The next thing we can try is a _blastp_ search. These searches are nice for identifying more distantly related samples, because the protein sequence of a gene changes more slowly than the nucleotide sequence. In order to run a blastp search, we need a protein sequence for our query. Luckily, we chose a full coding sequence. When we look at the GenBank entry for JX915632, we can find the coding sequence translated into the amino acids at the bottom of the page.


<img src="resources/images/04-ncbi_9.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


We can copy this amino acid sequence and paste it into the query box on the blastp page.


<img src="resources/images/04-ncbi_10.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


After the blastp search finishes and we filter out _Triticum aestivium_ results, we end up with several hundred matches. Great! ...or is it?


<img src="resources/images/04-ncbi_11.png" title="Major point!! example image" alt="Major point!! example image" style="display: block; margin: auto;" />


Unfortunately, all of the samples that are returned have very poor query coverage (less than 25%). None of these samples are likely to work for our purposes. Instead, we will have to try a homologous gene from a closely-related taxon. In our first blastn search, samples labeled "D-hordein" showed up near the bottom of the results. A Google search suggests that D-hordein is a barley homolog to the wheat _Glu-1_ gene product. This might serve nicely as an outgroup.

We will add 2 additional sequences to our list, for a total of 11:

  * D82941.1, _Hordeum vulgare_ (barley) D-hordein
  * JX276655.1, _Elymus sibiricus_ (Siberian wild rye) D-hordein


# Downloading the sequences from GenBank

Now that we have identified the sequences for our tree, we need to download those sequences from GenBank into R. One option is to download the sequences directly from GenBank as a fasta file. If you are interested in this option, [here](http://jonathancrabtree.github.io/Circleator/tutorials/gb_annotation/gb_download.html) is a good tutorial on how to do it. This will work and the subsequent fasta file can be uploaded into R.

However, the library `ape` has a command that allows us to download sequences from GenBank directly into R and store the sequences as a `DNA.bin` object. 




```r
#library(ape) #if you haven't previously loaded ape

#the first argument in the read.Genbank command is a vector
#of all the accession numbers we are using. We use the c("")
#to concatenate a string of characters that read.Genbank will
#interpret. 
read.GenBank(c("JX915632","EF105403.1","DQ073553.1",
      "FJ481575.1","EF204545.1","AJ314771.1","FJ481569.1",
      "DQ073533.1","AY804128.1","AY303125.2","KF887414.1",
      "D82941.1","JX276655.1"), as.character=F)
```

```
## 13 DNA sequences in binary format stored in a list.
## 
## Mean sequence length: 1814.385 
##    Shortest sequence: 1476 
##     Longest sequence: 2490 
## 
## Labels:
## JX915632
## EF105403.1
## DQ073553.1
## FJ481575.1
## EF204545.1
## AJ314771.1
## ...
## 
## Base composition:
##     a     c     g     t 
## 0.310 0.303 0.271 0.115 
## (Total: 23.59 kb)
```

Now that you have seen what read.Genbank does, we will
save it as an object, and also specify that we want the sequences in ATGC form (to make our lives easier downstream).


```r
#The as.characters=T argument tells R that 
#the sequences should be saved in "A,T,C,G" form.
grass <- read.GenBank(c("JX915632","EF105403.1","DQ073553.1",
      "FJ481575.1","EF204545.1","AJ314771.1","FJ481569.1",
      "DQ073533.1","AY804128.1","AY303125.2","KF887414.1",
      "D82941.1","JX276655.1"), as.character=T)
```

We even have the species information for each sample saved. This will be helpful when drawing our tree.



```r
sessionInfo()
```

```
## R version 4.0.2 (2020-06-22)
## Platform: x86_64-pc-linux-gnu (64-bit)
## Running under: Ubuntu 20.04.3 LTS
## 
## Matrix products: default
## BLAS/LAPACK: /usr/lib/x86_64-linux-gnu/openblas-pthread/libopenblasp-r0.3.8.so
## 
## locale:
##  [1] LC_CTYPE=en_US.UTF-8       LC_NUMERIC=C              
##  [3] LC_TIME=en_US.UTF-8        LC_COLLATE=en_US.UTF-8    
##  [5] LC_MONETARY=en_US.UTF-8    LC_MESSAGES=C             
##  [7] LC_PAPER=en_US.UTF-8       LC_NAME=C                 
##  [9] LC_ADDRESS=C               LC_TELEPHONE=C            
## [11] LC_MEASUREMENT=en_US.UTF-8 LC_IDENTIFICATION=C       
## 
## attached base packages:
## [1] stats     graphics  grDevices utils     datasets  methods   base     
## 
## other attached packages:
## [1] ape_5.4-1
## 
## loaded via a namespace (and not attached):
##  [1] Rcpp_1.0.8      knitr_1.33      magrittr_2.0.2  hms_0.5.3      
##  [5] lattice_0.20-41 R6_2.4.1        rlang_0.4.10    stringr_1.4.0  
##  [9] tools_4.0.2     parallel_4.0.2  grid_4.0.2      nlme_3.1-149   
## [13] xfun_0.26       jquerylib_0.1.4 htmltools_0.5.0 ellipsis_0.3.1 
## [17] ottrpal_0.1.2   yaml_2.2.1      digest_0.6.25   tibble_3.0.3   
## [21] lifecycle_1.0.0 crayon_1.3.4    bookdown_0.24   readr_1.4.0    
## [25] vctrs_0.3.4     fs_1.5.0        evaluate_0.14   rmarkdown_2.10 
## [29] stringi_1.5.3   compiler_4.0.2  pillar_1.4.6    pkgconfig_2.0.3
```
