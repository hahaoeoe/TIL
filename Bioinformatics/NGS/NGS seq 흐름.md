# NGS seq 흐름

1. Fastq

   : Fastqc로 quality 확인

2. Trim

   : 불필요한 부분 제거

   : Trimmomatic 사용

3. mapping

   : reference genome에 alignment 하기

   : HISAT 사용 

   : samtools로 sam -> bam 변환

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

1. Basic Statistics

   ![Quality control: Assessing FASTQC results | Introduction to RNA ...](https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon/img/fastqc_basic_stats.png)

   * Filename: The original filename of the file which was analysed 
   * File type: Says whether the file appeared to contain actual base calls or colorspace data which had to be converted to base calls 
   * Encoding: Says which ASCII encoding of quality values was found in this file.
   * Total Sequences: A count of the total number of sequences processed. There are two values reported, actual and estimated. At the moment these will always be the same. In the future it may be possible to analyse just a subset of sequences and estimate the total number, to speed up the analysis, but since we have found that problematic sequences are not evenly distributed through a file we have disabled this for now. 
   * Filtered Sequences: If running in Casava mode sequences flagged to be filtered will be removed from all analyses. The number of such sequences removed will be reported here. The total sequences count above will not include these filtered sequences and will the number of sequences actually used for the rest of the analysis. 
   * Sequence Length: Provides the length of the shortest and longest sequence in the set. If all sequences are the same length only one value is reported. 
   * %GC: The overall %GC of all bases in all sequences

   <br>

2. Per base sequence quality

   ![Quality scores per base summarized over all reads plotted by ...](https://www.researchgate.net/profile/Yonggang_Zhou4/publication/277727592/figure/fig4/AS:269528346173442@1441271973326/Quality-scores-per-base-summarized-over-all-reads-plotted-by-FastQC-Reads-were-trimmed.png)

   * The central red line is the median value 

   * The yellow box represents the inter-quartile range (25-75%) 

   * The upper and lower whiskers represent the 10% and 90% points 

   * The blue line represents the mean quality

   * The y-axis on the graph shows the quality scores. The higher the score the better the base call. The background of the graph divides the y axis into very good quality calls (green), calls of reasonable quality (orange), and calls of poor quality (red).

     <br>

3. Per sequence quality scores

   <img src="https://www.openbioinformatics.org/seqmule/example/trio_report/father/father.2_phred33_fastqc/Images/per_sequence_quality.png" alt="father.2_phred33.fastq FastQC Report" style="zoom: 67%;" />

   The per sequence quality score report allows you to see if a subset of your sequences have universally low quality values

   <br>

4. Per base sequence content

   <img src="https://i.imgur.com/hxUW2Cn.png" alt="High A in &quot;Per base sequence content&quot; of fastQC report" style="zoom: 67%;" />

   * Per Base Sequence Content plots out the proportion of each base position in a file for which each of the four normal DNA bases has been called.

   * 랜덤 라이브러리에서는 시퀀스 실행의 서로 다른 기준간에 거의 또는 전혀 차이가 없을 것으로 예상되므로 이 플롯의 선은 서로 평행하게 실행되어야 한다. 각 염기의 상대적인 양은 게놈에서 이러한 염기의 전체적인 양을 반영해야하지만, 어떤 경우에도 서로 크게 불균형해서는 안된다.

   * 다른 염기에서 변화하는 강한 치우침이 나타나는 경우, 이는 일반적으로 라이브러리를 오염시키는 과다 표현 된 시퀀스를 나타낸다. 모든 염기에 걸쳐 일관된 편향은 원래 라이브러리가 서열 편향되었거나 라이브러리 시퀀싱 중에 체계적인 문제가 있음을 나타냅니다.

     <br>

5. Per sequence GC content

   <img src="https://i.ibb.co/WsDF3JP/per-sequence-gc-content.png" alt="How to interpret Per sequence GC content module in FastQC for RNA ..." style="zoom:50%;" />

   * This module measures the GC content across the whole length of each sequence in a file and compares it to a modelled normal distribution of GC content.

     <br>

6. Per base N content

   <img src="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/per_base_n_content.png" alt="Per Base N Content" style="zoom: 50%;" />

   * If a sequencer is unable to make a base call with sufficient confidence then it will normally substitute an N rather than a conventional base] call This module plots out the percentage of base calls at each position for which an N was called.

     <br>

7. Sequence Length Distribution

   <img src="https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/sequence_length_distribution.png" alt="Sequence Length Distribution" style="zoom:67%;" />

   * This module generates a graph showing the distribution of fragment sizes in the file which was analysed.

   * In many cases this will produce a simple graph showing a peak only at one size, but for variable length FastQ files this will show the relative amounts of each different size of sequence fragment.

     <br>

