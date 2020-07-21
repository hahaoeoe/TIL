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

____

<br>

## FASTA / FASTQC

**FastQC**: 차세대 염기서열 데이터의 Quality Control을 진행한다. 이 때 데이터의 read length를 파악하여 추후 trimmimg 단계에서의 기준을 제공한다.



<br>

## Trimmomatic

**Trimmomatic**: 데이터를 분석하기 전 퀄리티가 낮은 read나 기준보다 짧은 read를 제거하는 작업

* [Trimmomatic](http://www.incodom.kr/Trimmomatic)은 Illumina의 시퀀싱 장비에서 생성된 NGS([Next-generation sequencing](http://www.incodom.kr/Next-generation_sequencing)) 데이터의 다양한 trimming 작업을 효과적으로 수행한다. 다양한 trimming steps은 command line에서 주어진 parameters를 기반으로 수행된다.

<br>

### **Process of trimming**

1.ILLUMINACLIP (ILLUMINACLIP:TruSeq3-PE.fa:2:30:10) : read에 존재하는 adapter sequence와 기타 illumina-specific sequence를 제거한다.

```
ILLUMINACLIP:<fastaWithAdaptersEtc>:<seed mismatches>:<palindrome clip threshold>:<simple clip threshold>
```

- FastaWithAdaptersEtc : TruSeq3-SE.fa는 install path내의 adapters directory에 존재하는 해당 adater fasta파일을 calling해 사용한다. ex) install_path/latest/adapters/TruSeq3-SE.fa
- Seed mismatches : 최초 16 bases를 seed로 놓고 이를 full match가 허용하는만큼 확장한다. full match에 허용할 mismatch의 최소값(2)
- PalindromeClipThreshold : paried-ended data의 경우 score 값(30) /약 50 bases
- SimpleClipThreshold : single-ended data의 경우 score 값(10) /약 17 bases

2.SLIDINGWINDOW (SLIDINGWINDOW:4:15) : 주어진 window(similar to mer) 상수값(4)만큼 sequence를 sliding하며 window falls (4)내의 average quality가 주어진 값(15)보다 낮을 경우 제거한다.

```
SLIDINGWINDOW:<windowSize>:<requiredQuality>
```

- windowSize: specifies the number of bases to average across
- requiredQuality: specifies the average quality required.

3.LEADING (LEADING:3): a read의 앞쪽이 주어진 threshold quality보다 낮은 경우 제거한다.

```
LEADING:<quality>
```

4.TRAILING (TRAILING:3): a read의 뒤쪽이 주어진 threshold quality보다 낮은 경우 제거한다.

```
TRAILING:<quality>
```

5.CROP : 명시된 길이만큼 read의 뒤쪽부분을 제거한다.

```
CROP:<length>
```

6.HEADCROP : 명시된 길이만큼 read의 앞쪽부분을 제거한다.

```
HEADCROP:<length>
```

7.MINLEN : 명시된 길이보다 짧을 경우 버린다.

```
MINLEN:<length>
```

8.TOPHRED33 : quality scores를 Phread-33으로 변경한다.

```
TOPHRED33 (no further parameters)
```

9.TOPHRED64 : quality scores를 Phread-64로 변경한다.

```
TOPHRED64 (no further parameters)
```

<br>

### **Input of trimmomatic** 

Trimmomatic은 [[FASTQ}}파일을 input으로 명시하고 있으며, quality는 -phred33 혹은 -phread64(default)로 command line에 명시해야한다. 이 부분은 곧 autodetected로 업데이트 될 예정이다. .gz확장자로 끝나는 경우 gzip form으로 인식하여 압축파일 자체로도 input 할 수 있다 (also .bz2).

<br>

### **Output of trimmomatic**

Single-ended data의 경우 1 input and 1 output이며, paired-ended data의 경우 2 input and 4 output이다. 4개의 output 중 2개는 모든 조건에 만족하는 clean paired-ended data 이고 나머지 2개의 output은 조건에 맞는 clean data 이지만 partener reads는 그렇지 못한 경우 clean single-ended data로 output 된다.

<br>

### Single end sequencing

If your project involved single end sequencing, then the command line you should use for Trimmomatic is the following:

```
java -jar /whereIsTrimmomatic/trimmomatic-0.36.jar SE input.fastq.gz output.fastq.gz ILLUMINACLIP:NexteraPE-PE:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:32
```

Let’s have a look on all the arguments of this command:

