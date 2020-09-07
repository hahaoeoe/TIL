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

#### 리눅스 기본명령어- Cat

cat은 리눅스/유닉스 초보자들이 가장 많이 배우는 명령중 하나이다. cat은 concatenate 또는 catenate에서 따온 이름이다. cat 명령은 파일이름을 인자로 받아서 그 내용을 쭉 이어주는 역할을 한다. 결국 어떤 내용을 받아서 그냥 그대로 터미널 화면에 뿌려주는 역할을 한다.

```
$ cat [옵션] [파일명]
```

간단 사용법은 다음과 같다.

```
$ cat file
$ cat file1 file2 file3
```

즉 파일 한 개 이상의 내용을 화면에 출력할 때 이용한다. 한 화면을 넘어가더라도 그냥 쭉 계속해서 출력한다. 만약 내용이 한 화면이상이면 제대로 읽을 수 없기 때문에 보통 [more](http://www.incodom.kr/Linux/기본명령어/more), [less](http://www.incodom.kr/Linux/기본명령어/less) 명령어와 같이 사용한다.

두번째 사용법은

```
$ cat file(s) > file2
```

파일 여러개를 합쳐서 하나의 큰 파일을 만들 때 사용한다. 여기서 주의할 점은 만들어 지는 새로운 파일은 기존에 있는 파일이 아니라 새로운 파일이어야 한다. 만약 기존에 있는 파일이라면 기존 파일을 덮어쓰게 된다. 즉 기존 내용은 삭제되고 만다.

```
$ cat file file2 file3 > file4
```

이와 같이 사용하면 해당 파일 세 개의 파일을 모두 합쳐서 새로운 file4로 만들어 주는 것이다. file, file1, file2의 내용은 기존 내용과 달라지지 않는다.

세번째 사용법은

```
$ cat file1 >> file2
```

이 경우에는 기존에 있는 file2에 file1의 내용을 덧붙여준다. 그러면 file1의 내용이 기존 파일 file2의 맨 끝에 붙여서 새로운 파일이 생성이 된다.

네번째 사용법은

```
$ cat > new_file
```

새로운 파일을 만들 때 사용한다. 이는 touch new_file과 같은 효과를 만들지만 이 경우에는 명령어를 입력한 후에 표준 입력으로 키보드에서 입력한 내용을 파일에 저장된다. 입력이 끝나게 되면 CTRL-d를 입력하면 새로운 내용이 저장된 새로운 파일이 만들어진다. 이 경우에는 초보자가 에디터 사용에 익숙하지 않을 경우에 사용하면 유용하다.

##### 옵션

- -b: 줄번호를 화면 왼쪽에 나타낸다. 비어있는 행은 제외한다.
- -e: 제어 문자를 ^ 형태로 출력하면서 각 행의 끝에 $를 추가한다.
- -n: 줄번호를 화면 왼쪽에 나타낸다. 비어있는 행도 포함한다.
- -s: 연속되는 2개이상의 빈 행을 한행으로 출력한다.
- -v: tab과 행 바꿈 문자를 제외한 제어 문자를 ^ 형태로 출력한다.
- -E: 행마다 끝에 $ 문자를 출력한다.
- -T: 탭(tab) 문자를 출력한다.
- -A: -vET 옵션을 사용한 것과 같은 효과를 본다.

##### 팁 

출력할 파일의 내용이 너무 많아 한 화면에 다 보이지 않고 넘어가버리는 경우 [more](http://www.incodom.kr/Linux/기본명령어/more)명령어와 함께 사용하면 조금더 읽기 편해진다.

```
$ cat file | more
```

<br>

## Alignment ; HISAT2

Sequencing 단계가 끝나면 alignment와 quantification(수량화) 작업을 수행한다. Alignment 단계란 reference assembly의 한 부분으로 표준 유전체에서 read와 얼마만큼 일치하는 서열을 찾을 지(예, 100% 동일한 서열만 허용할 것인지, read의 서열 중 서로 같지 않는 부분을 한 개까지 허용할 것인지 지정)를 설정하고 일치하는 부분의 위치를 찾는 작업이다. Alignment 작업이 끝나면 전체 유전체 서열 상에 전사체 분석을 위해 만들어진 read들이 알맞은 위치에 매핑(mapping)되게 된다. 매핑된 read들의 위치와 양을 분석하여 이는 어떤 유전자들이 얼마만큼 발현되었는지를 정의하는 quantification 작업을 수행하게 된다. 

### grcm38_tran

-> tran과 snp 등등의 차이점 정리

- genome: HFM index for reference
- genome_snp: HGFM index for reference plus SNPs
- genome_tran: HGFM index for reference plus transcripts
- genome_snp_tran: HGFM index for reference plus SNPs and transcripts

<br>

## Analysis

마지막으로 Analysis(분석) 단계에서는 측정된 유전자의 종류와 발현량을 이용하여 특정조직과 유전자 간의 연관성을 밝히거나 유전자들 사이의 연관성 등을 찾는 작업을 진행한다. 



<br>

### 모르는 단어 정리

**Technical seq** 

1. adapter
2. Kmer
3. PCR

<br>

**Assembly**

read조각들을 조립하여 사람의 전체 유전 서열을 만들어내는 작업을 assembly라고 하는데, reference assembly와 de novo assembly의 두 가지 방법으로 나눌 수 있다. reference assembly는 기존 연구를 통해 알려진 표준 유전체에서  - 2 - read의 서열과 동일한 부위에 붙여 나가면서 전체 유전 서열을 완성하는 방식이고, de novo assembly는 read와 read 사이에서 겹쳐지는 서열을 이용해서 read의 길이를 늘려나가면서 전체 유전 서열을 완성 하는 방식이다.

<br>

**Genotyping; 지노타이핑**

지노타이핑(genotyping)은 비슷한 시료의 [유전자](https://terms.naver.com/entry.nhn?docId=5144908&ref=y) 염기서열(DNA sequence)이 어떻게 다른가를 알아내는 방법이다. 일반적으로 두 가지 이상의 시료로부터 유전체의 [DNA](https://terms.naver.com/entry.nhn?docId=5144868&ref=y)를 얻고 이들이 염기서열이나 유전체 구조상으로 어떻게 다른가를 확인하여 유전형적(genotype)으로 구별하고자 하는 실험법이다. 가장 정확한 비교는 유전체 전체의 염기서열을 결정하여 비교하는 것이나, 비용이나 시간적으로 간단하게 차이를 확인하고자 할 때 여러가지 실험기법을 사용하여 지노타이핑을 수행한다.

또한, 유전자 변이를 통해 발생한 질병들을 진단하기도 하며, 치료제 개발에 획기적인 정보를 제공하기도 한다. 보통 유전형질 분석에는 SNP 분석이 중요하게 작용하며 이를 통해 유전적 질병의 이해와 예방, 맞춤약물의 개발을 가능하게 한다.



<br>

## 참조

[base calling] https://chamath2.tistory.com/215

[Trimmomatic] http://www.incodom.kr/Trimmomatic/Adapter

[linux_cat] http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/cat

[genotyping] [지노타이핑](https://terms.naver.com/entry.nhn?docId=5894373) [Genotyping] (미생물학백과 )

[genotyping] https://blog.naver.com/nanohelix/220070673859

