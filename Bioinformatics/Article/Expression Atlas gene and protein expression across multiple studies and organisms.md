# Expression Atlas: gene and protein expression across multiple studies and organisms

>Irene Papatheodorou1,*, Nuno A. Fonseca1, Maria Keays1, Y. Amy Tang1, Elisabet Barrera1, Wojciech Bazant1, Melissa Burke1, Anja Fullgrabe ¨ 1, Alfonso Munoz-Pomer Fuentes ˜ 1, Nancy George1, Laura Huerta1, Satu Koskinen1, Suhaib Mohammed1, Matthew Geniza2, Justin Preece2, Pankaj Jaiswal2, Andrew F. Jarnuczak1, Wolfgang Huber1, Oliver Stegle1, Juan Antonio Vizcaino1, Alvis Brazma1 and Robert Petryszak1
>
>1European Molecular Biology Laboratory, European Bioinformatics Institute, EMBL-EBI, Hinxton, UK and 2Oregon State University, Corvallis, USA Received September 15, 2017; 
>
>Revised October 26, 2017; Editorial Decision October 27, 2017; Accepted November 06, 2017
>
>읽고 요약한 내용입니다.

<br>

## Abstract

Expression Atlas는 tissue, developmental stage, disease 또는 cell type과 같은 다양한 종과 contexts의 gene과 protein expression에 대한 정보를 제공한다. 

모든 가공된 data matrices는  tab-delimited format이나 R data로 다운 가능하다.

<br>

## Introduction

Expression Atlas의 목표는 다양한 organisms으로 부터 제공된 데이터 세트들로 구성된 커다란 reaearch community를 제공하는 것이다. 

Expression Atlas는 microarray와 RNA-seq 데이터 세트를 아래 기간에서 부터 제공받아 사용한다.

* ArrayExpress
* NCBI's Gene Expression Onibus(GEO)
* European Nucleotide Archive (ENA)

Mass spectrometry proteomics data는 아래 기관에서 제공받았다. 

* PRoteomics IDEntification(PRIDE)

<br>

Gene expression data set가 Expression Atlas에 포함되는 기준은

1. the study must be of general interest
2. it must include at least three biological replicates and
3. clear experimental variables must be available to enable re-analysis.

<br>

Expression Atlas data sets는 두가지로 분류된다.

* *baseline* 
* *differential*

________

The baseline data sets report transcript or protein abundance typically in constitutive conditions such as healthy tissues, cell types, developmental stages or cell lines.

Differential studies report changes in expression between two different conditions, such as healthy and diseased tissue.

<br>

## Results

#### Data and analysis

##### Data dissemination

모든 data sets는 FTP를 통해 링크로 웹사이트에서 다운 받을 수 있다.

<br>

##### Expression Atlas R object

Bioconductor에 R 패키지를 출시하여 사용자는 사전 패키지 된 데이터를 모두 검색하고 다운로드 할 수 있습니다. 

R 세션에 대한 Expression Atlas 데이터베이스 연구 (https://www.bioconductor.org/packages/release/bioc/html / ExpressionAtlas.html). 

<br>

##### Data analysis

Data analysis. Since the last update, there has been a change in the way RNA-Seq data sets are retrieved for processing in Expression Atlas. All RNA-Seq data sets from ENA, including the data from the Sequence Read Archive, SRA (14), corresponding to species covered by Expression Atlas are processed automatically using the open source iRAP pipeline and made available via the RNAseq-er Application Programming Interface (API) (15). This first level processing includes FASTQ QC, alignment, mapping QC and gene expression quantification. Selected experiments are then curated for dissemination in Expression Atlas and processed further downstream using quantile normalization followed by co-expression analyses (if baseline) or differential expression analysis, gene ontology and pathway enrichment (if differential). In addition, the Expression Atlas team actively looks for other RNA-Seq data sets that are not included in ENA for various reasons, such as controlled access, and try to obtain permissions to re-analyze and disseminate the processed data from these, and include them in the Atlas. The tools employed for genome alignment, quantification and differential expression (Tophat2 (16)/Star (17), HTSeq (18), DESeq2 (19)) remain the same, as described in the previous update (1), although they have been updated to their latest versions. Additionally, users can now explore and download gene expression quantification values in TPM (transcripts per million) units as well as in FPKM (fragments per kilobase of exon model per million reads mapped).

<br>

