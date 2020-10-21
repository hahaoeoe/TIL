# Variant calling with RNA seq data 

![img](https://drive.google.com/uc?id=1yLptERtjtDzx36vqAPZCMJgvbhASdsjq)

1. bam file  (이미 hisat2로 mapping 끝난 결과 파일)
2. PICARD 다운로드
3. GATK 다운로드



* bam file을 samtools로 sort해서 .sorted.bam 파일 만들기

* picard MarkDuplicates 실행하기

* GATK SplitNCigarReads 실행하기

  \> human Reference 는 grch38_tran/genome.fa 사용

  \> GATK에서 -R (reference) 사용하기 위해서는 samtools로 genome.fa.fai 생성 및 gatk CreateSequenceDictionary를 사용해서 genome.dict 생성해야 작동한다.

* Base Quality Recalibration을 진행해야 하지만 해당 reference에 대한 분석된 vcf가 있어야 하지만 처음 분석이라 vcf가 없으면 다음 과정으로 넘어간다.

  \> Tool로서 BaseRecalibrator를 사용한다.

* 

<br>









<출처>

http://www.incodom.kr/GATK

https://2wordspm.com/2019/09/23/ngs-dna-seq-pipeline-gatk-best-practice-code-part1-fastq-to-bam/

https://www.nature.com/articles/s41598-020-70527-8

https://solution4u.tistory.com/13

https://blog.naver.com/gieyoung0226/221942012994