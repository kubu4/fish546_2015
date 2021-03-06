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

```
$for file in `ls -1 /Volumes/Owl/nightingales/C_gigas/[246]M[_-]*.gz`
do
    newname=`basename $file | sed -e "s/.gz//"`
    gunzip -c $file > /Volumes/Data/Sam/testing/$newname
    echo $file
    echo $newname
done
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

####Use TopHat to map reads from  pre-heat shocked samples

```
tophat2 -p 16 ./raw_data/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome \
./raw_data/2M_fastx_trimmed.fastq.gz,\
./raw_data/4M_fastx_trimmed.fastq.gz,\
./raw_data/6M_fastx_trimmed.fastq.gz
```

####Use TopHat to map reads from post-heat shocked samples

```
tophat2 -p 16 ./raw_data/Crassostrea_gigas.GCA_000297895.1.24.dna_sm.genome \
./raw_data/2MHS_fastx_trimmed.fastq.gz,\
./raw_data/4MHS_fastx_trimmed.fastq.gz,\
./raw_data/6MHS_fastx_trimmed.fastq.gz
```

###Use Cufflinks to assemble and transcriptomic data

```
$cufflinks ./analysis/tophat_preHS/tophat_out/accepted_hits.bam
```


```
$cufflinks ./analysis/tophat_postHS/tophat_out/accepted_hits.bam
```

###Use Cuffmerge to assemble both transciptomes into a single reference transcriptome

####Create Cuffmerge manifest
Cuffmerge requires full paths to the transcripts.gtf files in the Cuffmerge manifest

```
$find `pwd` -name "transcripts.gtf" >> ./analysis/cuffmerge/cuffmerge_manifest.txt
```

####Run Cuffmerge

```
$cuffmerge -p 16 ./analysis/cuffmerge/cufflinksManifest.txt
```

###Use Cuffquant to quantify gene and transcript expression

```
$cuffquant -p 16 ./analysis/cuffmerge/merged_asm/merged.gtf \
./analysis/tophat_preHS/tophat_out/accepted_hits.bam
```

```
$cuffquant -p 16 ./analysis/cuffmerge/merged_asm/merged.gtf \
./analysis/tophat_postHS/tophat_out/accepted_hits.bam
```
