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

* HISAT = Hierarchical Indexing for Spliced Alignment of Transcripts
* Fast spliced aligner with low memory requirement
* Reference genome is (BWT FM) indexed for fast searching
  - Currently Chipster offers human and mouse reference genome 
  - Let us know if you need others
  - You can provide own (small) reference genome in fasta format
* Uses two types of indexes
  * A global index: used to anchor a read in genome (28 bp is enough)
  * Thousands of small local indexes, each covering a genomic region of 56 Kbp: used for rapid extension of alignments (good for spliced read with short anchors)
* Uses splice site information found during the alignment of earlier reads in the same run

<br>

### grcm38_tran

-> tran과 snp 등등의 차이점 정리

- genome: HFM index for reference
- genome_snp: HGFM index for reference plus SNPs
- genome_tran: HGFM index for reference plus transcripts
- genome_snp_tran: HGFM index for reference plus SNPs and transcripts

<br>

### Splice-aware aligners in Chipster

1. STAR

   Human genome available

2. HISAT2

   Human and mouse genome availabel

   You can also supply own genome if it is small

3. Tophat2

   Many genomes available

   You can also supply own genome

4. Output files

   BAM = contains the alignments

   bai = index file for BAM, required by genome browsers etc

   log = useful information about the alignment run

<br>

### Command line  중 이해안간 것들

**환경변수**

```c
export PATH="/home/cylee/bin/usr/bin:$PATH"
```

환경변수란 운영체제 즉 리눅스, 윈도우, 유닉스 등에서는 모두 path환경변수가 존재한다. 

사용자의 명령을 기다리는 프롬프트 상에서 command를 내리면 그 실행파일을 OS가 찾게 되는데 어디에서 찾냐면 path에 설정되어 있는 곳에서 찾는다.

path에 셋팅된 위치에 있는 명령어는 그냥 명령어만 치면 실행이 되고,

path에 셋팅되지 않은 명령어는 명령어의 위치를 나타내는 절대경로를 사용하여야 한다.

※ path란? 사용자가 정의해놓은 길을 의미한다. path에 있는 경로를 따라가면 이런 명령어들을 실행할 수 있을거야~ 같이...

절대경로는 C:\xxx\xxx 처럼, root디렉토리에서부터 명령어(실행파일) 있는 곳까지의 경로를 다 적는 것을 말한다.

path 설정을 왜 사용하는 것일까? 물론 매번 사용자들이 절대경로를 모두 알고 일일이 사용할 때 마다 절대경로를 사용한다면 상관없지만 솔직히 귀찮지 않은가... 사용자의 편의를 위해 사용되는 것이다

예를 들어 사용자가 만든 프로그램 test1가 /usr/bin/local/test 에 있다고 가정을 할 때..

사용자 위치는 /home/testuser 이라고 가정을 하자.

사용자가 test1 명령을 수행하기 위해 /usr/bin/local/test/test1 을 수행해야 한다.

하지만 path=.;/usr/bin/local/test

라는 설정이 추가되어 있다면, test1 수행만으로 실행이 되는 것이다.

왜냐? path에서 이미 /usr/bin/local/test로 가면 test1이라는게 있다고 미리 정의해놨으니까! 길을 터놓은 셈이다.

리눅스에 예를 들어보자.

cd, ls 등 이런 명령어들은 어딘가의 경로에 디렉토리 안에서 프로그램으로 존재하고 있다. (명령어니까)

그래서 이 프로그램들을 실행하기 위해서는 각각의 절대경로를 사용해서 /bin/ls 이런식으로 일일이 써줘야지 명령어가 먹힌다. 하지만 이런것들을 모두 타이핑 하지 않고도 cd, ls 이렇게만 써줘도 사용가능한 이유가 환경변수 때문이다. 그 이유는... 환경변수에 명시되어 있기 때문이다.

자, 그렇다면 운영체제가 명령어들을 찾는 순서를 알아볼까?

/etc/profile -> /etc/bashrc -> /etc/inputrc -> $HOME/.bash_profile -> $HOME/.bashrc -> $HOME/.inputrc 이런식으로 찾아보게 된다.

 

.bashrc : 주로 alias가 등록되어 있다.

.bash_profile : path같은 환경변수가 등록되어 있다.

