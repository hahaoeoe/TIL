# Command Line Tools for Genomic Data Science

> * by 존스홉킨스대학교 (coursera 강의)
>
> - **강사:**[Liliana Florea, PhD](https://www.coursera.org/instructor/~12152931), Assistant Professor
>
>   McKusick-Nathans Institute of Genetic Medicine

<br>

## Week1. Basic Unix Commands

1. cd

2. cd .. 

3. cd .

4. ls

5. ls -l

6. ls -lf

7. more 

   > spacebar 누르면 정보가 넘어간다.
   >
   > /> 하면 chromosome 1에서 2로 넘어간다.

8. less

9. rm

10. rm -r

11. pwd

12.  cp

13. mv

14. head 

    > head - 50 peach.genome
    >
    > : line 50으로 가서 정보 보여줌

15. tail peach.genome

16. tail -15 peach.genome

17. cat peach.genes

    > concatenate

18. cat \*/*.genome

19. wc apple/apple.genome

20. wc -l apple/apple.genome

21. wc -l apple/apple.genome > nlines

22. wc -l < apple/apple.genes

23. ls | wc -l 

24. cat \*/*.genome | more

25. more months

26. sort months

    > 순서대로

27. sort -r months

    > 반대로

28. vi months

    > editort로 달력 내용 바꿈

29. sort -k2 months

    > 내용이 january 1 winter 
    >
    > october 10 fall
    >
    > 이렇게 있는데 1의 자리 숫자로 sort 했다는 의미이다.

30. sort -k 2n months

    > 1부터 순차적으로 sort

31. sort -k 2nr months

    > 30번 결과의 반대

32. sort -k 3 months

33. sort -k 3 -k 2n months

34. sort -k 3  -k 2rn

35. vi months

36. cut -f1 months

37. cut -f1,2 months

38. cut -f1-3 months

39. cut -d ' ' -f1 months

40. cut -d '  ' -f1-3 months

    > 앞에 -d 를 붙이면 내용이 f1만 나오게 하는 것
    >
    > f2까지 썼으면 f2까지 나온다.

41. cut -d ' ' -f3 months > seasons

42. more seasons

43. sort -u seasons

    > -u는 unique

44. uniq seasons

    > 이렇게 하면 winter가 두 번 나오게 됨
    >
    > hen we had the three spring lines replace with just one spring, summer, 
    >
    > fall and then we had December the winter might correspond to December at the end. 
    >
    > So you can see that winter now appears twice because it was stated two 
    >
    > distinct places within the file.

45. uniq seaons | sort -u

46. uniq -c seasons

    > c는 counts 즉, number of ouucrrences

47. grep root \*/*.samples

48. grep " 12 winter" months

49. grep "7 winter" months

50. grep -n "12 winter" months

51. more orchard

    > 내용 보여줌

52. cp orchard orchard.1

53. vi orchard.1

    > 에디터를 통해 두번째 줄 지움

54. more orchard.1

55. diff orchard orcahrd.1

    > 2d1이 화면에 뜸
    >
    > < the smell of cool, ~~ 두번 째줄 내용 나옴

56. vi orchard.1

    > evening을 bright night로 바꿈

57. diff orchard orchard.1

    > 2c1

58. more apple/apple.samples

59. comm apple/apple.samples pear/pear.samples

    > - 2개의 파일을 비교하는 리눅스 명령어
    > - (diff 와는 달리) 동일 행끼리만 단순 비교
    >
    > <br>
    >
    > - 첫번째 파일에만 있으면 첫번째 칸(탭 없음)
    > - 두번째 파일에만 있으면 두번째 칸(탭 1개)
    > - 둘 다 있으면 세번째 칸(탭 2개)

60. sort apple/apple.samples > apple/apple.samples.sorted

61. sort pear/pear.samples > pear/pear.samples.sorted

62. comm apple/apple.samples.sorted pear/pear.samples.sorted

63. comm  -1 -2 apple/apple.samples.sorted pear/pear.samples.sorted

    > 이렇게 하면 62번 결과에서 둘 다 있는 세번째 칸만 결과로 나오게 된다.

64. gzip apple.genome

65. ls -l apple.*

66. gunzip apple.genome.gz 

67. ls 

68. ls -l

69. bizp2 apple.genome

70. ls -l apple.genome.bz2

71. bunzip2 apple.genome.bz2

72. ls -l 

73. tar -cvf Apple.tar apple.genes apple.genome apple.samples

    > cvf 알아보기
    
74. gzip Apple.tar

75. tar -xvf Apple.tar

76. 

77. rm * 

    > everything remove in the local directory

78. tar -cvf AppleD.tar apple/

79. bzip2 AppleD.tar

80. mv AppleD.tar.bz2 sandbox

81. bunzip2 AppleD.tar.bv2

82. tar -xvf AppleD.tar

<br>

**Practical Expercises I**



## Week2. Sequences and Genomic Features

#### Molecular Bio Primer

 Exon - Intron

<Br>

####  Sequence Representation and Generation

**Generation**

* Sanger sequencing (until 2008)
  - Shear DNA/RNA into fragments
  - Clone in vector (amplification)
  - Sequence insert end(s): 500-800bp
  - Slow, costly, medium-to-high throughput
* Next Generation Sequencing(NGS) (>2008)
  - Shear DNA/RNA (cDNA)
  - May or may not require amplification
  - Sequence fragment ends: 50-450 bp, single-end or paired-end reads
  - Low cost (<$0.04/Mb), fast, very high throughput (100s millions / run)

*=> Assemlby can be very challenging, especially with the shorter sequences!*

<br>

**Fast/Fastaq**

<br>

#### Annotation

> 이해 잘 못함

**Genomic features**

* Genome annotation = determine the precise location and structure (intervals, or lists of intervals, and associated biological information) of genomic features along the genome
* Genomic features: genes, promoters, protein binding sites, translation start /stop site, DNasel sites, etc.
* Example - gene annotations:
  * Exon/intron structure (exon and intron start-end coordinates)
  * Strand ( + or - )
  * Start and end sites for translation (ORF)



**BED format**

: Browser Extensible Data

<br>

**GTF format**

: Genomic Transfer Format

<br>

**GFF3 format**

**GDF format**

<br>

#### Alignment I

**Alignments**

* Sequence a fragment of  the gene (RNA) or genomic region (DNA), then map (align) it to the genome
* *Alignment*: a mapping between the letters of the two sequences, with some spacers (indels)
* The alignment will take into account differences such as polymotphisms and sequencing errors, and introns (for genes)
* Insertion / Deletion / Substitutions

<br>

**Representation: SAM/BAM format**

<br>

#### Alignment II

**SAM format: FLAG**

**SAM format: CIGAR**

* M match (sequence match or substitution)

* I insertion to the reference 
* D deletion from the reference 
* N skipped region (intron) 
* S soft clipping (sequence start or end not aligned; seq appears in SEQ) 
* H hard clipping (seq not in SEQ) 
* P padding first segment (mate) 
* = sequence match 
* X sequence mismatch 

<br>

#### Recreating Sequences & Features

> GenBank:	Nucleo6de, SRA, GEO	
>
> UCSC Table Browser	
>
> Ensembl

NCBI에서 원하는 gene을 검색하고 FASTQ 파일이나, SRA 파일을 다운받아서 weget ftp://ftp-trace~~ 를 prompt에서 실행하면 원하는 gene을 다운 가능

<br>

```c
$ nohup fastq-dump STT1107997.sra &
$ head SSRR1107997.fastq

```

<br>

#### Genomic Feature Retrieval

: UCSC Table Browser

<br>

#### SAMtools I

$ samtools1 flagstat example.bam

$ samtools1 sort

> option 볼 수 있음

$ samtools1 sort example.bam

$ nohup samtools1 sort example.bam examole.sorted &

$ ls  NA12814

$ samtools1 index

$ index example.sorted.bam

$ ls -lt

$ ls NA12814_1.fastq.gz NA12814_2.fastq.gz

$ zcat NA12814_1.fastq.gz | wc -l

$ samtools1 merge

$ nohup samtools1 merge NA12814.bam NA12814_1.bam NA12814_2.bam &

$ samtools1 flagstat NA12814_1.bam

<br>

=> SAMtools flags, sort, index에 대해 배움

<br>

#### SAMtools II

$ samtools1 view example.bam | more

$ view -h example.bam | more

$ samtools1 view -h exmple.bam > example.sam

$ samtools view -H example.bam

<br>

*How to convert SAM to BAM*

$ samtools1 view -bT /data1/igm3/genomes/hg38/hg38c.fa example.sam > example.sam.bam

$ samtools1 view example.sam.bam

$ samtools1 view

$ samtools1 view -H example.bam "chr22:2400000-2500000"

$ samtools1 index example.bam

$ ls -l example.bam.bam

$ samtools1 view example.bam "chrr22:2400000-2500000" | head

$ samtools1 view -L example.bed example.bam | head

$ samtools1 view -L example.bed example.bam | tail

<br>

#### BEDtools I

$ bedtools >& bedtools.log

$ bedtools intersect >& intersect.log

$ vi intersect.log

 $ bedtools intersect -wo -a RefSeq.grf -b Alus.bed | more

$ bedtools intersect -wo -a RefSeq.gtf -b Alus.bed | cut -f9 | cut -d ' ' -f2 |more

$ bedtools intersect -wo -a RefSeq.gtf -b Alus.bed | cut -f9 | cut -d ' ' -f2 |sort -u wc -l

<br>

#### BEDtools II

<br>

## Week3. Alignment & Sequence Variation

