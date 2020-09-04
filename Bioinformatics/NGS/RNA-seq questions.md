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

<Br>

## FASTQC command-line (manual)

[FastQC](http://www.bioinformatics.babraham.ac.uk/projects/fastqc/) is a very simple program to run that provides inforation about sequence read quality.

The basic command looks like:

```c
$ fastqc -o RESULT-DIR INPUT-FILE.fq(.gz) ...
```

- `-o RESULT-DIR` is the directory where the result files will be written
- `INPUT-FILE.fq` is the sequence file to analyze, can be more than one file.

You can run FastQC interactively or using ht CLI, which offers the following options:

```
fastqc --help

SYNOPSIS

  fastqc seqfile1 seqfile2 .. seqfileN

  fastqc [-o output dir] [--(no)extract] [-f fastq|bam|sam] [-c contaminant file] seqfile1 .. seqfileN

OPTIONS:

  -o --outdir     Create all output files in the specified output directory.
                  Please note that this directory must exist as the program
                  will not create it.  If this option is not set then the
                  output file for each sequence file is created in the same
                  directory as the sequence file which was processed.

  --casava        Files come from raw casava output. Files in the same sample
                  group (differing only by the group number) will be analysed
                  as a set rather than individually. Sequences with the filter
                  flag set in the header will be excluded from the analysis.
                  Files must have the same names given to them by casava
                  (including being gzipped and ending with .gz) otherwise they
                  won't be grouped together correctly.

  --nano          Files come from naopore sequences and are in fast5 format. In
                  this mode you can pass in directories to process and the program
                  will take in all fast5 files within those directories and produce
                  a single output file from the sequences found in all files.

  --nofilter      If running with --casava then don't remove read flagged by
                  casava as poor quality when performing the QC analysis.

  --nogroup       Disable grouping of bases for reads >50bp. All reports will
                  show data for every base in the read.  WARNING: Using this
                  option will cause fastqc to crash and burn if you use it on
                  really long reads, and your plots may end up a ridiculous size.
                  You have been warned!

  -f --format     Bypasses the normal sequence file format detection and
                  forces the program to use the specified format.  Valid
                  formats are bam,sam,bam_mapped,sam_mapped and fastq

  -t --threads    Specifies the number of files which can be processed
                  simultaneously.  Each thread will be allocated 250MB of
                  memory so you shouldn't run more threads than your
                  available memory will cope with, and not more than
                  6 threads on a 32 bit machine

  -c              Specifies a non-default file which contains the list of
  --contaminants  contaminants to screen overrepresented sequences against.
                  The file must contain sets of named contaminants in the
                  form name[tab]sequence.  Lines prefixed with a hash will
                  be ignored.

  -a              Specifies a non-default file which contains the list of
  --adapters      adapter sequences which will be explicity searched against
                  the library. The file must contain sets of named adapters
                  in the form name[tab]sequence.  Lines prefixed with a hash
                  will be ignored.

  -l              Specifies a non-default file which contains a set of criteria
  --limits        which will be used to determine the warn/error limits for the
                  various modules.  This file can also be used to selectively
                  remove some modules from the output all together.  The format
                  needs to mirror the default limits.txt file found in the
                  Configuration folder.

 -k --kmers       Specifies the length of Kmer to look for in the Kmer content
                  module. Specified Kmer length must be between 2 and 10. Default
                  length is 7 if not specified.

 -q --quiet       Supress all progress messages on stdout and only report errors.

 -d --dir         Selects a directory to be used for temporary files written when
                  generating report images. Defaults to system temp directory if
                  not specified.
```

<Br>

## Trimmomatic

### Adapter

**Custom adapter sequence의 제작**

Custom adapter sequence를 제작하기 위해서는 Trimmomatic의 작동 전략을 이해하는 것이 유리하다. Trimmomatic은 Palindrome 과 simple 전략을 adapter trimming 에 적용하고있다. Simple trimming 은 주어진 adpater fasta 에 있는 각각의 adapter sequence 들이 NGS reads 과의 비교를 통해 유의한 수준 이상의 정확도를 보이는 경우, 해당 NGS reads 는 clipping 된다. 반면, Palindrome trimming 은 단순한 비교가 아닌 paired-end mode 에서 양쪽 서열 모두 contaminant 가 있는지 확인을 한다. Paired-end sequencing 의 경우 대부분의 경우에 3' end of reads에 adapter 서열이 존재하므로, 양쪽 서열에 이를 확인해 adapter trimming 을 수행한다. 이는 Simple trimming 보다 정확한 trimming result를 기대할 수 있다.

네이밍은 반드시 'Prefix' 이후에 /1 과 /2 로 제작되어야 한다.

<Br>

### Command line  중 이해안간 것들

```c
for i in 'cat ../FASTQ/fastq1.list'
	do
	#echo $i
	ifp=${i%_*z}
	ifpfz=${ifp}.'fp.gz'
	
	echo '---', $ifp
	#echo $i, $ifp, $ifpfz
```

* ifp=${i%_*z}

-> 의미: 

* echo '---', $ifp

<br>

### 모르는 단어 정리

Technical seq 

1. adapter
2. Kmer
3. PCR

<br>



<br>

## 참조

[base calling] https://chamath2.tistory.com/215

[Trimmomatic] http://www.incodom.kr/Trimmomatic/Adapter