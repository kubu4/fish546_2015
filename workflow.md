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


*Crassostrea gigas genome (Ensembl)*

* Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome
ftp://ftp.ensemblgenomes.org/pub/metazoa/release-24/fasta/crassostrea_gigas/dna/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa.gz


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

-BLAST+ 2.2.29 ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.2.29/

* tblastx

---

##Workflow

All commands assume I'm in the following directory

/Volumes/Data/Sam/fish546_2015/gigasHSrnaSeq

###Use FASTQC to check sequence quality

```
$fastqc --outdir=./analysis/fastqc \
./raw_data/2M_AGTCAA_L001_R1_001.fastq.gz \
./raw_data/2M-HS_CCGTCC_L001_R1_001.fastq.gz \
./raw_data/4M_AGTTCC_L001_R1_001.fastq.gz \
./raw_data/4M-HS_GTCCGC_L001_R1_001.fastq.gz \
./raw_data//6M_ATGTCA_L001_R1_001.fastq.gz \
./raw_data/6M-HS_GTGAAA_L001_R1_001.fastq.gz
```

###Trim first 15 nucleotides with fastx_trimmer

Can't run on gzipped files.

gunzip files

>Should be done with a for loop
>so that the code is usable outside of this project.

```
$gunzip ./raw_data/2M_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/2M_AGTCAA_L001_R1_001.fastq
```

```
$gunzip ./raw_data/2M-HS_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/2M-HS_AGTCAA_L001_R1_001.fastq

```


```
$gunzip ./raw_data/4M_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/4M_AGTCAA_L001_R1_001.fastq

```


```
$gunzip ./raw_data/4M-HS_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/4M-HS_AGTCAA_L001_R1_001.fastq
```


```
$gunzip ./raw_data/6M_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/6M_AGTCAA_L001_R1_001.fastq
```


```
$gunzip ./raw_data/6M_AGTCAA_L001_R1_001.fastq.gz -c > \
./raw_data/6M-HS_AGTCAA_L001_R1_001.fastq
```

####Trim first 15 nucleotides

>Should be changed to for loop
>so that this isn't project specific

```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/2M_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/2M_fastx_trimmed.fastq.gz
```


```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/2M-HS_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/2MHS_fastx_trimmed.fastq.gz
```


```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/4M_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/4M_fastx_trimmed.fastq.gz
```


```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/4M-HS_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/4MHS_fastx_trimmed.fastq.gz
```


```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/6M_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/6M_fastx_trimmed.fastq.gz
```


```
$fastx_trimmer -Q33 -f 15 -z \
-i ./raw_data/6M-HS_CCGTCC_L001_R1_001.fastq \
-o ./raw_data/6MHS_fastx_trimmed.fastq.gz
```

###Use FASTQC to check post-trim sequence quality

```
fastqc --outdir=./analysis/fastqc/post_fastx_trim \
./raw_data/2M_fastx_trimmed.fastq.gz \
./raw_data/2MHS_fastx_trimmed.fastq.gz \
./raw_data/4M_fastx_trimmed.fastq.gz \
./raw_data/4MHS_fastx_trimmed.fastq.gz \
./raw_data/6M_fastx_trimmed.fastq.gz \
./raw_data/6MHS_fastx_trimmed.fastq.gz
```

###Use TopHat to assemble trimmed reads to Ensembl genome

####Build sequence index with Bowtie

```
$bowtie2-build -f ./raw_data/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome.fa \
./raw_data/Cgigas_ensembl_1.24
```

####Use TopHat to assemble pre-heat shocked samples

```
tophat2 -p 16 ./raw_data/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome \
./raw_data/2M_fastx_trimmed.fastq.gz,\
./raw_data/4M_fastx_trimmed.fastq.gz,\
./raw_data/6M_fastx_trimmed.fastq.gz
```

####Use TopHat to assemble post-heat shocked samples

```
tophat2 -p 16 ./raw_data/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome \
./raw_data/2MHS_fastx_trimmed.fastq.gz,\
./raw_data/4MHS_fastx_trimmed.fastq.gz,\
./raw_data/6MHS_fastx_trimmed.fastq.gz
```
