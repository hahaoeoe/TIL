# RNA-SEQ 분석 실습 (2018)

> 강사: 부산대 박미영 박사 (Meeyoung Park, Ph.D.)
> 과정: 부산대 유전체데이터과학전공
>
> KGOL 강의를 듣고 요약한 내용입니다.



## 1강

### Transcriptome

**Transcriptome**

: "The complete set of transcripts in a cell, and their quantity, for a specific developmental stage or physiological condition" (Wang et al, 2009)

<br>

**Why do we need to understand the transcriptome**

: Interpreting the functional elements of the genome

: Revealing the molecular constituents of cells and tissues

: Understanding development and disease

<br>

**Transcriptomics**

> Transcriptome과 그 기능에 대해서 공부하는 것을 의미한다.

Aims

: To caltalogue all species of transcript including mRNAs, non-coding RNAs and small RNAs

: To determine the transcriptional structure of genes in terms of their start sites and ends, splicing patterns and other post-transcriptional modifications

: To quantify the changing expression levels of each transcipt during development and under different conditions.

<br>

**How to measure mRNA**

* *Northern blots*

: Single gene per experiment

> 전기영동을 이용하여 gene expression 측정
>
> 유전자 변화 세밀하게 측정 가능, 실험 당 하나의 mRNA만 측정 가능한 단점



* *Real Time-qPCR*

: A few genes

: Very accurate

> northern blot 보다 측정 gene 가능하지만 여전히 적은 양의 gene
>
> northern blot 보다 더 정확함

* *Microarray*

: High-throughput data

: Pre-built probes for lots of genes - relying upon existing knowlege about genome sequence

: High background levels => 실제 유전자 발현 시그널과 구별 가능하기 힘듬

: Requires complicated normalization methods 

> 획기적인 방법으로 더 많은 gene을 측정 가능
>
> 이미 알고 있는 genome seq 기반 

==> NGS 방법

<br>

* *Sequence-based measuing*

: ESTs (expressed sequencee tags) - Sanger sequencing

> relatively low throughput, expensive and generally not quantitative

: SAGE (serial analysis of gene expression)

> high throughput, but dependent on expensive Sanger sequencing technology

​                                                   ⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓⇓

RNA-Seq (RNA sequencing)

: Next-generation sequencing

: High-throughput data

: Describes the abundance and sequence of RNA transcripts

<br>

**RNA-seq Adavatages**

Detecting new transcripts

: non-model organisms with genomic sequences

: reveal sequence variations (SNPs)

: Quantigiy alternative splicing

<br>

Very low background signal

: accurate for quantifying expression levels

<br>

Requires less RNA sample

<br>

**Why RNA-seq**

What are we looking for?

: Biological pathways and mechanisms (모든 생물체에 대한  / 질병에 대한)

> Central dogma: DNA - gene - protein

: Comparisons the transciptomic profiles of two or more biological conditions

> Wild type VS. Mutant
>
> Normal VS. Disease
>
> Control VS. Knockdown

<br>

**RNA Sequencing Workflow**

RNA-Extraction

: 회사나 연구소로 보내 주로 sample을 받는다.

▼

RNA QC

: Quality check해서 제거한다. 

▼

RNA Sample Submission

▼

Order Sequencing

▼

RNA Library Preparation

▼

Sequencing

▼

Data Analysis

<br>

**Experimental Variation**

Sample variation

: Total RNA extraction (각각 다른 RNA 추출)

: Only a small fraction of nucleic acid is sampled.

<br>

Technical variation

: Variation between libraries (준비하는 과정에서 variation 발생 가능성 있음)

: Library preparation for RNA-Seq is a series of coordinated enzymatic reactions

<br>

Biological variation

: Number of biological replicates (각각 다른 생물학적 특징에 주의)

: Generally recommended at least 3~6 biological replicates per sample group

<br>

**Experimental design**

 Replicates

: Biological replicates

: Technical replicates

=> RNA seq 실험 계획 시 가장 기본 적인 것은 각 그룹당 어느정도의 sample을 이용할 것인지 정해야한다.

<br>

Sequence Depth

: 리드의 양을 얼마로 정할지 정해야한다.

: 실험자가 무엇을 보는 지에 따라 양이 달라질 수 있다.

: How many reads to sequence ('depth')

> "RNA-seq differential expression studies: more sequence or more replication?" (Liu et al., 2014)

: 차등 발현인지 새로운 유전자에 대한 실험인지 등에 따라 depth 다를 수 있다.

: https://www.ncbi.nlm.nih.gov/pmc/articels/PMC3904521

<br>

**Paired or Single-end Reads**

* Single end (SE): from each cDNA fragment only one end is read.
* Paired end (PE): the cDNA fragment is read from borh ends -: 2 reads per fragment, a given distance apart.

<br>

**Paired-end vs. Single-end sequencing**

Paired-end sequencing

: Applications

> De novo assembly => 처음으로 서열 분석 등
>
> Discovery of novel non-coding RNAs
>
> Splice isoform detection
>
> Resolution at the 3'end of your transcipt
>
> Identification of polycistronic mRNAs and operons

<br>

Single-end sequencing

: Differential expression

: Counting application

<br>

**Sequencing**

Parameters for your sequencing run will depend on your experiment.

> 각 실험에 따라 다르기 때문에 정답은 없지만 

: For differential expression profiling, between 10-25M single 1X50 or 1X100 reads.

: For de novo assembly or alternative splicing, around 100M paired 2X100 or 2X150 reads.

<br>

**Remember...**

To reduce the biological/technical biases,

: Never work without biological replicates

> Go for at least 3~4 biological replicates

: Need technial replicates

Cost-effective sequencing depth

Paired or Single-end reads

<br>

## 2강

