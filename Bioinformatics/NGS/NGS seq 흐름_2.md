# NGS 흐름 2

> HTSeq-count
>
> DESeq2 

<br>

## HTSeq-count

> bam 파일을 read count하여 read의 수를 세어준다.

htseq-count [options] <alignment_files> <gff_file>

* The <alignment_file> are one or more files containing the aligned reads in SAM format. (SAMtools contain Perl scripts to convert most alignment formats to SAM.) Make sure to use a splicing-aware aligner such as STAR. HTSeq-count makes full use of the information in the CIGAR field.
* The <gff_file> contains the features in the GFF format.

<br>

### HTSeq의 기능 중 read counting

- 사용자가 각 유전자의 exonic region 안으로 얼마나 많은 read들이 떨어지는 지 알고 싶다면, 이 목적을 위해서 우리는 첫 번째로 exon의 위치에 대한 정보를 읽을 필요가 있다. 이와 같은 정보의 편리한 source는 Ensembl로부터 GTF file을 받는 것이다. HTSeq은 GFF file 내 정보를 읽을 수 있는 GFF_Reader class를 제공한다.

  예제 : gff_file = HTSeq.GFF_Reader("gff_name", ...)

- 더 쉬운 사용방법으로는 위에 처럼 직접 작성하지 않고 HTSeq-count를 사용해서 결과를 얻는 방법이 있다.

  예제 : htseq-count [option]

- 중요한 점 strandedness에 대해서 default는 yes이다. 만약 당신의 RNA-Seq data가 strand-specific protocol로 만들어지지 않았다면 이 것은 read의 절반을 잃어버린다. 그런 이유로 사용자가 strand-specific data를 가지지 않았다면 --stranded=no 옵션을 설정해야 한다.

<br>

```c
echo "htseq-count -s reverse -f bam $input_path$i_bam $index_path/Mus_musculus.GRCm38.96.gtf> $i_count"
echo "htseq-count -s reverse -f bam $input_path$i_bam $index_path/Mus_musculus.GRC,38.96.gtf> $i_count" >>$log_file
htseq-count -s reverse -f bam $input_path$i_bam $index_path/Mus_musculus.GRC,38.96.gtf> $i_count
```

<br>

### Options

| -f <format>, -- format = <format>                        | Format of the input data. Possible values are sam (for text SAM files) and bam (for binary BAM files). Default is sam. DEPRECATED: Modern versions of samtools/htslibs, which HTSeq uses to access SAM/BAM/CRAM files, have automatic file type detection. This flag will be removed in future versions of htseq-count. |
| -------------------------------------------------------- | ------------------------------------------------------------ |
| **-r** <order>, **--order**=<order>                      | For paired-end data, the alignment have to be sorted either by read name or by alignment position. If your data is not sorted, use the samtools sort function of samtools to sort it. Use this option, with name or pos for  to indicate how the input data has been sorted. The default is name. If name is indicated, htseq-count expects all the alignments for the reads of a given read pair to appear in adjacent records in the input data. For pos, this is not expected; rather, read alignments whose mate alignment have not yet been seen are kept in a buffer in memory until the mate is found. While, strictly speaking, the latter will also work with unsorted data, sorting ensures that most alignment mates appear close to each other in the data and hence the buffer is much less likely to overflow. |
| **--max-reads-in-buffer**=<number>                       | When  is paired end sorted by position, allow only so many reads to stay in memory until the mates are found (raising this number will use more memory). Has no effect for single end or paired end sorted by name. (default: 30000000) |
| **-s** <yes/no/reverse>, **--stranded**=<yes/no/reverse> | Whether the data is from a strand-specific assay (default: yes) For stranded=no, a read is considered overlapping with a feature regardless of whether it is mapped to the same or the opposite strand as the feature. For stranded=yes and single-end reads, the read has to be mapped to the same strand as the feature. For paired-end reads, the first read has to be on the same strand and the second read on the opposite strand. For stranded=reverse, these rules are reversed. |

> 그 외로 다른 options 들도 존재

<br>

## DEseq2

**Description**

DESeq2는 DEG분석의 대표적인 방법 중의 하나로, 차세대 염기서열분석(Next Generation Sequencing)으로부터 얻는 read count data를 분석하는 R 패키지이다.

The DESeq2 package is designed for normalization, visualization, and differential analysis of highdimensional count data. It makes use of empirical Bayes techniques to estimate priors for log fold change and dispersion, and to calculate posterior estimates for these quantities.

DEG 분석을 수행하면 두 개의 결과를 반드시 얻는다. (Fold-change와 P-value)요소이다. Fold change란, 어떤 유전자에 대하여 실험군에서의 평균발현량이 대조군에서의 평균발현량의 몇 배인지를 나타내고, P-value는 두 군의 평균발현량 차이가 통계적으로 유의미한 값인지를 알려준다.

생명과학 연구에서 유전자 발현량을 두 조건에 대하여 서로 비교하고자할 때 fold change(FC)를 많이 이용한다. 이 수치는 DEG(differentially expressed gene)을 찾는데 매우 중요한 의미를 갖는다. FC의 정의는 비교 조건(treatment)의 값을 기준 조건(control)의 값으로 나누는 것이다.

FC = treatment / control

