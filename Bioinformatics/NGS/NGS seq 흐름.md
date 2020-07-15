# NGS seq 흐름

1. Fastq

   : Fastqc로 quality 확인

2. Trim

   : 불필요한 부분 제거

   : Trimmomatic 사용

3. mapping

   : reference genome에 alignment 하기

   : HISAT 사용

4. read counting

   : alignment 된 read들 count 하기

   : csv 파일로 합치기 (R로 진행)

   : HTseq-count

5. DEG 분석

   : R을 통해 진행

   : DEseq2

   : DEseq2를 통해 Normalization 진행

<br>

___

#### 흐름 중 필요한 공부

1. Alignment -> Reference (.fa file) ▶Read count (Hiseq-count)

2. Normalization (DEseq2)  ▷ Youtube 시청하기

   > RPKM, FPKM, TPM

3. DEG 선별 

   > DESeq 패키지
   >
   > Log fold change ★

4. Pathway 분석

5. VCF

   > BAM file, SAM file

6. Single-end / paired end 공부필요

