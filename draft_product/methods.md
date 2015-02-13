All analyses were performed using the default paramaters for each piece of software, unless otherwise noted.

###Sequence Quality Assessment

Six Illumina (HiSeq2500) gzipped FASTQ sequencing files (100bp, single-end reads; three biological replicates each, from pre- and post-heat shock) were analyzed with FASTQC (v0.11.2). To perform quality trimming, the files were uncompressed with gunzip and the first 15 bases were trimmed with FASTX Trimmer (v0.0.13.2). Upon trimming completion, the FASTQ files were re-compressed with gzip and re-evaluated with FASTQC.

###Sequence Alignment

A Bowtie (v2.1.0) sequence index was created using the Crassostrea gigas genome FASTA file from Ensembl: Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa. TopHat (v2.0.13.OSX_x86_64), using default parameters was used to align the reads to the Bowtie sequence index.  The two sets of reads (pre- and post-heat shock) were aligned separately. Each TopHat output was directed to seperate directories. The TopHat analysis produced an output file called ```accepted_hits.bam```, which was used in subsequent steps in the RNA-seq workflow.

###Transcriptome Assembly & Quantification

Cufflinks, a component of the TopHat computing package, was used to assemble and quantify the reads from the pre- and post-heat shock, separately. The Cufflinks analysis was performed on each ```accepted_hits.bam``` file from the previous TopHat outputs. Additionally, the Cufflinks analyses utilized the Ensembl general transfer format (.gtf) file for sequence annotation information. Each analysis with Cufflinks analysis was directed to new, separate directories and generated a ```transcripts.gtf``` file to be used for combining the two transcriptomes in the next step of the workflow.

###Merge Transcriptomes

Using Cuffmerge, another component of the TopHat software, the two ```transcripts.gtf``` files from the Cufflinks process were merged together and the output file, ```merged.gtf```, was sent to a new directory. The ```merged.gtf``` file was used in the subsequent step of the analysis pipeline.

###

###Differential Gene Expression

> Have to figure out what to for this


