###Sequence Quality Assessment

Six Illumina (HiSeq2500) gzipped FASTQ sequencing files (100bp, single-end reads; three biological replicates each, from pre- and post-heat shock) were analyzed with FASTQC (v0.11.2). To perform quality trimming, the files were uncompressed with gunzip and the first 15 bases were trimmed with FASTX Trimmer (v0.0.13.2). Upon trimming completion, the FASTQ files were re-compressed with gzip and re-evaluated with FASTQC.

###Sequence Alignment

A Bowtie (v2.1.0) sequence index was created using the Crassostrea gigas genome FASTA file from Ensembl: Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa. TopHat (v2.0.13.OSX_x86_64), using default parameters was used to align the reads to the Bowtie sequence index.  The two sets of reads (pre- and post-heat shock) were aligned separately. Each TopHat output was directed to seperate directories. The TopHat analysis produced an output file called ```accepted_hits.bam```, which was used in subsequent steps in the RNA-seq workflow.

###Sequence Annotation

Sequences were identified with blastx (v?) by comparing them to the Swiss-Prot (downloaded DATE) protein database. Annotated sequences were further annotated using gene ontologies (GO) to categorize putative functions.


###Differential Gene Expression

> Have to figure out what to for this