.bash_history : 사용한 명령어가 기록되어 있다.

.bash_logout : 로그아웃시 실행할 명령어가 기록되어 있다.

 

자, 그렇다면 환경변수 path에 내가 만든 프로그램을 실행시키고 싶어 절대경로가 아닌 그 프로그램명만 입력했을때 바로 실행되게 하려면 어떻게 해야할까?

path환경변수에 넣어주면 된다.

export PATH=[추가할경로]:$PATH를 입력하면 환경변수에 추가된다.

환경변수의 구분자는 :이다.

echo $PATH를 사용해서 지금 사용하고 있는 리눅스에 어떤 환경변수들이 저장되어 있는지 알아보자.

![img](https://t1.daumcdn.net/cfile/tistory/22796C4E567918EE06)

<br>

### HISAT2 options

- ‘-p 8’ tells HISAT2 to use eight CPUs for bowtie alignments.

- ’–dta’ Reports alignments tailored for transcript assemblers.

- --dta-cufflinks

  reports alignments tailored specifically for cufflinks

- `--dta/--downstream-transcriptome-assembly`  Report alignments tailored for transcript assemblers including StringTie. With this option, HISAT2 requires longer anchor lengths for de novo discovery of splice sites. This leads to fewer alignments with short-anchors, which helps transcript assemblers improve significantly in computation and memory usage.

- ‘-x /path/to/hisat2/index’ The HISAT2 index filename prefix (minus the trailing .X.ht2) built earlier including splice sites and exons.

- ‘-1 /path/to/read1.fastq.gz’ The read 1 FASTQ file, optionally gzip(.gz) or bzip2(.bz2) compressed.

- ‘-2 /path/to/read2.fastq.gz’ The read 2 FASTQ file, optionally gzip(.gz) or bzip2(.bz2) compressed.

- ‘-S /path/to/output.sam’ The output SAM format text file of alignments.

<br>

### alignment summary 저장

hisat2 mapping 이 끝나면 alignment summary 가 출력 되는데 이 내용이 따로 저장되지 않고 standard error ("stderr") 로 출력 됩니다. 하지만, 이 내용은 중요한 정보이기 때문에 stderr 를 저장하는 명령어를 통해 저장합니다.

저는 command 뒤에 2>&1 |tee alignmet.summary 이런식으로 지정을 해줍니다.

```c
hisat2 -p 8 -x IndexName -1 forward.fastq -2 reverse.fastq -S output.sam --dta-cufflinks 2>&1 |tee alignmet.summary
```



ex) alignment.summary 에서 overall alignment rate 계산하는 방법

37537336 reads; of these:

 37537336 (100.00%) were paired; of these:

  986245 (2.63%) aligned concordantly 0 times

  33036211 (88.01%) aligned concordantly exactly 1 time

  3514880 (9.36%) aligned concordantly >1 times

----

  986245 pairs aligned concordantly 0 times; of these:

   112422 (11.40%) aligned discordantly 1 time

----

  873823 pairs aligned 0 times concordantly or discordantly; of these:

   1747646 mates make up the pairs; of these:

​    1077358 (61.65%) aligned 0 times

​    579867 (33.18%) aligned exactly 1 time

​    90421 (5.17%) aligned >1 times



98.56% overall alignment rate

98.56% = {33036211 + 3514880 + 112422 + (579867 + 90421) / 2} / 37537336 x 100



<br>

## Alignment ; Samtools

### Description

Samtools is a set of utilities that manipulate alignments in the BAM format. It imports from and exports to the SAM (Sequence Alignment/Map) format, does sorting, merging and indexing, and allows to retrieve reads in any regions swiftly.

Samtools is designed to work on a stream. It regards an input file `-' as the standard input (stdin) and an output file `-' as the standard output (stdout). Several commands can thus be combined with Unix pipes. Samtools always output warning and error messages to the standard error output (stderr).

Samtools is also able to open a BAM (not SAM) file on a remote FTP or HTTP server if the BAM file name starts with `ftp://' or `http://'. Samtools checks the current working directory for the index file and will download the index upon absence. Samtools does not retrieve the entire alignment file unless it is asked to do so.

<br>

### Command and Options

**sort**

