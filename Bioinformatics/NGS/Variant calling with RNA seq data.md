# Variant calling with RNA seq data 

1. bam file  (이미 hisat2로 mapping 끝난 결과 파일)
2. PICARD 다운로드
3. GATK 다운로드



* bam file을 samtools로 sort해서 .sorted.bam 파일 만들기
* picard MarkDuplicates 실행하기
* GATK SplitNCigarReads 실행하기







<출처>

http://www.incodom.kr/GATK

https://2wordspm.com/2019/09/23/ngs-dna-seq-pipeline-gatk-best-practice-code-part1-fastq-to-bam/

https://www.nature.com/articles/s41598-020-70527-8

