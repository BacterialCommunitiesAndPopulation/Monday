#Mothur
Jenni Hultman

##Summary
mothur pipeline based on lectures by Kaisa Koskinen. See also mothur SOP http://www.mothur.org/wiki/MiSeq_SOP



    mothur > make.contigs(file=wwtp_example.txt, processors=8

Total of all groups is 1246070

Output File Names: 
wwtp.trim.contigs.fasta
wwtp.trim.contigs.qual
wwtp.contigs.report
wwtp.scrap.contigs.fasta
wwtp.scrap.contigs.qual
wwtp.contigs.groups

    mothur > summary.seqs(fasta=wwtp.trim.contigs.fasta)

Using 8 processors.

Start	  End	  NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	301	301	0	3	1
2.5%-tile:	1	431	431	0	4	31152
25%-tile:	1	460	460	0	5	311518
Median: 	1	467	467	0	5	623036
75%-tile:	1	483	483	1	5	934553
97.5%-tile:	1	506	506	11	6	1214919
Maximum:	1	602	602	260	301	1246070
Mean:	1	467.727	467.727	1.23448	5.05348
# of Seqs:	1246070

Output File Names: 
wwtp.trim.contigs.summary

It took 9 secs to summarize 1246070 sequences.


    mothur > screen.seqs(fasta=wwtp.trim.contigs.fasta, group=wwtp.contigs.groups, summary=wwtp.trim.contigs.summary, maxambig=0, maxlength=506, minlength=400)

Output File Names: 
wwtp.trim.contigs.good.summary
wwtp.trim.contigs.good.fasta
wwtp.trim.contigs.bad.accnos
wwtp.contigs.good.groups


It took 60 secs to screen 1246070 sequences.

    mothur > summary.seqs(fasta=wwtp.trim.contigs.good.fasta)

Using 8 processors.

Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	400	400	0	3	1
2.5%-tile:	1	431	431	0	4	21572
25%-tile:	1	460	460	0	5	215717
Median: 	1	466	466	0	5	431433
75%-tile:	1	482	482	0	5	647149
97.5%-tile:	1	506	506	0	6	841293
Maximum:	1	506	506	0	48	862864
Mean:	1	467.12	467.12	0	4.98364
# of Seqs:	862864

Output File Names: 
wwtp.trim.contigs.good.summary

It took 7 secs to summarize 862864 sequences.

Before screen.seqs there were 1246070 reads, thus 383206 (30%) were of bad quality (or no pairs). 

    mothur > unique.seqs(fasta=wwtp.trim.contigs.good.fasta)

Output File Names: 
wwtp.trim.contigs.good.names
wwtp.trim.contigs.good.unique.fasta

    mothur > count.seqs(name=wwtp.trim.contigs.good.names, group=wwtp.contigs.good.groups)

Using 8 processors.
It took 7 secs to create a table for 862864 sequences.


Total number of sequences: 862864

Output File Names: 
wwtp.trim.contigs.good.count_table

    mothur > summary.seqs(fasta=wwtp.trim.contigs.good.unique.fasta, count=wwtp.trim.contigs.good.count_table)

Using 8 processors.

		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	400	400	0	3	1
2.5%-tile:	1	431	431	0	4	21572
25%-tile:	1	460	460	0	5	215717
Median: 	1	466	466	0	5	431433
75%-tile:	1	482	482	0	5	647149
97.5%-tile:	1	506	506	0	6	841293
Maximum:	1	506	506	0	48	862864
Mean:	1	467.12	467.12	0	4.98364
# of unique seqs:	404102
total # of seqs:	862864

Output File Names: 
wwtp.trim.contigs.good.unique.summary

Around 50% of the reads are non-unique.

    mothur > align.seqs(fasta=wwtp.trim.contigs.good.unique.fasta, reference=/proj/hultman/KURSSI/silva.nr_v123.align)
After alingment check how the alignment looks:    

    mothur > summary.seqs(fasta=wwtp.trim.contigs.good.unique.align, count=wwtp.trim.contigs.good.count_table)
	
	Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	-1	-1	0	0	1	1
2.5%-tile:	1046	10289	430	0	4	21572
25%-tile:	1046	13125	458	0	5	215717
Median: 	1046	13125	465	0	5	431433
75%-tile:	1046	13125	481	0	5	647149
97.5%-tile:	1046	13878	505	0	6	841293
Maximum:	43116	43116	505	0	48	862864
Mean:	1119.36	13054.7	464.559	0	4.96943
# of unique seqs:	404102
total # of seqs:	862864

Output File Names: 
wwtp.trim.contigs.good.unique.summary

It took 77 secs to summarize 862864 sequences.

Filter the reads that are not from region of interest (V1-V3 of 16S rRNA gene in our case by screen.seqs. Now we can remove the reads with homopolymers as well.

	mothur > screen.seqs(fasta=wwtp.trim.contigs.good.unique.align, count=wwtp.trim.contigs.good.count_table, summary=wwtp.trim.contigs.good.unique.summary, start=1046, end=13878, maxhomop=8)
	
Output File Names: 
wwtp.trim.contigs.good.unique.good.summary
wwtp.trim.contigs.good.unique.good.align
wwtp.trim.contigs.good.unique.bad.accnos
wwtp.trim.contigs.good.good.count_table


It took 92 secs to screen 404102 sequences.

Then filter the alignment

	mothur > filter.seqs(fasta=wwtp.trim.contigs.good.unique.good.align, vertical=T, trump=.)

Length of filtered alignment: 1089
Number of columns removed: 48911
Length of the original alignment: 50000
Number of sequences used to construct filter: 21199

Output File Names: 
wwtp.filter
wwtp.trim.contigs.good.unique.good.filter.fasta

	mothur > unique.seqs(fasta=wwtp.trim.contigs.good.unique.good.filter.fasta, count=wwtp.trim.contigs.good.good.count_table)
	mothur > pre.cluster(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.fasta, count=wwtp.trim.contigs.good.unique.good.filter.count_table, diffs=2)
	
Total number of sequences before pre.cluster was 5807.
pre.cluster removed 395 sequences.

It took 8 secs to cluster 5807 sequences.
It took 13 secs to run pre.cluster.

Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.fasta
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.count_table
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER1089.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER1094.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER1100.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER1168.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER538.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER625.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER626.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER716.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER745.map
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.ER746.map

Then let's search for chimeric sequnces with uchime (see Kaisa's notes for details).

	chimera.uchime(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.fasta, count=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.count_table, dereplicate=t)
	
Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.pick.count_table
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.chimeras
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.accnos
Then we'll need to remove the chimeric sequences from the current fasta file:

	mothur > remove.seqs(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.fasta, accnos=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.accnos)
	
[WARNING]: This command can take a namefile and you did not provide one. The current namefile is wwtp.trim.contigs.good.names which seems to match wwtp.trim.contigs.good.unique.good.filter.unique.precluster.fasta.
Removed 4795 sequences from your fasta file.

Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta

Let's ummarize what we have:

	mothur > summary.seqs(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.pick.count_table)

Using 8 processors.

		Start	End	NBases	Ambigs	Polymer	NumSeqs
Minimum:	1	1087	437	0	3	1
2.5%-tile:	1	1089	452	0	4	437
25%-tile:	1	1089	456	0	4	4365
Median: 	1	1089	479	0	5	8730
75%-tile:	1	1089	485	0	5	13095
97.5%-tile:	1	1089	497	0	6	17023
Maximum:	1	1089	505	0	8	17459
Mean:	1	1089	472.851	0	4.86414
# of unique seqs:	15161
total # of seqs:	17459

Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.summary
Time to classify the reads

	mothur > classify.seqs(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.pick.count_table, reference=/proj/hultman/KURSSI/silva.bacteria/silva.bacteria.fasta, taxonomy=/proj/hultman/KURSSI/silva.bacteria/silva.bacteria.silva.tax, cutoff=80)
	
Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.silva.wang.taxonomy
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.silva.wang.tax.summary

Then we will remove the possible contaminant taxa. As the primers are for bacterial sequences, we will remove unknown, Eukaryota and Archaea.

	mothur > remove.lineage(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.fasta, count=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.pick.count_table, taxonomy=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.silva.wang.taxonomy, taxon=unknown-Eukaryota-Archaea)

	mothur > dist.seqs(fasta=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.fasta, cutoff=0.20)
	
	Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.dist

It took 255 seconds to calculate the distances for 15161 sequences.

	mothur > cluster(column=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.dist, count=wwtp.trim.contigs.good.unique.good.filter.unique.precluster.denovo.uchime.pick.pick.count_table)
Output File Names: 
wwtp.trim.contigs.good.unique.good.filter.unique.precluster.pick.pick.an.unique_list.list