FC = treatment/control라는 수식을 그대로 따르면 1을 기준으로 너무나 비대칭적인 값을 얻게 된다는 것도 문제이다. 즉 control < treatment이면 FC는 1보다 큰 값을 가지며 그 범위도 매우 넓다. 그러나 control > treatment 이면? 0 < FC < 1이 되어 너무 좁은 범위에 FC가 분포하게 된다. 이를 해결하는 방법 중 하나는 FC를 그대로 쓰지 않고 log2(FC)로 전환하는 것이다. 단 FC = 0인 것은 적절히 제거를 하든지 전처리를 해야 한다.



<br>

**Code 이해**

```R
meta <- rownames_to_column(meta, "sample")
```

 행을 사용하기 위해서 행 이름을 sample 변수로 바꿔준다. 이때,  tibble 패키지에 있는 rownames_to_column() 을 사용한다.  바꿔줄 데이터, 그리고 생길 행의 변수명을 입력한다.

```R
all(meta$sample == colnames(JKreadcount))
```

True / False로 반환해줌

```R
desing = data.frame(row.names = colnames(JKreadcount), condition = meta$phenotype)
condition = meta$phenotype
dds <- DESeqDataSetFromMatrix(countData = JKreadcount, colData = design =~condition)
```

* Create a coldata frame and instantiate the DESeqDataSet. See ?DESeqDataSetFromMatrix

* ```r
  dds <- DESeq2::DESeqDataSetFromMatrix(
    countData = cts, 
    colData   = coldata, 
    design    = ~batch + condition
    )
  
  # count matrix called cts and a table of sample information called coldata. 
  # The design indicates how to model the samples, here, that we want to measure the effect of the condition, controlling for batch differences. The two factor variables batch and condition should be columns of coldata.
  
  # countData is your experimental data, prepared as above;
  ```

<br>

colData is your coldata matrix, with experimental metadata; ~treatment is the formula, describing the experimental model you test in your experiment. It could be anything like ~ treatment + sex * age etc.

* To use DESeqDataSetFromMatrix, the user should provide the counts matrix, the information about the samples (the columns of the count matrix) as a DataFrame or data.frame, and the `design` formula. 
* To demonstate the use of DESeqDataSetFromMatrix, we will read in count data from the pasilla package. We read in a count matrix, which we will name `count Data`, and the sample information table, which we will name `colData`.

<br>

```R
baseMean <- sapply(levels(dds$condition), function(lvl) rowMeans(counts(JKdds$condition == lvl)))
res <- cbind(baseMean, res)
```

* sapply 함수: 모든 기준 및 연산 과정이 lappy와 동일하며, 차이점은 리턴값이 행렬 또는 벡터라는 점 뿐이다. sapply(데이터, 연산)
* levels 함수: factor형의 값으로서, 미리 정해진(predefined), 한정된 수의 범주형 값(finite number of categorical values)을 수준(levels) 이라고 합니다. **요인 데이터의 수준을 확인하거나 지정, 변경하려면 levels() 함수**를 사용한다.
* function 함수: 사용자 정의 함수, function(인자)
* 





## Variant 

인간 Exome-seq VCF 파일에 수록된 변이들에 대한 기능적 주석 달기

: Ensembl VEP라는 웹 서비스를 사용하면 두 가지 종류의 작업을 하는데, 하나는 dbSNP와 같은 데이터베이스에 등재된 변이인지 여부를 알려주는 것이고, 나머지 하나는 변이가 가져올 수 있는 유전자 기능적인 변화를 예측하는 것이다.



$ bcftools stats NA18959.chrom20.ILLUMINA.bwa.JPT.exome.20121211.bcf | less –S

Annotation : VCF 파일의 Variants에 대해 dbSNP에 등재되어있는지 확인하는 과정

(여기서는 Ensembl VEP 서비스를 이용한다.)

구글 > ensembl VEP > Web interface > GRCh37 website 클릭 >

$ bcftools view NA18959.chrom20.ILLUMINA.bwa.JPT.exome.20121211.bcf

\> NA18959.chrom20.ILLUMINA.bwa.JPT.exome.20121211.vcf

\> NA18959.chrom20.ILLUMINA.bwa.JPT.exome.20121211.vcf 파일 업로드

\> Additional annotations에서 Phenotypes 선택 > Run

http://grch37.ensembl.org/Homo_sapiens/Tools/VEP/Results?tl=lFqOJVjcqHlrUtOt-6200437



\> Download All VCF (ensembl_VEP_result.vcf으로 이름 변경)

$ less -S ensembl_VEP_result.vcf

$ egrep "\|rs[[:digit:]]+" ensembl_VEP_result.vcf | less –S (rs를 가지는 경우만 확인)

$ egrep "\|rs[[:digit:]]+" ensembl_VEP_result.vcf | wc –l (rs를 가지는 개수)

$ egrep "^#|\|rs[[:digit:]]+" ensembl_VEP_result.vcf | less –S (rs와 Header 함께 확인)

$ egrep "^#|\|rs[[:digit:]]+" ensembl_VEP_result.vcf > known.vcf

$ bcftools stats known.vcf | less –S









<br>

## 출처

[HTSeq] http://www.incodom.kr/HTSeq#h_83acc28eabd73c7c5ff5c98b103e98ce

[Deseq2] http://blog.genoglobe.com/2017/10/fold-change.html

[Deseq2] http://blog.naver.com/PostView.nhn?blogId=cjh226&logNo=221360753408

[Variant] https://blog.naver.com/glow216/222003671172

[levels 함수] 출처: https://rfriend.tistory.com/32