### RNA-seq Raw Data

**Raw fastq data**

![1_fastq.PNG](https://github.com/hahaoeoe/TIL/blob/master/Bioinformatics/NGS/Image_NGS/RNA-seq%20%EB%B6%84%EC%84%9D(KGOL)/1_fastq.PNG?raw=true)

<br>

**Minimal list of Linux commands**

$ ls   => list

$ cd [dir path]  => change directory

$ mkdir [dir_name]  => make directory

$ gunzip [file_name] => uncompress

> fastq 압축 풀 때 사용

$ tar -xvf [file_ name] => uncompress

> tar 로 압축 되어 있을 때 사용

$ more  => view by page

$ chmod +x [file_name]

$ In -s [file path]  => make a symbolic link

<Br>

**Analysis flowchart**

![2_flowchart.PNG](https://github.com/hahaoeoe/TIL/blob/master/Bioinformatics/NGS/Image_NGS/RNA-seq%20%EB%B6%84%EC%84%9D(KGOL)/2_flowchart.PNG?raw=true)

: 먼저 QC를 실행해야한다. Outlier 알 수 있고 다음으로 넘어갈 수 있다.

: fragment 상태에 있는 read를 실제 genome data에 mapping 하여 allign을 해야한다. 이때, Tophat을 이용하여 aling 하여 mapping

: cufflinks/cuffmerge를 이용하여 mapping이 끝난 read를 assemble하고 하나의 transcriptome으로 merge 하게 됨

: cuffdiff를 이용하여  Differential analysis를 한다.

: Downstrame anaylsis를 위해 functional analysis라던가 machine learning 기법을 이용하여 분석한다.

<br>

**FastQC Results**

: Fred Score를 중요하게 봐야한다.

: Score 30이라는 것은 1000개의 base를 calling 했을 때 한개의 error가 나온다는 뜻 = 즉, 99.9%로 seq 됬다는 의미

: 그래프가 너무 내려오면 안좋은 것이다.

![FastQC analysis of ChIP-Seq data. FASTQ format files are derived ...](https://www.researchgate.net/profile/Hiroko_Tabunoki/publication/236639867/figure/fig1/AS:601678776659968@1520462807138/FastQC-analysis-of-ChIP-Seq-data-FASTQ-format-files-are-derived-from-short-read-NGS-data.png)

![Effects of random hexamer priming bias in RNA-Seq de novo ...](https://i.imgur.com/TfCLAXJ.png)

: Fluctuate가 심하다는 것은 seq quality에 문제가 생긴 것이다.

<br>

**Preprocessing**

Contamination

: Remove technical contamination

* Low quality read parts
* adaptors

: Remove biological contamination

* polyA-tails
* rRNA, non-coding
* mtDNA sequences

<Br>

**Trimming**

> Trim을 통해 불필요한 데이터 제거

Tools

: SolexaQA

: Trimmomatic

: ngsShort

: ConDeTri

<br>

=> trimming이 끝난 후 다시 fastqc를 돌려보고 technical하게 문제 되었던 부분들이 지워졌는 지 확인하도록 한다.

=> Techinal check가 끝난 후 본격적으로 데이터 분석을 시작한다.

<br>

**Data Analysis Pipeline**

![3_Pipeline.PNG](https://github.com/hahaoeoe/TIL/blob/master/Bioinformatics/NGS/Image_NGS/RNA-seq%20%EB%B6%84%EC%84%9D(KGOL)/3_Pipeline.PNG?raw=true)

<br>

**Reference Genome files**

Obtain a genome against which to align yout reads

: Homo_sapiens, Mus_musculus, etc..

: Convert the FASTA genome files to a special indexed format (ebwt) using  a tool provided with the Bowtie build

: Reference genome을 다운 받은 후 서버에 저장하여 Align 하여 mapping 한다.

<br>

**Tuxedo Protocol**

> https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3334321/

사용할 툴들은 tuxedo protocol에 포함된 툴들이다.

![4_Tuxedo.PNG](https://github.com/hahaoeoe/TIL/blob/master/Bioinformatics/NGS/Image_NGS/RNA-seq%20%EB%B6%84%EC%84%9D(KGOL)/4_Tuxedo.PNG?raw=true)

<br>

**TopHat**

Tuxedo protocl의 첫 번째 단계는 TopHat 단계이다.

: Fast splice junction mapper for RNA-seq reads.

: It aligns RNA-Seq reads to mammalian-sized genomes using the ultra high-throughput short read aligner Bowtie

: Analyzes the mapping results to identify splice junctions between exons.

> bowtie라는 read를 align 해주는 프로그램
>
> mapping까지 tophat을 통해 끝낼 수 있다.

<br>

**Cufflinks**

alingn된 sequence는 각 컨디션 별로 취합하여 하나의 genome으로 만든다. 이때 사용하는 것이 cufflinks

: Assembles transcipts

: Cuffmerge

* Estimates theis abundances
* 각 그룹 별로 통합하여 RNA 양 측정

: Cuffdiff

* Tests for differential expression and regulation in RNA-Seq samples
* 차등 발현된 rna양을 알 수 있다.

<Br>

**Intergrative Genomics Viewer (IGV)**

Viwer programe을 통해 시각화 할 수 있다.

<br>

File Formats

> IGV supports many different file formats

![5_file.PNG](https://github.com/hahaoeoe/TIL/blob/master/Bioinformatics/NGS/Image_NGS/RNA-seq%20%EB%B6%84%EC%84%9D(KGOL)/5_file.PNG?raw=true)

BAM이나 BED 파일을 주로 사용

<br>

**FTP download**

Windows

: Filezilla or PSFTP, & Pageant (from Putty website)

Mac

: Filezilla, Cyberduck

<Br>

