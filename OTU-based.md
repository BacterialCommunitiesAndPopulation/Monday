OTU-based analysis 
=====================================
Jenni Hultman

### SummaryIn Progress!

In the previous session you quality filtered your data, made contigs, created OTUs and assigned taxonomy. Now we will continue with the OTUs to figure out what does the data mean. 


## Biom table for R 

You will need to create a biom-formatted file for analysis in R-studio


```
make.biom(shared=sample.shared, constaxonomy=sample.constaxonomy)
```
## Did sequencing produce more reads for some samples?
```
mothur > count.groups(shared=stability.an.shared)
```

##Alpha diversity (more in R)
These can be created in mothur or in R. For some indices you need to rarify your data. Subsampling is one of the approaches. 
```
mothur > summary.single(shared=sample.shared, calc=nseqs-coverage-sobs-invsimpson, subsample=xxxx)
```

## Phylogenetic three for betadiversity analysis

```
mothur > dist.seqs(fasta=sample.fasta, output=lt, processors=8)
mothur > clearcut(phylip=sample.phylip.dist)

```
##Venn diagrams can be a good way to visualize the data
```
mothur > venn(shared=stability.subsample.shared, groups=A-B-C-D)
```