8. Sequence Duplication Levels

   <img src="https://image.ibb.co/g1Eu9U/image.png" alt="Illumina HiSeq generates abnormally high duplication levels and ..." style="zoom:67%;" />

   * This module counts the degree of duplication for every sequence in the set and creates a plot showing the relative number of sequences with different degrees of duplication

     <br>

9. Overrepresented sequences

   <img src="https://i.imgur.com/AYv7nyq.png" alt="3' Tag-Seq / DE Analysis - FASTQC detects overrepresented ..." style="zoom: 25%;" />

   

10. Adapter Content

    <img src="https://galaxyproject.github.io/training-material/topics/sequence-analysis/images/quality-control/adapter_content.png" alt="Quality Control" style="zoom:50%;" />

11. Kmer Content

    ![kmer content failling with fastqc](https://preview.ibb.co/gfAMbT/kmer.png)

    <br>

=> 주로 보는 part는 **Basic Statistics / Per base sequence quality / Sequence Length Distribution / Adapter Content** 이다. 

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

## Alignment

> HISAT2를 이용하여 alignment 후 samtools로 fastq 파일을 sam/bam으로 변환한다.

<br>

### HISAT2

HISAT2 is a fast and sensitive alignment program for mapping next-generation sequencing reads (whole-genome, transcriptome, and exome sequencing data) against the general human population (as well as against a single reference genome).

<br>

#### Running HISAT2

**Adding to PATH**

By adding your new HISAT2 directory to your [PATH environment variable](http://en.wikipedia.org/wiki/PATH_(variable)), you ensure that whenever you run `hisat2`, `hisat2-build` or `hisat2-inspect` from the command line, you will get the version you just installed without having to specify the entire path. This is recommended for most users. To do this, follow your operating system's instructions for adding the directory to your [PATH](http://en.wikipedia.org/wiki/PATH_(variable)).



```c
echo "hisat2 -p 8 --dta-cufflinks -x $index_path/genome_tran -1 $input_path$i_paired_1 -2 $input_path$i_paired_2 -S $i_sam"
echo "hisat2 -p 8 --dta-cufflinks -x $index_path/genome_tran -1 $input_path$i_paired_1 -2 $input_path$i_paired_2 -S $i_sam" >> $ log_file
hisat2 -p 8 --dta-cufflinks -x $index_path/genome_tran -1 $input_path$i_paired_1 -2 $input_path$i_paired_2 --summary-file $log_temp -S $i_sam
```

* -p : 스레드 수를 지정하는 데 사용된다.
* --dta-cufflinks : Report alignments tailored specifically for Cufflinks.
* -x : 참조 게놈에 해당하는 색인 파일을 지정하는 데 사용된다.
* -1 / -2 : 시퀀싱 읽기가 있는 파일을 지정하는 데 사용된다.
* -S : 저장 영역 비교 결과를 지정하는 데 사용되는 파일 이름



<br>

#### Usage

```
hisat2 [options]* -x <hisat2-idx> {-1 <m1> -2 <m2> | -U <r> | --sra-acc <SRA accession number>} [-S <hit>]
```

#### Main arguments

| `-x <hisat2-idx>`                  | The basename of the index for the reference genome. The basename is the name of any of the index files up to but not including the final `.1.ht2` / etc. `hisat2` looks for the specified index first in the current directory, then in the directory specified in the `HISAT2_INDEXES` environment variable. |
| ---------------------------------- | ------------------------------------------------------------ |
| `-1 <m1>`                          | Comma-separated list of files containing mate 1s (filename usually includes `_1`), e.g. `-1 flyA_1.fq,flyB_1.fq`. Sequences specified with this option must correspond file-for-file and read-for-read with those specified in `<m2>`. Reads may be a mix of different lengths. If `-` is specified, `hisat2` will read the mate 1s from the "standard in" or "stdin" filehandle. |
| `-2 <m2>`                          | Comma-separated list of files containing mate 2s (filename usually includes `_2`), e.g. `-2 flyA_2.fq,flyB_2.fq`. Sequences specified with this option must correspond file-for-file and read-for-read with those specified in `<m1>`. Reads may be a mix of different lengths. If `-` is specified, `hisat2` will read the mate 2s from the "standard in" or "stdin" filehandle. |
| `-U <r>`                           | Comma-separated list of files containing unpaired reads to be aligned, e.g. `lane1.fq,lane2.fq,lane3.fq,lane4.fq`. Reads may be a mix of different lengths. If `-` is specified, `hisat2` gets the reads from the "standard in" or "stdin" filehandle. |
| `--sra-acc <SRA accession number>` | Comma-separated list of SRA accession numbers, e.g. `--sra-acc SRR353653,SRR353654`. Information about read types is available at http://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?sp=runinfo&acc=**sra-acc**&retmode=xml, where **sra-acc** is SRA accession number. If users run HISAT2 on a computer cluster, it is recommended to disable SRA-related caching (see the instruction at [SRA-MANUAL](https://github.com/ncbi/sra-tools/wiki/Toolkit-Configuration)). |
| `-S <hit>`                         | File to write SAM alignments to. By default, alignments are written to the "standard out" or "stdout" filehandle (i.e. the console). |

#### Options

##### Input options

| `-q`               | Reads (specified with `<m1>`, `<m2>`, `<s>`) are FASTQ files. FASTQ files usually have extension `.fq` or `.fastq`. FASTQ is the default format. See also: [`--solexa-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-solexa-quals) and [`--int-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-int-quals). |
| ------------------ | ------------------------------------------------------------ |
| `--qseq`           | Reads (specified with `<m1>`, `<m2>`, `<s>`) are QSEQ files. QSEQ files usually end in `_qseq.txt`. See also: [`--solexa-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-solexa-quals) and [`--int-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-int-quals). |
| `-f`               | Reads (specified with `<m1>`, `<m2>`, `<s>`) are FASTA files. FASTA files usually have extension `.fa`, `.fasta`, `.mfa`, `.fna` or similar. FASTA files do not have a way of specifying quality values, so when `-f` is set, the result is as if `--ignore-quals` is also set. |
| `-r`               | Reads (specified with `<m1>`, `<m2>`, `<s>`) are files with one input sequence per line, without any other information (no read names, no qualities). When `-r` is set, the result is as if `--ignore-quals` is also set. |
| `-c`               | The read sequences are given on command line. I.e. `<m1>`, `<m2>` and `<singles>` are comma-separated lists of reads rather than lists of read files. There is no way to specify read names or qualities, so `-c` also implies `--ignore-quals`. |
| `-s/--skip <int>`  | Skip (i.e. do not align) the first `<int>` reads or pairs in the input. |
| `-u/--qupto <int>` | Align the first `<int>` reads or read pairs from the input (after the [`-s`/`--skip`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-s) reads or pairs have been skipped), then stop. Default: no limit. |
| `-5/--trim5 <int>` | Trim `<int>` bases from 5' (left) end of each read before alignment (default: 0). |
| `-3/--trim3 <int>` | Trim `<int>` bases from 3' (right) end of each read before alignment (default: 0). |
| `--phred33`        | Input qualities are ASCII chars equal to the [Phred quality](http://en.wikipedia.org/wiki/Phred_quality_score) plus 33. This is also called the "Phred+33" encoding, which is used by the very latest Illumina pipelines. |
| `--phred64`        | Input qualities are ASCII chars equal to the [Phred quality](http://en.wikipedia.org/wiki/Phred_quality_score) plus 64. This is also called the "Phred+64" encoding. |
| `--solexa-quals`   | Convert input qualities from [Solexa](http://en.wikipedia.org/wiki/Phred_quality_score) (which can be negative) to [Phred](http://en.wikipedia.org/wiki/Phred_quality_score) (which can't). This scheme was used in older Illumina GA Pipeline versions (prior to 1.3). Default: off. |
| `--int-quals`      | Quality values are represented in the read input file as space-separated ASCII integers, e.g., `40 40 30 40`..., rather than ASCII characters, e.g., `II?I`.... Integers are treated as being on the [Phred quality](http://en.wikipedia.org/wiki/Phred_quality_score) scale unless [`--solexa-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-solexa-quals) is also specified. Default: off. |

##### Alignment options

| `--n-ceil <func>` | Sets a function governing the maximum number of ambiguous characters (usually `N`s and/or `.`s) allowed in a read as a function of read length. For instance, specifying `-L,0,0.15` sets the N-ceiling function `f` to `f(x) = 0 + 0.15 * x`, where x is the read length. See also: [setting function options]. Reads exceeding this ceiling are [filtered out](https://ccb.jhu.edu/software/hisat2/manual.shtml#filtering). Default: `L,0,0.15`. |
| ----------------- | ------------------------------------------------------------ |
| `--ignore-quals`  | When calculating a mismatch penalty, always consider the quality value at the mismatched position to be the highest possible, regardless of the actual value. I.e. input is treated as though all quality values are high. This is also the default behavior when the input doesn't specify quality values (e.g. in [`-f`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-f), [`-r`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-r), or [`-c`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-c) modes). |
| `--nofw/--norc`   | If `--nofw` is specified, `hisat2` will not attempt to align unpaired reads to the forward (Watson) reference strand. If `--norc` is specified, `hisat2` will not attempt to align unpaired reads against the reverse-complement (Crick) reference strand. In paired-end mode, `--nofw` and `--norc` pertain to the fragments; i.e. specifying `--nofw` causes `hisat2` to explore only those paired-end configurations corresponding to fragments from the reverse-complement (Crick) strand. Default: both strands enabled. |

##### Scoring options

| `--mp MX,MN`          | Sets the maximum (`MX`) and minimum (`MN`) mismatch penalties, both integers. A number less than or equal to `MX` and greater than or equal to `MN` is subtracted from the alignment score for each position where a read character aligns to a reference character, the characters do not match, and neither is an `N`. If [`--ignore-quals`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-ignore-quals) is specified, the number subtracted quals `MX`. Otherwise, the number subtracted is `MN + floor( (MX-MN)(MIN(Q, 40.0)/40.0) )` where Q is the Phred quality value. Default: `MX` = 6, `MN` = 2. |
| --------------------- | ------------------------------------------------------------ |
| `--sp MX,MN`          | Sets the maximum (`MX`) and minimum (`MN`) penalties for soft-clipping per base, both integers. A number less than or equal to `MX` and greater than or equal to `MN` is subtracted from the alignment score for each position. The number subtracted is `MN + floor( (MX-MN)(MIN(Q, 40.0)/40.0) )` where Q is the Phred quality value. Default: `MX` = 2, `MN` = 1. |
| `--no-softclip`       | Disallow soft-clipping.                                      |
| `--np <int>`          | Sets penalty for positions where the read, reference, or both, contain an ambiguous character such as `N`. Default: 1. |
| `--rdg <int1>,<int2>` | Sets the read gap open (`<int1>`) and extend (`<int2>`) penalties. A read gap of length N gets a penalty of `<int1>` + N * `<int2>`. Default: 5, 3. |
| `--rfg <int1>,<int2>` | Sets the reference gap open (`<int1>`) and extend (`<int2>`) penalties. A reference gap of length N gets a penalty of `<int1>` + N * `<int2>`. Default: 5, 3. |
| `--score-min <func>`  | Sets a function governing the minimum alignment score needed for an alignment to be considered "valid" (i.e. good enough to report). This is a function of read length. For instance, specifying `L,0,-0.6` sets the minimum-score function `f` to `f(x) = 0 + -0.6 * x`, where `x` is the read length. See also: [setting function options]. The default is `L,0,-0.2`. |

##### Spliced alignment options

| `--pen-cansplice <int>`                     | Sets the penalty for each pair of canonical splice sites (e.g. GT/AG). Default: 0. |
| ------------------------------------------- | ------------------------------------------------------------ |
| `--pen-noncansplice <int>`                  | Sets the penalty for each pair of non-canonical splice sites (e.g. non-GT/AG). Default: 12. |
| `--pen-canintronlen <func>`                 | Sets the penalty for long introns with canonical splice sites so that alignments with shorter introns are preferred to those with longer ones. Default: G,-8,1 |
| `--pen-noncanintronlen <func>`              | Sets the penalty for long introns with noncanonical splice sites so that alignments with shorter introns are preferred to those with longer ones. Default: G,-8,1 |
| `--min-intronlen <int>`                     | Sets minimum intron length. Default: 20                      |
| `--max-intronlen <int>`                     | Sets maximum intron length. Default: 500000                  |
| `--known-splicesite-infile <path>`          | With this mode, you can provide a list of known splice sites, which HISAT2 makes use of to align reads with small anchors. You can create such a list using `python hisat2_extract_splice_sites.py genes.gtf > splicesites.txt`, where `hisat2_extract_splice_sites.py` is included in the HISAT2 package, `genes.gtf` is a gene annotation file, and `splicesites.txt` is a list of splice sites with which you provide HISAT2 in this mode. Note that it is better to use indexes built using annotated transcripts (such as *genome_tran* or *genome_snp_tran*), which works better than using this option. It has no effect to provide splice sites that are already included in the indexes. |
| `--novel-splicesite-outfile <path>`         | In this mode, HISAT2 reports a list of splice sites in the file : chromosome name `<tab>` genomic position of the flanking base on the left side of an intron `<tab>` genomic position of the flanking base on the right `<tab>` strand (+, -, and .) '.' indicates an unknown strand for non-canonical splice sites. |
| `--novel-splicesite-infile <path>`          | With this mode, you can provide a list of novel splice sites that were generated from the above option "--novel-splicesite-outfile". |
| `--no-temp-splicesite`                      | HISAT2, by default, makes use of splice sites found by earlier reads to align later reads in the same run, in particular, reads with small anchors (<= 15 bp). The option disables this default alignment strategy. |
| `--no-spliced-alignment`                    | Disable spliced alignment.                                   |
| `--rna-strandness <string>`                 | Specify strand-specific information: the default is unstranded. For single-end reads, use F or R. 'F' means a read corresponds to a transcript. 'R' means a read corresponds to the reverse complemented counterpart of a transcript. For paired-end reads, use either FR or RF. With this option being used, every read alignment will have an XS attribute tag: '+' means a read belongs to a transcript on '+' strand of genome. '-' means a read belongs to a transcript on '-' strand of genome.(TopHat has a similar option, --library-type option, where fr-firststrand corresponds to R and RF; fr-secondstrand corresponds to F and FR.) |
| `--tmo/--transcriptome-mapping-only`        | Report only those alignments within known transcripts.       |
| `--dta/--downstream-transcriptome-assembly` | Report alignments tailored for transcript assemblers including StringTie. With this option, HISAT2 requires longer anchor lengths for de novo discovery of splice sites. This leads to fewer alignments with short-anchors, which helps transcript assemblers improve significantly in computation and memory usage. |
| `--dta-cufflinks`                           | Report alignments tailored specifically for Cufflinks. In addition to what HISAT2 does with the above option (--dta), With this option, HISAT2 looks for novel splice sites with three signals (GT/AG, GC/AG, AT/AC), but all user-provided splice sites are used irrespective of their signals. HISAT2 produces an optional field, XS:A:[+-], for every spliced alignment. |
| `--no-templatelen-adjustment`               | Disables template length adjustment for RNA-seq reads.       |

##### Reporting options

| `-k <int>`          | It searches for at most `<int>` distinct, primary alignments for each read. Primary alignments mean alignments whose alignment score is equal or higher than any other alignments. The search terminates when it can't find more distinct valid alignments, or when it finds `<int>`, whichever happens first. The alignment score for a paired-end alignment equals the sum of the alignment scores of the individual mates. Each reported read or pair alignment beyond the first has the SAM 'secondary' bit (which equals 256) set in its FLAGS field. For reads that have more than `<int>` distinct, valid alignments, `hisat2` does not guarantee that the `<int>` alignments reported are the best possible in terms of alignment score. Default: 5 (HFM) or 10 (HGFM)Note: HISAT2 is not designed with large values for `-k` in mind, and when aligning reads to long, repetitive genomes large `-k` can be very, very slow. |
| ------------------- | ------------------------------------------------------------ |
| `--max-seeds <int>` | HISAT2, like other aligners, uses seed-and-extend approaches. HISAT2 tries to extend seeds to full-length alignments. In HISAT2, --max-seeds is used to control the maximum number of seeds that will be extended. HISAT2 extends up to these many seeds and skips the rest of the seeds. Large values for `--max-seeds` may improve alignment sensitivity, but HISAT2 is not designed with large values for `--max-seeds` in mind, and when aligning reads to long, repetitive genomes large `--max-seeds` can be very, very slow. The default value is the maximum of 5 and the value that comes with`-k`. |
| `--secondary`       | Report secondary alignments.                                 |

##### Paired-end options

| `-I/--minins <int>` | The minimum fragment length for valid paired-end alignments.This option is valid only with --no-spliced-alignment. E.g. if `-I 60` is specified and a paired-end alignment consists of two 20-bp alignments in the appropriate orientation with a 20-bp gap between them, that alignment is considered valid (as long as [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X) is also satisfied). A 19-bp gap would not be valid in that case. If trimming options [`-3`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-3) or [`-5`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-5) are also used, the [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) constraint is applied with respect to the untrimmed mates.The larger the difference between [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) and [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X), the slower HISAT2 will run. This is because larger differences between [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) and [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X) require that HISAT2 scan a larger window to determine if a concordant alignment exists. For typical fragment length ranges (200 to 400 nucleotides), HISAT2 is very efficient.Default: 0 (essentially imposing no minimum) |
| ------------------- | ------------------------------------------------------------ |
| `-X/--maxins <int>` | The maximum fragment length for valid paired-end alignments. This option is valid only with --no-spliced-alignment. E.g. if `-X 100` is specified and a paired-end alignment consists of two 20-bp alignments in the proper orientation with a 60-bp gap between them, that alignment is considered valid (as long as [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) is also satisfied). A 61-bp gap would not be valid in that case. If trimming options [`-3`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-3) or [`-5`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-5) are also used, the `-X` constraint is applied with respect to the untrimmed mates, not the trimmed mates.The larger the difference between [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) and [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X), the slower HISAT2 will run. This is because larger differences between [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) and [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X) require that HISAT2 scan a larger window to determine if a concordant alignment exists. For typical fragment length ranges (200 to 400 nucleotides), HISAT2 is very efficient.Default: 500. |
| `--fr/--rf/--ff`    | The upstream/downstream mate orientations for a valid paired-end alignment against the forward reference strand. E.g., if `--fr` is specified and there is a candidate paired-end alignment where mate 1 appears upstream of the reverse complement of mate 2 and the fragment length constraints ([`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I) and [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X)) are met, that alignment is valid. Also, if mate 2 appears upstream of the reverse complement of mate 1 and all other constraints are met, that too is valid. `--rf` likewise requires that an upstream mate1 be reverse-complemented and a downstream mate2 be forward-oriented. `--ff` requires both an upstream mate 1 and a downstream mate 2 to be forward-oriented. Default: `--fr` (appropriate for Illumina's Paired-end Sequencing Assay). |
| `--no-mixed`        | By default, when `hisat2` cannot find a concordant or discordant alignment for a pair, it then tries to find alignments for the individual mates. This option disables that behavior. |
| `--no-discordant`   | By default, `hisat2` looks for discordant alignments if it cannot find any concordant alignments. A discordant alignment is an alignment where both mates align uniquely, but that does not satisfy the paired-end constraints ([`--fr`/`--rf`/`--ff`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-fr), [`-I`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-I), [`-X`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-X)). This option disables that behavior. |

##### Output options

| `-t/--time`                                                 | Print the wall-clock time required to load the index files and align the reads. This is printed to the "standard error" ("stderr") filehandle. Default: off. |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| `--un <path> --un-gz <path> --un-bz2 <path>`                | Write unpaired reads that fail to align to file at `<path>`. These reads correspond to the SAM records with the FLAGS `0x4` bit set and neither the `0x40` nor `0x80` bits set. If `--un-gz` is specified, output will be gzip compressed. If `--un-bz2` is specified, output will be bzip2 compressed. Reads written in this way will appear exactly as they did in the input file, without any modification (same sequence, same name, same quality string, same quality encoding). Reads will not necessarily appear in the same order as they did in the input. |
| `--al <path> --al-gz <path> --al-bz2 <path>`                | Write unpaired reads that align at least once to file at `<path>`. These reads correspond to the SAM records with the FLAGS `0x4`, `0x40`, and `0x80` bits unset. If `--al-gz` is specified, output will be gzip compressed. If `--al-bz2` is specified, output will be bzip2 compressed. Reads written in this way will appear exactly as they did in the input file, without any modification (same sequence, same name, same quality string, same quality encoding). Reads will not necessarily appear in the same order as they did in the input. |
| `--un-conc <path> --un-conc-gz <path> --un-conc-bz2 <path>` | Write paired-end reads that fail to align concordantly to file(s) at `<path>`. These reads correspond to the SAM records with the FLAGS `0x4` bit set and either the `0x40` or `0x80` bit set (depending on whether it's mate #1 or #2). `.1` and `.2` strings are added to the filename to distinguish which file contains mate #1 and mate #2. If a percent symbol, `%`, is used in `<path>`, the percent symbol is replaced with `1` or `2` to make the per-mate filenames. Otherwise, `.1` or `.2` are added before the final dot in `<path>` to make the per-mate filenames. Reads written in this way will appear exactly as they did in the input files, without any modification (same sequence, same name, same quality string, same quality encoding). Reads will not necessarily appear in the same order as they did in the inputs. |
| `--al-conc <path> --al-conc-gz <path> --al-conc-bz2 <path>` | Write paired-end reads that align concordantly at least once to file(s) at `<path>`. These reads correspond to the SAM records with the FLAGS `0x4` bit unset and either the `0x40` or `0x80` bit set (depending on whether it's mate #1 or #2). `.1` and `.2` strings are added to the filename to distinguish which file contains mate #1 and mate #2. If a percent symbol, `%`, is used in `<path>`, the percent symbol is replaced with `1` or `2` to make the per-mate filenames. Otherwise, `.1` or `.2` are added before the final dot in `<path>` to make the per-mate filenames. Reads written in this way will appear exactly as they did in the input files, without any modification (same sequence, same name, same quality string, same quality encoding). Reads will not necessarily appear in the same order as they did in the inputs. |
| `--quiet`                                                   | Print nothing besides alignments and serious errors.         |
| `--summary-file`                                            | Print alignment summary to this file.                        |
| `--new-summary`                                             | Print alignment summary in a new style, which is more machine-friendly. |
| `--met-file <path>`                                         | Write `hisat2` metrics to file `<path>`. Having alignment metric can be useful for debugging certain problems, especially performance issues. See also: [`--met`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-met). Default: metrics disabled. |
| `--met-stderr`                                              | Write `hisat2` metrics to the "standard error" ("stderr") filehandle. This is not mutually exclusive with [`--met-file`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-met-file). Having alignment metric can be useful for debugging certain problems, especially performance issues. See also: [`--met`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-met). Default: metrics disabled. |
| `--met <int>`                                               | Write a new `hisat2` metrics record every `<int>` seconds. Only matters if either [`--met-stderr`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-met-stderr) or [`--met-file`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-met-file) are specified. Default: 1. |

##### SAM options

| `--no-unal`        | Suppress SAM records for reads that failed to align.         |
| ------------------ | ------------------------------------------------------------ |
| `--no-hd`          | Suppress SAM header lines (starting with `@`).               |
| `--no-sq`          | Suppress `@SQ` SAM header lines.                             |
| `--rg-id <text>`   | Set the read group ID to `<text>`. This causes the SAM `@RG` header line to be printed, with `<text>` as the value associated with the `ID:` tag. It also causes the `RG:Z:` extra field to be attached to each SAM output record, with value set to `<text>`. |
| `--rg <text>`      | Add `<text>` (usually of the form `TAG:VAL`, e.g. `SM:Pool1`) as a field on the `@RG` header line. Note: in order for the `@RG` line to appear, [`--rg-id`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-rg-id) must also be specified. This is because the `ID` tag is required by the [SAM Spec](http://samtools.sourceforge.net/SAM1.pdf). Specify `--rg` multiple times to set multiple fields. See the [SAM Spec](http://samtools.sourceforge.net/SAM1.pdf) for details about what fields are legal. |
| `--remove-chrname` | Remove 'chr' from reference names in alignment (e.g., chr18 to 18) |
| `--add-chrname`    | Add 'chr' to reference names in alignment (e.g., 18 to chr18) |
| `--omit-sec-seq`   | When printing secondary alignments, HISAT2 by default will write out the `SEQ` and `QUAL` strings. Specifying this option causes HISAT2 to print an asterisk in those fields instead. |

##### Performance options

| `-o/--offrate <int>`    | Override the offrate of the index with `<int>`. If `<int>` is greater than the offrate used to build the index, then some row markings are discarded when the index is read into memory. This reduces the memory footprint of the aligner but requires more time to calculate text offsets. `<int>` must be greater than the value used to build the index. |
| ----------------------- | ------------------------------------------------------------ |
| `-p/--threads NTHREADS` | Launch `NTHREADS` parallel search threads (default: 1). Threads will run on separate processors/cores and synchronize when parsing reads and outputting alignments. Searching for alignments is highly parallel, and speedup is close to linear. Increasing `-p` increases HISAT2's memory footprint. E.g. when aligning to a human genome index, increasing `-p` from 1 to 8 increases the memory footprint by a few hundred megabytes. This option is only available if `bowtie` is linked with the `pthreads` library (i.e. if `BOWTIE_PTHREADS=0` is not specified at build time). |
| `--reorder`             | Guarantees that output SAM records are printed in an order corresponding to the order of the reads in the original input file, even when [`-p`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-p) is set greater than 1. Specifying `--reorder` and setting [`-p`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-p) greater than 1 causes HISAT2 to run somewhat slower and use somewhat more memory then if `--reorder` were not specified. Has no effect if [`-p`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-p) is set to 1, since output order will naturally correspond to input order in that case. |
| `--mm`                  | Use memory-mapped I/O to load the index, rather than typical file I/O. Memory-mapping allows many concurrent `bowtie` processes on the same computer to share the same memory image of the index (i.e. you pay the memory overhead just once). This facilitates memory-efficient parallelization of `bowtie` in situations where using [`-p`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-p) is not possible or not preferable. |

##### Other options

| `--qc-filter`         | Filter out reads for which the QSEQ filter field is non-zero. Only has an effect when read format is [`--qseq`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-qseq). Default: off. |
| --------------------- | ------------------------------------------------------------ |
| `--seed <int>`        | Use `<int>` as the seed for pseudo-random number generator. Default: 0. |
| `--non-deterministic` | Normally, HISAT2 re-initializes its pseudo-random generator for each read. It seeds the generator with a number derived from (a) the read name, (b) the nucleotide sequence, (c) the quality sequence, (d) the value of the [`--seed`](https://ccb.jhu.edu/software/hisat2/manual.shtml#hisat2-options-seed) option. This means that if two reads are identical (same name, same nucleotides, same qualities) HISAT2 will find and report the same alignment(s) for both, even if there was ambiguity. When `--non-deterministic` is specified, HISAT2 re-initializes its pseudo-random generator for each read using the current time. This means that HISAT2 will not necessarily report the same alignment for two identical reads. This is counter-intuitive for some users, but might be more appropriate in situations where the input consists of many identical reads. |
| `--version`           | Print version information and quit.                          |
| `-h/--help`           | Print usage information and quit.                            |





### SAMTOOLS

```c
echo "samtools sort -@ 40 -o $i_bam $i_sam"
echo "samtools sort -@ 40 -o $i_bam $i_sam" >> $log_file
samtools sort -@ 40 -o $i_bam $i_sam
echo " "
echo " " >> $log_file
```

[SAM](http://www.incodom.kr/SAM) 포맷의 파일을 가지고 여러 가지 기능을 수행할 수 있는 커맨드 기반의 프로그램이다. [SAMtools](http://www.incodom.kr/SAMtools)로 분석을 하기 위해서는 SAM의 binary 형태인 [BAM](http://www.incodom.kr/BAM)으로 변환해야 한다. [SAMtools](http://www.incodom.kr/SAMtools)는 SAM 파일 안에서 얼라인먼트를 다루기 위한 다양한 프로그램을 제공한다. (ex. sort, view, index, stats 등).

| sort                                                         |
| ------------------------------------------------------------ |
| samtools sort [**-l** *level*] [**-m** *maxMem*] [**-o** *out.bam*] [**-O** *format*] [**-n**] [**-t** *tag*] [**-T** *tmpprefix*] [**-@** *threads*] [*in.sam* |

Sort alignments by leftmost coordinates, or by read name when **-n** is used. An appropriate **@HD-SO** sort order header tag will be added or an existing one updated if necessary.

The sorted output is written to standard output by default, or to the specified file (*out.bam*) when **-o** is used. This command will also create temporary files *tmpprefix***.***%d***.bam** as needed when the entire alignment data cannot fit into memory (as controlled via the **-m** option).

Consider using **samtools collate** instead if you need name collated data without a full lexicographical sort.



??? bam 파일이 없는데 bam이 자동 생성되서 sort되나요? 코드에서 sam > bam 변환하는 코드 없는 것 같습니다. 또한 samtools view와 sort 차이가 뭔지 view는 변환이고 sort 는 정렬로 알고 있는데 저희 코드에서 sort만 사용했는데 변환이 되나요?

??? alignment 코드에서 index_path 저와 차이가 있는 이유



#### SAM Workflow

- [SAM](http://www.incodom.kr/SAM) 파일을 binary 형태인 [BAM](http://www.incodom.kr/BAM) 파일로 전환

  - **samtools view**를 이용해서 변환한다.

    ```
    samtools view -Sb -h -@ [threads] [SAM] -o [BAM]
    
    -S : input file 포맷 자동으로 확인함.
    -b : output file 포맷 BAM
    -h : SAM file header 출력 (사용하지 않으면, 매핑 결과부터 전환)
    -@ : 사용할 multi-core 수
    -o : samtools view의 결과 파일 (사용하지 않으면, STDOUT)
    ```

- Data processing을 간소화하기 위한 sorting

  - 시퀀싱 리드 매핑 결과를 참조 유전체 서열의 포지션 별로 정렬하는 프로그램이다.

  - **samtools sort**를 이용해서 변환한다.

    ```
    samtools sort -@ [threads] -o [sorted.bam] [BAM]
    
    -o : samtools sort의 결과 파일 (사용하지 않으면, STDOUT)
    -@ : 사용할 multi-core 수
    ```

- sorted.bam file 내의 매핑 결과를 빠르게 접근하기 위한 index

  ```
  samtool index -b -@ [threads] [sorted.bam] [out.index]
  
  -b : BAM file의 index (.bai 파일 생성)
  -@ : 사용할 multi-core 수
  ```



## 출처

[FASTQC] FASTQC manual 참고

[Trimmomatic] http://www.incodom.kr/Trimmomatic

[Trimmomatic search] https://slequime.github.io/HTS-tutorial/trimming-trimmomatic.html

[HISAT2] https://ccb.jhu.edu/software/hisat2/manual.shtml

[samtools] http://www.incodom.kr/SAMtools

[samtools] https://man.cx/samtools