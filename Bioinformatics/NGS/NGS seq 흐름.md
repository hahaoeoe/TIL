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
   
   
   
   

