---
###Core Files
---
####RNA-Seq Data Files
The following files are the raw sequencing reads produced by the Illumina HiSeq2500 platform.  That are compressed with gzip. All the raw data for this project is located here: 

* [2M_AGTCAA_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/2M_AGTCAA_L001_R1_001.fastq.gz)

* [2M-HS_CCGTCC_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/2M-HS_CCGTCC_L001_R1_001.fastq.gz)

* [4M_AGTTCC_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/4M_AGTTCC_L001_R1_001.fastq.gz)

* [4M-HS_GTCCGC_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/4M-HS_GTCCGC_L001_R1_001.fastq.gz)

* [6M_ATGTCA_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/6M_ATGTCA_L001_R1_001.fastq.gz)

* [6M-HS_GTGAAA_L001_R1_001.fastq.gz](http://owl.fish.washington.edu/nightingales/C_gigas/6M-HS_GTGAAA_L001_R1_001.fastq.gz)

####Genome File
The following file is a FASTA file of the <em>Crassostrea gigas</em> genome, curated and maintained by Ensembl

* [Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa](http://eagle.fish.washington.edu/trilobite/Crassostrea_gigas_ensembl_tracks/Crassostrea_gigas.GCA_000297895.1.24.fa)

####Genome Annotation File
The following file is a gene transfer format ([GTF](http://uswest.ensembl.org/info/website/upload/gff.html?redirect=no)) file that contains annotation information (e.g. coding regions, intron regions, untranslated regions) about the Ensembl FASTA file.  Like the Ensembl FASTA file, this GTF file is curated and maintained by Ensembl.

* [Crassostrea_gigas.GCA_000297895.1.24.gtf](http://eagle.fish.washington.edu/trilobite/Crassostrea_gigas_ensembl_tracks/Crassostrea_gigas.GCA_000297895.1.24.gtf)

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

####Read Alignment ([TopHat](http://ccb.jhu.edu/software/tophat/index.shtml))

TopHat utilizes Bowtie to map reads to the reference genome and then identifies splice junctions between exons.

#####Output Files
TopHat outputs a human-readable text file that contains the number of input reads, the number of reads that mapped, and the number of reads with multiple alignments, called ```align_summary.txt```. The pre-heat shock TopHat analysis ```align_summary.txt``` indicates 76,612,745 input reads, while the post-heat shock shows 60,567,172 input reads. Additionally, 71.5% of the pre-heat shock reads mapped, while 67.2% of the post-heat shock reads were successfully mapped. Finally, 9.4% of the pre-heat shock reads have multiple alignments and the post-heat shock reads show 10.7% having multiple alignments.

TopHat will also produce two [BAM](http://samtools.github.io/hts-specs/SAMv1.pdf) files. This is a binary version of a Sequnce Alignment/Map (SAM) file, compressed in the BGZF format. TopHat produces the following two BAM files ```accepted_hits.bam``` and ```unmapped.bam```. Although not human-readable they are required for downstream processing in this workflow.

Additionally, three [BED](genome.ucsc.edu/FAQ/FAQformat.html#format1) files are generated: ```junctions.bed```, ```deletions.bed```, and ```insertions.bed```. These are human-readable, tab-delimited text files that store spatial information about contigs and their mapping/features within a reference genome. The pre-heat shock analysis reveals 192,994 splice junctions, 191,099 deletions, and 132,679 insertions. The post-heat shock samples identify 169,839 splice junctions, 141,387 deletions, and 109,791 insertions.

####Transcriptome Assembly & Quantification ([Cufflinks](http://cole-trapnell-lab.github.io/cufflinks/cufflinks/index.html))
Cufflinks is both a suite of software and a component of that software suite that takes a BAM file (```accepted_hits.bam```) generated by TopHat and assembles transcriptomes and quantifies their expression.

#####Output files

Cufflinks generates the following two [FPKM_Tracking](http://www.broadinstitute.org/cancer/software/genepattern/gp_guides/file-formats/sections/fpkm_tracking) files: ```genes.fpkm_tracking``` and ```isoforms.fpkm_tracking```. The FPKM files contain estimated expression values in fragments per kilobase of transcript per million (FPKM) mapped reads. The ```genes.fpkm_tracking``` file contains gene-level expression data. The file does <em>not</em> contain the "q" format found in the standard FPKM file format because there is only one input file. The ```isoforms.fpkm_tracking``` contains isoform-level expression data. The file does <em>not</em> contain the "q" format found in the standard FPKM file format because there is only one input file (```accepted_hits.bam```).

The ```genes.fpkm_tracking``` file for the pre-heat shocked data set contains 43,381 genes and 44,155 genes for the post-heat shocked data set. The ```isoforms.fpkm_tracking`` show 51,688 isoforms for the pre-heat shocked samples and 51,459isoforms for the post-heat shocked samples.

####Differential Expression ([Cuffdiff](http://cole-trapnell-lab.github.io/cufflinks/cuffdiff/index.html))
Cuffdiff determines changes in gene expression differences and identifies differentially spliced genes using an annotation file generated by Cufflinks (```trascripts.gtf```) and aligned reads produced by Cuffquant (```abundances.cxb```).

#####Output files

[Produces 16 files of the following formats](http://cole-trapnell-lab.github.io/cufflinks/cuffdiff/index.html):
* FPKM_tracking
* count_tracking
* read_group_tracking
* diff
