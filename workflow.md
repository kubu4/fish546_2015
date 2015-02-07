##Workflow for reproducing differential gene expression analysis in pre- and post-heat shock mantle tissue from Crassostrea gigas (Pacific oyster).

###Illumina FASTQ Data (single-end reads)

*Pre-heat shock (gzipped fastq)*

* [2M](http://owl.fish.washington.edu/nightingales/C_gigas/2M_AGTCAA_L001_R1_001.fastq.gz)
* [4M](http://owl.fish.washington.edu/nightingales/C_gigas/4M_AGTTCC_L001_R1_001.fastq.gz)
* [6M](http://owl.fish.washington.edu/nightingales/C_gigas/6M_ATGTCA_L001_R1_001.fastq.gz)

*Post-heat shock (gzipped fastq)*

* [2M-HS](http://owl.fish.washington.edu/nightingales/C_gigas/2M-HS_CCGTCC_L001_R1_001.fastq.gz)
* [4M-HS](http://owl.fish.washington.edu/nightingales/C_gigas/4M-HS_GTCCGC_L001_R1_001.fastq.gz)
* [6M-HS](http://owl.fish.washington.edu/nightingales/C_gigas/6M-HS_GTGAAA_L001_R1_001.fastq.gz)





###Software

-[TopHat v2.0.13](http://ccb.jhu.edu/software/tophat/index.shtml)

-[FastQC v0.1](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/)

-[fastx_toolkit v0.0.13.2](http://hannonlab.cshl.edu/fastx_toolkit/index.html)

* fastx_trimmer

-[Cufflinks Package v2.2.1](http://cole-trapnell-lab.github.io/cufflinks/install/)

* Cufflinks
* Cuffmerge
* Cuffquant
* Cuffdiff
* CummeRbund

-[BLAST+ 2.2.29](ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.2.29/)

* tblastx

---

###Workflow

All commands assume I'm in the following directory

/Volumes/Data/Sam/fish546_2015/gigasHSrnaSeq

###Use FASTQC to check sequence quality

```
$fastqc --outdir=./analysis/fastqc \
/Volumes/Owl/nightingales/C_gigas/2M_AGTCAA_L001_R1_001.fastq.gz \
/Volumes/Owl/nightingales/C_gigas/2M-HS_CCGTCC_L001_R1_001.fastq.gz \
/Volumes/Owl/nightingales/C_gigas/4M_AGTTCC_L001_R1_001.fastq.gz \
/Volumes/Owl/nightingales/C_gigas/4M-HS_GTCCGC_L001_R1_001.fastq.gz \
/Volumes/Owl/nightingales/C_gigas/6M_ATGTCA_L001_R1_001.fastq.gz \
/Volumes/Owl/nightingales/C_gigas/6M-HS_GTGAAA_L001_R1_001.fastq.gz

