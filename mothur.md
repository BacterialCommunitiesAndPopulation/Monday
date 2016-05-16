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

    mothur > unique.seqs(fasta=wwtp.trim.contigs.good.fasta)

Output File Names: 
wwtp.trim.contigs.good.names
wwtp.trim.contigs.good.unique.fasta

    mothur > count.seqs(name=wwtp.trim.contigs.good.names, group=wwtp.contigs.groups)




  
