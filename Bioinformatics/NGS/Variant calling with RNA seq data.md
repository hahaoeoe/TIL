# Variant calling with RNA seq data 

![img](https://drive.google.com/uc?id=1yLptERtjtDzx36vqAPZCMJgvbhASdsjq)

https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-

## 관련 Tool 모음

1. Samtools
2. Picard
3. GATK
4. VEP

<br>

## VCF file 생성

1. bam file  (이미 hisat2로 mapping 끝난 결과 파일)
2. PICARD 다운로드
3. GATK 다운로드



* bam file을 samtools로 sort해서 .sorted.bam 파일 만들기



* picard MarkDuplicates 실행하기

  \>  Picard 알고리즘을 이용하여 PCR 증폭 과정에서 발생하는 중복 된 시퀀싱 리드(duplicate read)를 표시하여 제거하는 과정

  \> https://gatk.broadinstitute.org/hc/en-us/articles/360037052812-MarkDuplicates-Picard-

  

* GATK SplitNCigarReads 실행하기

  > Split Reads with N in Cigar

  \> human Reference 는 grch38_tran/genome.fa 사용

  \> GATK에서 -R (reference) 사용하기 위해서는 samtools로 genome.fa.fai 생성 및 gatk CreateSequenceDictionary를 사용해서 genome.dict 생성해야 작동한다.

  

* Picard AddOrReplaceReadGroups 실행

  \> 실행 하지 않고 바로 base quality recalibration 넘어가면 error

  

* Base Quality Recalibration을 진행해야 하지만 해당 reference에 대한 분석된 vcf가 있어야 하지만 처음 분석이라 vcf가 없으면 다음 과정으로 넘어간다.

  \>  인터넷에서 다운 받아 실행 (common_all_20180418.vcf.gz 파일 다운) 

  \> Tool로서 BaseRecalibrator를 사용한다.

  

* GATK HaplotypeCaller 실행

  \> 실행하여 vcf file을 생성

<br>

## VEP annotation 실행







<br>









<출처>

http://www.incodom.kr/GATK

https://2wordspm.com/2019/09/23/ngs-dna-seq-pipeline-gatk-best-practice-code-part1-fastq-to-bam/

https://www.nature.com/articles/s41598-020-70527-8

https://solution4u.tistory.com/13

https://blog.naver.com/gieyoung0226/221942012994