samtools sort [**-l** *level*] [**-m** *maxMem*] [**-o** *out.bam*] [**-O** *format*] [**-n**] [**-t** *tag*] [**-T** *tmpprefix*] [**-@** *threads*] [*in.sam*|*in.bam*|*in.cram*]

Sort alignments by leftmost coordinates, or by read name when **-n** is used. An appropriate **@HD-SO** sort order header tag will be added or an existing one updated if necessary.

The sorted output is written to standard output by default, or to the specified file (*out.bam*) when **-o** is used. This command will also create temporary files *tmpprefix*.%d**.bam** as needed when the entire alignment data cannot fit into memory (as controlled via the **-m** option).

**Options:**

- **-l** *INT*

  Set the desired compression level for the final output file, ranging from 0 (uncompressed) or 1 (fastest but minimal compression) to 9 (best compression but slowest to write), similarly to **gzip**(1)'s compression level setting.If **-l** is not used, the default compression level will apply.

- **-m** *INT*

  Approximately the maximum required memory per thread, specified either in bytes or with a **K**, **M**, or **G** suffix. [768 MiB]To prevent sort from creating a huge number of temporary files, it enforces a minimum value of 1M for this setting.

- **-n**

  Sort by read names (i.e., the **QNAME** field) rather than by chromosomal coordinates.

- **-t** *TAG*

  Sort first by the value in the alignment tag TAG, then by position or name (if also using **-n**). **-o** *FILE* Write the final sorted output to *FILE*, rather than to standard output.

- **-O** *FORMAT*

  Write the final output as **sam**, **bam**, or **cram**.By default, samtools tries to select a format based on the **-o** filename extension; if output is to standard output or no format can be deduced, **bam** is selected.

- **-T** *PREFIX*

  Write temporary files to *PREFIX***.***nnnn***.bam,** or if the specified *PREFIX* is an existing directory, to *PREFIX***/samtools.***mmm***.***mmm***.tmp.***nnnn***.bam,** where *mmm* is unique to this invocation of the **sort** command.By default, any temporary files are written alongside the output file, as *out.bam***.tmp.***nnnn***.bam,** or if output is to standard output, in the current directory as **samtools.***mmm***.***mmm***.tmp.***nnnn***.bam.**

- **-@** *INT*

  Set number of sorting and compression threads. By default, operation is single-threaded.

**Ordering Rules**

The following rules are used for ordering records.

If option **-t** is in use, records are first sorted by the value of the given alignment tag, and then by position or name (if using **-n**). For example, “-t RG” will make read group the primary sort key. The rules for ordering by tag are:



- Records that do not have the tag are sorted before ones that do.
- If the types of the tags are different, they will be sorted so that single character tags (type A) come before array tags (type B), then string tags (types H and Z), then numeric tags (types f and i).
- Numeric tags (types f and i) are compared by value. Note that comparisons of floating-point values are subject to issues of rounding and precision.
- String tags (types H and Z) are compared based on the binary contents of the tag using the C **strcmp**(3) function.
- Character tags (type A) are compared by binary character value.
- No attempt is made to compare tags of other types — notably type B array values will not be compared.

When the **-n** option is present, records are sorted by name. Names are compared so as to give a “natural” ordering — i.e. sections consisting of digits are compared numerically while all other sections are compared based on their binary representation. This means “a1” will come before “b1” and “a9” will come before “a10”. Records with the same name will be ordered according to the values of the READ1 and READ2 flags (see **flags**).

When the **-n** option is **not** present, reads are sorted by reference (according to the order of the @SQ header records), then by position in the reference, and then by the REVERSE flag.

**Note**



Historically **samtools sort** also accepted a less flexible way of specifying the final and temporary output filenames:

samtools sort [**-f**] [**-o**] *in.bam out.prefix*

This has now been removed. The previous *out.prefix* argument (and **-f** option, if any) should be changed to an appropriate combination of **-T** *PREFIX* and **-o** *FILE*. The previous **-o** option should be removed, as output defaults to standard output.



**index**

samtools index [**-bc**] [**-m** *INT*] *aln.bam*|*aln.cram* [*out.index*]

Index a coordinate-sorted BAM or CRAM file for fast random access. (Note that this does not work with SAM files even if they are bgzip compressed — to index such files, use tabix(1) instead.)

