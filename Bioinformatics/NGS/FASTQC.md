## FASTQC

1. SRA (NCBI) 에서 toolkit 다운

   * zip 다운 받기

   * ubuntu shell에서 tar zxvf sratoolkit.2.10.7-ubuntu64.tar.gz 

     > tar.gz 압축 풀기

   * bin 파일로 들어가기

   * chrome에서 SRR490124.sra 다운받고

   * fastq-dump로 풀기

     > fastq-cump SRR490124.sra

   * less SRR~ 해서 내용 확인하기

2. Chrome에서 Fastqc 다운해서 unzip으로 압축 풀기

3. ls -l 로 권한 확인 후 fastqc에 x가 없다면

   > chmod +x fastqc로 권한 주기

4. ./fastqc 하여 fastqc 실행하기