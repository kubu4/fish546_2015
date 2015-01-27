###Sequence Quality Assessment

Six Illumina (HiSeq2500) FASTQ sequencing files (three biological replicates each, from pre- and post-heat shock) were analyzed with FASTQC (v0.11.2).  
> Need to determine what to do with output from FASTQC - just describe the results?

> What constitutes "passing" qualities?

All FASTQ files were processed with FASTX Trimmer (v0.0.13.2) and re-evaluated with FASTQC.

###Sequence Alignment

A Bowtie (v2.1.0) sequence index was created using the Crassostrea gigas genome FASTA file from Ensembl: Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa. TopHat (v2.0.13.OSX_x86_64), using default parameters was used to align the reads to the Bowtie sequence index.  The two sets of reads (pre- and post-heat shock) were aligned separately. The TopHat output file, accepted_hits.bam, was converted to some file type

> Should I have a supplemental file that lists the default parameters?

> Should I have done separate alignments for each FASTQ file to represent the biological replicates?

> i.e. at what stage should the data be "pooled" into the two different treatments?

> Regardless, should probably align all separately to assess individual variation within treatments...

> Need to figure out what to do with all the bed files now.

> Will I be doing anything with splice junction info?

###Sequence Annotation

Sequences were identified with blastx (v?) by comparing them to the Swiss-Prot (downloaded DATE) protein database. Annotated sequences were further annotated using gene ontologies (GO) to categorize putative functions.


###Differential Gene Expression

> Have to figure out what to for this


