# RNA-seq questions

> RNA-seq 과정을 따라가면서 모르는 내용을 정리한 것입니다.

<br>

## Base calling

**Base calling** 은 염기 ( [핵산 염기](https://en.wikipedia.org/wiki/Nucleobase) )를 [크로마토 그램](https://en.wikipedia.org/wiki/Chromatogram) 피크 에 할당하는 과정이다. 이 업무를 수행하기위한 컴퓨터 프로그램 중 하나는 [Phred base-calling으로,](https://en.wikipedia.org/wiki/Phred_base_calling)이 시스템은 높은 기본 호출 정확도 때문에 학술 및 상업 DNA 시퀀싱 실험실에서 널리 사용되는 기본 소프트웨어이다.

<br>

## Prefetch & Fastq-dump

**Tools** 

* prefetch: For downloading the SRA files themselves from NCBI 
* vdb-config: Must use this to configure the toolkit and specify the location of the dbGaP private key 
* sra-validate: Tool that performs a checksum on SRA to ensure transfer of data was successful  
* fastq-dump: For converting the SRA files into the FASTQ format for easy use 
* Anisimov Launcher: Blue Waters tool that launches multiple jobs in parallel 
* Aspera: Download tool





## 참조

[base calling] https://chamath2.tistory.com/215