This index is needed when *region* arguments are used to limit **samtools view** and similar commands to particular regions of interest.

If an output filename is given, the index file will be written to *out.index*. Otherwise, for a CRAM file *aln.cram*, index file *aln.cram***.crai** will be created; for a BAM file *aln.bam*, either *aln.bam***.bai** or *aln.bam***.csi** will be created, depending on the index format selected.

**Options:**

- **-b**

  Create a BAI index. This is currently the default when no format options are used.

- **-c**

  Create a CSI index. By default, the minimum interval size for the index is 2^14, which is the same as the fixed value used by the BAI format.

- **-m** *INT*

  Create a CSI index, with a minimum interval size of 2^INT.

<br>

## Stringtie

**StringTie** is a fast and highly efficient assembler of RNA-Seq alignments into potential transcripts. It uses a novel network flow algorithm as well as an optional *de novo* assembly step to assemble and quantitate full-length transcripts representing multiple splice variants for each gene locus. Its input can include not only alignments of short reads that can also be used by other transcript assemblers, but also alignments of longer sequences that have been assembled from those reads. In order to identify differentially expressed genes between experiments, StringTie's output can be processed by specialized software like [Ballgown](https://github.com/alyssafrazee/ballgown), [Cuffdiff](http://cole-trapnell-lab.github.io/cufflinks/cuffdiff/index.html) or other programs (DESeq2, edgeR, etc.).

<br>

| `-e`                    | Limits the processing of read alignments to only estimate and output the assembled transcripts matching the reference transcripts given with the `-G` option (requires `-G`, recommended for `-B/-b`). With this option, read bundles with no reference transcripts will be entirely skipped, which may provide a considerable speed boost when the given set of reference transcripts is limited to a set of target genes, for example. |
| ----------------------- | ------------------------------------------------------------ |
| `-B`                    | This switch enables the output of *Ballgown* input table files (*.ctab) containing coverage data for the reference transcripts given with the -G option. (See the [Ballgown documentation](https://github.com/alyssafrazee/ballgown) for a description of these files.) With this option StringTie can be used as a direct replacement of the *tablemaker* program included with the Ballgown distribution. If the option `-o `is given as a full path to the output transcript file, StringTie will write the *.ctab files in the same directory as the output GTF. |
| `-p <int>`              | Specify the number of processing threads (CPUs) to use for transcript assembly. The default is 1. |
| `-G <ref_ann.gff>`      | Use the reference annotation file (in [GTF or GFF3 format](http://ccb.jhu.edu/software/stringtie/gff.shtml)) to guide the assembly process. The output will include expressed reference transcripts as well as any novel transcripts that are assembled. This option is required by options -B, -b, -e, -C (see below). |
| `-o [<path/>]<out.gtf>` | Sets the name of the output GTF file where StringTie will write the assembled transcripts. This can be specified as a full path, in which case directories will be created as needed. By default StringTie writes the GTF at standard output. |
| `-A <gene_abund.tab>`   | Gene abundances will be reported (tab delimited format) in the output file with the given name. |

<Br>

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

### 존스 홉킨스 대학 컴퓨터 생물학 센터 (Johns Hopkins University: Center for Computational Biology) 생물정보학 프로그램 툴 모음

**[출처]** [존스 홉킨스 대학 컴퓨터 생물학 센터 (Johns Hopkins University: Center for Computational Biology) 생물정보학 프로그램 툴 모음](https://blog.naver.com/naturelove87/221498468900)|**작성자** [랩몬LM](https://blog.naver.com/naturelove87)

<br>

## 참조

[base calling] https://chamath2.tistory.com/215

[Trimmomatic] http://www.incodom.kr/Trimmomatic/Adapter

[linux_cat] http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/cat

[genotyping] [지노타이핑](https://terms.naver.com/entry.nhn?docId=5894373) [Genotyping] (미생물학백과 )

[genotyping] https://blog.naver.com/nanohelix/220070673859

[환경변수] https://itdexter.tistory.com/251 [IT_Dexter]

[samtools] https://pd3.github.io/www.htslib.org/doc/samtools-1.7.html

[stringtie] http://ccb.jhu.edu/software/stringtie/index.shtml?t=manual