- **java -jar** calls the Java interpreter, as Trimmomatic is coded in Java.
- **/whereIsTrimmomatic/trimmomatic-0.36.jar** is the path to the program.
- **SE** indicates the sequencing was single end (SE).
- **input.fastq.gz** is your input file. It can be compressed (foo.fastq.gz or foo.fq.gz) or uncompressed (foo.fq or foo.fastq).
- **output.fastq.gz** is your output file. It can be compressed (foo.fastq.gz or foo.fq.gz) or uncompressed (foo.fq or foo.fastq).
- ILLUMINACLIP:NexteraPE-PE:2:30:10
  - **NexteraPE-PE** is the fasta file’s name that contain the adapters sequence (given with the program; you could also add your custom ones). You may have to specify the path to it in certain conditions. Beware, Nextera adapters (works for Nextera XT too) are always PE adapters (can be used for PE and SE).
  - **:2:30:10** are mismatch/accuracy treshold for adapter/reads pairing. See [Trimmomatic website][TRI] for more details.
- **LEADING:3** is the quality under which leading (hence the first, at the beginning of the read) nucleotide is trimmed.
- **TRAILING:3** is the quality under which trailing (hence the last, at the end of the read) nucleotide is trimmed.
- **SLIDINGWINDOW:4:15** Trimmomatic scans the reads in a **4** base window… If mean quality drops under **15**, the read is trimmed.
- **MINLEN:32** is the minimum length of trimmed/controled reads (here **32**). If the read is smaller, it is discarded.

> I usually use the settings present in this command line. You may however want to adapt according to your project’s specificity or requirements.

<br>

### Paired end sequencing

If your project involved single end sequencing, then the command line you should use for Trimmomatic is the following:

```
java -jar /whereIsTrimmomatic/trimmomatic-0.36.jar PE input_forward.fastq.gz input_reverse.fastq.gz output_forward_paired.fastq.gz output_forward_unpaired.fastq.gz output_reverse_paired.fastq.gz output_reverse_unpaired.fastq.gz ILLUMINACLIP:NexteraPE-PE.fa:2:30:10 LEADING:3 TRAILING:3 SLIDINGWINDOW:4:15 MINLEN:36
```

Let’s have a look on all the arguments of this command:

- **java -jar** calls the Java interpreter, as Trimmomatic is coded in Java.
- **/whereIsTrimmomatic/trimmomatic-0.36.jar** is the path to the program.
- **PE** indicates the sequencing was paired end (PE).
- **input_forward.fastq.gz** is your input file for read 1 (R1, typically something like XXX_S1_R1.fastq.gz). It can be compressed (foo.fastq.gz or foo.fq.gz) or uncompressed (foo.fq or foo.fastq).
- **input_reverse.fastq.gz** is your input file for read 2 (R3, typically something like XXX_S1_R2.fastq.gz). It can be compressed (foo.fastq.gz or foo.fq.gz) or uncompressed (foo.fq or foo.fastq).
- **output_forward_paired.fastq.gz** is the output file for all trimmed read 1 (R1) that are paired.
- **output_forward_unpaired.fastq.gz** is the output file for all trimmed read 1 (R1) that are unpaired (orphans).
- **output_reverse_paired.fastq.gz** is the output file for all trimmed read 2 (R2) that are paired.
- **output_reverse_unpaired.fastq.gz** is the output file for all trimmed read 2 (R2) that are unpaired (orphans).
- ILLUMINACLIP:NexteraPE-PE:2:30:10
  - **NexteraPE-PE** is the fasta file’s name that contain the adapters sequence (given with the program; you could also add your custom ones). You may have to specify the path to it in certain conditions. Beware, Nextera adapters (works for Nextera XT too) are always PE adapters (can be used for PE and SE).
  - **:2:30:10** are mismatch/accuracy treshold for adapter/reads pairing. See [Trimmomatic website][TRI] for more details.
- **LEADING:3** is the quality under which leading (hence the first, at the beginning of the read) nucleotide is trimmed.
- **TRAILING:3** is the quality under which trailing (hence the last, at the end of the read) nucleotide is trimmed.
- **SLIDINGWINDOW:4:15** Trimmomatic scans the reads in a **4** base window… If mean quality drops under **15**, the read is trimmed.
- **MINLEN:32** is the minimum length of trimmed/controled reads (here **32**). If the read is smaller, it is discarded.

> I usually use the settings present in this command line. You may however want to adapt according to your project’s specificity or requirements.



<br>

### Trimommatic attributes

* PE
* -threads 12
* -phred33

<br>

## Aligner

<br>



## 출처

[Incodom] http://www.incodom.kr/Trimmomatic

[Trimmomatic search] https://slequime.github.io/HTS-tutorial/trimming-trimmomatic.html