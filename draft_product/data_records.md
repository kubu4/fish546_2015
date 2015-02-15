---
###Core Files
---
####RNA-Seq Data Files
The following files are the raw sequencing reads produced by the Illumina HiSeq2500 platform.  That are compressed with gzip.

* 2M-HS_CCGTCC_L001_R1_001.fastq.gz

* 2M_AGTCAA_L001_R1_001.fastq.gz

* 4M-HS_GTCCGC_L001_R1_001.fastq.gz

* 4M_AGTTCC_L001_R1_001.fastq.gz

* 6M-HS_GTGAAA_L001_R1_001.fastq.gz

* 6M_ATGTCA_L001_R1_001.fastq.gz

####Genome File
The following file is a FASTA file of the <em>Crassostrea gigas</em> genome, curated and maintained by Ensembl

* Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa

####Genome Annotation File
The following file is a gene transfer format ([GTF](http://uswest.ensembl.org/info/website/upload/gff.html?redirect=no)) file that contains annotation information (e.g. coding regions, intron regions, untranslated regions) about the Ensembl FASTA file.  Like the Ensembl FASTA file, this GTF file is curated and maintained by Ensembl.

* Crassostrea_gigas.GCA_000297895.1.24.gtf

---
###Analysis Files
---

####Quality Evaluation ([FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/))

FastQC evaluates fastq files for sequence quality at each nucleotide postion. It also examines fastq files for over-represented sequences (e.g. Illumina adaptors, kmer repeats, etc) and overall GC content. 

Output files:

* HTML - This is a file that can be opened with any browser and contains graphs detailing the sequence quality of the input file at each sequencing position.
* ZIP - This file contains the following files:
  - fastqc_data_.txt - A text file representation of the HTML file.
  - summary.txt - A text file that lists the various aspects of quality assessment and whether they pass, file, or generate a warning.
  - fastqc_report.html - This is the same HTML file generated above.
  - fastqc.fo - A text file containing the raw output from the FastQC analysis
  - Icons/ - A folder containing the icons used in the HTML file that is generated.
  - Images/ - A folder cotaining the graphs used in the HTML file that is generated.

####Quality Trimming ([FASTA/Q Trimmer](http://hannonlab.cshl.edu/fastx_toolkit/))

FASTA/Q Trimmer is a component of the FASTX-Toolkit. FASTA/Q Trimmer trims input fastq files (note: it cannot process gzipped fastq files) on a range of nucleotides specified by the user.

Output files:

* FASTQ or FASTQ.GZ - Input format is not altered, but allows the user to specify if output should be gzipped or not.

####Genome Index ([Bowtie](http://bowtie-bio.sourceforge.net/))

Bowtie is used to create a set of index files, generated from a genome fasta file,to be used by TopHat for short read alignments.

Output files:
* BT2 - A binary file generated from the input genome fasta file. A set of these files are generated and the number generated is dependent on the input genome size.

####Read Alignment ([TopHat](http://ccb.jhu.edu/software/tophat/index.shtml))

TopHat utilizes Bowtie to map reads to the reference genome and then identifies splice junctions between exons.

Output files:
* align_summary.txt - A text file that contains the number of input reads, the number of reads that mapped, and the number of reads with multiple alignments.
* [BAM](http://samtools.github.io/hts-specs/SAMv1.pdf) - A binary version of a Sequnce Alignment/Map (SAM) file, compressed in the BGZF format. TopHat produces the following two BAM files:
  - accepted_hits.bam - All reads that were aligned/mapped to the reference genome.
  - unmapped.bam - All reads that did <em>not</em> align/map to the reference genome.
* [BED](genome.ucsc.edu/FAQ/FAQformat.html#format1) - A tab-delimited text file that stores spatial information about contigs and their mapping/features within a reference genome. Three columns are required for a properly formatted BED file:
  1. chromosome/contig name
  2. chromosome/contig starting position  (coordinate) within the reference genome
  3. chromosome/contig ending position (coordinate) within the reference genome 

  There are nine additional features that can be included in a BED file. 
  
  TopHat produces the following BED files:
  - deletions.bed & insertions.bed - These two files show the coordinates of nucleotide deletions/insertions discovered during mapping. This has the following five columns:
  
|Chromosome/Contig Name|Coordinate Before Deletion/Insertion|Coordinate After Deletion/Insertion|Contig Annotation|No. Reads Spanning Deletion/Insertion|
|----------------------|--------------------------|-------------------------|-----------------|------------------|

* prep_reads.info - A text file that lists the min/max length of all input reads and lists the number of input reads and the number of output reads.
* logs/ - A directory that contains a variety of log files generated during the mapping/assembly stage.

####Transcriptome Assembly & Quantification (Cufflinks)
Cufflinks is both a suite of software and a component of that software suite that takes a BAM file (```accepted_hits.bam```) generated by TopHat and assembles transcriptomes and quantifies their expression.

Output files:

* [GTF](http://uswest.ensembl.org/info/website/upload/gff.html?redirect=no) - Cufflinks generates the following two GTF files:
  - skipped.gtf - Not listed/described in the [Cufflinks](http://cole-trapnell-lab.github.io/cufflinks/cufflinks/index.html) documentation
  - transcripts.gtf - Contains the isoforms assembled by Cufflinks. The first seven columns are standard GTF and the attributes column contains some [additional information generated by Cufflinks](http://cole-trapnell-lab.github.io/cufflinks/cufflinks/index.html).
* [FPKM_Tracking](http://www.broadinstitute.org/cancer/software/genepattern/gp_guides/file-formats/sections/fpkm_tracking) - Contains estimated expression values in fragments per kilobase of transcript per million (FPKM) mapped reads. Cufflinks generates teh following two FPKM_tracking files:
  - genes.fpkm_tracking - Contains gene-level expression data. The file does <em>not</em> contain the "q" format found in the standard FPKM file format because there is only one input file.
  - isoforms.fpkm_tracking - Contains isoform-level expression data. The file does <em>not</em> contain the "q" format found in the standard FPKM file format because there is only one input file.

 ####Transcriptome Merging ([Cuffmerge](http://cole-trapnell-lab.github.io/cufflinks/cuffmerge/index.html))
 Cuffmerge merges transcriptome files (```transcripts.gtf```) generated by Cufflinks in to a "master transcriptome" file.
 
 Output files:
 
 * merged.gtf - A GTF file containing expression data of the merged transcriptomes.
 * logs/ - A directory that contains a variety of log files generated during the mapping/assembly stage.
  
####Quantify Gene & Transcriptome Expression ([Cuffquant](http://cole-trapnell-lab.github.io/cufflinks/cuffquant/index.html))
Generates gene and expression profiles from a file of alignments (```accepted_hits.bam```) and an annotation file (```Crassostrea_gigas.GCA_000297895.1.24.gtf```).

Output file:

* abundances.cxb - A binary file that can be used in the subsequent Cuffdiff or Cuffnorm step.
