##Quality control, trimming and merging of sequencing reads
_**Working with bacterial communities and populations - 2016**_  
_Antti Karkman_


_In the sample commands everything in CAPITAL LETTERS is something you should change, so you can't just copy and paste the commands._

###QC

The first step after you get your sequences is the quality control. We will do this only for one sample to see how it works and later on do it for all samples in `mothur`.
We will first examine the quality of the sequencing reads with a program called `FastQC`, then do some quality filtering and examine the results.

So start by logging in to taito either using `PuTTY` or `ssh` on the command line.

	#command line
	ssh USERNAME@taito-shell.csc.fi

After logging in, move from your home folder to the directory where you have your sequences and load `FastQC` using the `module load` command

    module load fastqc

After that we can run the analysis with one sample. Choose any sample and do it for both R1 and R2 reads.

	#R1 reads
	fastqc YOUR_R1_READS.FASTQ 
	#R2 reads
	fastqc YOUR_R2_READS.FASTQ

FastQC outputs a html report that can be viewed in any web browser. Easiest is to download it to your own laptop and view it in your favourite browser.
You can download the file either using `WinSCP`, `scp` or any other program you like.

*Note the full-stop in the end when using `scp`. It means "here". So you copy the file to where you are. *
	
	#scp in your own laptop terminal
	scp USERNAME@taito.csc.fi:/wrk/USERNAME/SOME_FOLDER/*.html .

Have a look at the report. On the left side you can see the different quality controls measures and if they passed or not.
FastQC is more meant for shotgun sequencing data, not for amplicon data, so don't worry if some parts failed. Instead try to think why they might have failed.

Answer to the following questions in groups. After that we will go through them together before moving forward.

**1. Were there any differences in the quality scores between the forward and reverse reads?**

**2. How many sequences did you have in each file? Should they contain the same amount of reads?**

**3. Overall, do you think the quality of the sequences is sufficient and would you continue from here with the analysis?**


###Trimming

Next we will trim the same sequence files. For this we will use a program called `cutadapt`, which can trim sequencing adapters, primers and low quality regions from sequence data.
First load the biopython environment, which includes the program.

	module load biopython-env

The example sequences don't have PCR primer sequences anymore, so we don't need to trim them. And if `FastQC` didn't find any other adapters either, we can just quality trim the sequences. 
Read the [manual](http://cutadapt.readthedocs.io/en/stable/) or look at the cutadapt help for instructions how to do it:

	cutadapt -h

And since we have paired-end reads, it's probably better to do it in paired-end mode too. 
So you should find out which option to use for low quality trimming and how to do it in paired-end mode. You can choose the quality cut-off you use, but something between 15-30 probably makes sense. 

Adapters and primers are trimmed with options `-a/-b/-g` and in paired-end mode from the reverse reads with uppercase counterparts `-A/-B/-G`. What is the difference between these three options?   

To quality trim also the reverse reads in paire-end mode `cutadapt` will need an adapter to be specified for the reverse reads. Without it only the forward reads will be quality trimmed.
For this you can use a dummy adapter `XXX`. Also specify the minimum sequence length after trimming with `-m`.
    
	cutadapt -A XXX -m 10 [other options] [outfiles] [infiles]


After trimming the reads take another look at the quality scores with `FastQC`. Do you see any difference compared to the raw reads?

###Merging

As the last exercise before lunch we will merge the paired-end reads with PEAR.
If you haven't downloaded PEAR yet, go to your applications folder in taito and download the pre-compiled version from the developers website.
The `~/` refers to your home folder. And just by calling the program, you will get all the options of PEAR.

```sh
	#go to the applications folder
	cd ~/appl_taito/
	
	#download the program and unpack it
	wget http://sco.h-its.org/exelixis/web/software/pear/files/pear-0.9.6-bin-32.tar.gz
	tar -xzvf pear-0.9.6-bin-32.tar.gz 
	
	#print all the options of PEAR
	~/appl_taito/pear-0.9.6-bin-32/pear-0.9.6-bin-32
```

Now PEAR should be working and we can merge the forward and reverse reads from the same sample we used for trimming.
But, use the raw versions, not the trimmed ones. If you have time, you can try also with the trimmed ones and see how well it works.  

PEAR prints out the percentage of merged reads, from there you can see which one worked better.  
Before merging, move back to the folder where you have your sequences. 

	cd /SOME/FOLDER/
    ~/appl_taito/pear-0.9.6-bin-32/pear-0.9.6-bin-32 -f YOUR_R1_READS.FASTQ -r YOUR_R1_READS.FASTQ -o OUTPUT_PREFIX

As the last thing have a look at the `OUTPUT_PREFIX.assembled.fastq` file with `FastQC` to see how the quality of the amplicons looks like, especially the merged ovelapping part.  
And write down the number of merged reads, so that you can compare it with what you get from mothur.


