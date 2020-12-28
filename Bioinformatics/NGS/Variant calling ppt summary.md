# 개요 

**VCF** **file** **생성**

1. Samtools (sorted.bam 생성)
2. Picard AddOrReplaceReadGroups 실행
3. picard MarkDuplicates 실행
4. GATK SplitNCigarReads 실행
5. BaseRecalibratior / ApplyBQSR 실행 -> recalibration graph 생성 시 AnalyzeCovariates 실행
6. GATK HaplotypeCaller 실행
7. GATK VariantFiltration 실행



**Annovar Annotation** **진행**

1. Annovar 실행 (ANNOtate VARiation)

<br>

# 1. Samtools (sorted.bam 생성)

: bam file 정렬하기

: 사실 안해도 되는 과정 / AddOrReplaceReadGroups의 option에 --sort-order (-SO) 사용하여 coordinate 조건 주면 out bam file이 정렬되기 때문

: -SO coordinate 의 의미는 mapping position에 기반하여 정렬한다는 의미이다.

<br>

# 2. Picard AddOrReplaceReadGroups 실행

: header section에 @RG tag 부분에 해당하는 정보를 위 tool을 이용하여 추가한다.

: Read group은 시퀀싱 기계의 single run으로 부터 생성된 read들의 묶음이다.

: required Argument는 --input, --output, -RGLB, -RGPL, -RGPU, -RGSM이며 추가로 -SO와 -RGID를 넣어 실행하였다.

\> Input과 output은 bam file기입하면 된다.

\> RGID는 read group idenrifier의 의미로써 ID는 flowcell name, lane number로 구성되고 전세계 시퀀싱 데이터의 고유한 식별자가 된다. 또한 BQSR에 사용되어 같은 errorr model을 사용하기 떄문에 technical batch effect에 기여하는 다양한 요소들을 구분하는 lowest denominator이다.

> batch effect
>
> 분자생물학에서, 배치 효과는 실험에서 비 생물학적 요인이 실험에 의해 생성 된 데이터에 변화를 야기할 때 발생한다. 이러한 영향은 원인이 시험에서 관심있는 하나 이상의 결과와 상관 될 때 부정확한 결론으로 이어질 수 있다.

\> RGLB는 DNA preparation library idenrifier의 의미로써 Markduplicates 사용 시 필수적이며, 이 때 동일한 DNA library가 여러 레인에서 시퀀싱 된 경우, 어떤 read groups가 molecular duplicates를 포함하는 지를 결정한다

\> RGPL은 platform/technology used to produce the read의 의미로써 시퀀싱 데이터를 생성하는데 사용된 시퀀싱 기술을 의미한다. ex) ILLUMINA, SOLID 등

\> RGSM은 sample의 의미로 Read group에서 시퀀싱된 sample의 이름이다. 같은 시퀀싱 데이터를 포함하는 모든 read group들을 같은 SM value로 처리한다.

\> RGPU는 platform unit의 의미로써 1) {flow cell_barcode}, 2) {LANE}, 3) {sample_barcode} 3가지 정보를 보유한다. 1)은 flow cell의 고유 식별자 2)는 flow cell lane 3) sample/libarary 별 식별자이다. ID가 없으면 recalbration 시 우선되어 사용된다.

\> Read group을 추가하는 이유는, SNP/INdel 호출을 할 때 특정 유전자형을 이 origin에 할당하기 위해 Read group에 origin을 할당한다. 이 단계가 없으면 SNP 세트가 있지만 특정 유전자형을 할 당할 수 없다.

\> 즉, SNP 뿐만 아니라 유전자형도 호출한다고 가정하므로 GATK 파이프라인에서 AddOrReplaceReadGRoups가 요구되어 진다. GATK genotyper 시, 특정 SNP가 특정 sample에 영향을 준다는 것을 구분, 식별 될 수 있다.

<br>

# 3. picard MarkDuplicates 실행

: Picard 알고리즘을 이용하여 PCR 중폭과정에서 발생하는 duplicate reads를 발견하고 tag한다.

: Duplicate reads는 시퀀싱 기계의 광학센서가 single amplification cluster을 multiple cluster라고 감지 할 때 또한 생성된다. (= optical duplicates)

> 즉, duplicate 종류는 크게 PCR duplicates와 optical duplicates가 있다.

: reader read pair 둘 다의 5 prime 위치 시퀀스를 비교하여 duplicade read를 mark 한다. 그 후 duplicate read를 다 모으면, 각 read의 base quality의 합을 비교하여 순위를 매기는 알고리즘을 사용하여 primary와 duplicate를 구분한다.

: duplicate reads는 sam flag field에서 hexademical value (0X400)으로 mark 된다.

: 중복으로 표시 되었는 지 여부는 나타내지만 중복 유형을 식별하지는 않는데 이를 위해 Duplicate type (DT) tag가 'optional field' section에 추가 되었다.

: 원하면 REMOVE_DUPLICATE and REMOVE_SEQUENCING_DUPLICATES 옵션을 줘서 duplicates를 지울 수 있다.

: Markduplicate를 사용하는 이유

 \> duplicate는 variant calling 시에 bias를 일으킬 수 있다. 또한 중복을 표시하지 않으면, 증폭된 duplicate가 우선적으로 over-representation 할 지도 모른다. 그러므로 중복을 표시하고 제거하는 것이 전체 범위이 기ㅠ이에 실제로 해로운 영향을 끼치지는 않지만 해당 영역의 품질/신뢰성을 증가 시킨다.

\> doubling counting evidence를 (같은 sequence에서) 피하는 것에 유용하여 snp calling에 적합하다.

<br>

# 4. GATK SplitNCigarReads 실행

: SAM/BAM file 내 CIGAR String의 N 부분을 포함하는 read를 잘라 exon과 intron을 구분한다. 

: 자르는 방식은 hardclip 방식을 사용하였고, softclip과의 차이점은 자를 N string 부분이 bam file내에서 지워져 실제 시퀀스 길이는 length(SEQ) + count-of-hard-clipped-base 이다.

: CIGAR string은 특정 locus에서 reference sequence와 실제 read mapping 시 필수적인 부분이다. 총 9가지가 있고, CNV 또는 indel 추정 시 유용하게 사용되는데 이 때 'N'은 reference에 skip된 부분을 의미한다. RNA alignment시 N은 intron을 의미한다.

: 실행 시 주의 할 점은 reference 사용 시 samtools로 genome.fa.fai 생성 및 gatk CreateSequenceDictionary를 사용해서 genome.dict를 생성해야 작동한다.

<br>

# 5. BaseRecalibratior / ApplyBQSR 실행 

> recalibration graph 생성 시 AnalyzeCovariates 실행

: Base quality score는 시퀀싱 머신에서 각 base 마다 발생하는 error의 추정치를 의미하며, Base Quality Score Recalibration은 이러한 각각의 base score를 다시 한 번 Recalibration 하여, 좀 더 정확한 base quality score르 ㄹ부여하는 과정이다.

: Illumina 기기 이전부터, sanger sequencing을 할 때에도 Phred-score로 염기서열 읽은 것에 대한 수치를 기록해 왔는데, 이러한 phred-score 기반으로 variants가 맞는가 아닌가를 베이지안 추론을 통해 진행하게 된다. 즉, ex) Illumina 기기의 특유 bias를 줄이지 않는다면 계통적 오류 (systemic error)에 의한 위양성(False positive)를 얻게 된다. (물론 위음성_False nagative 도 생김)

> Phred-score는 해당 결과를 얼마나 믿을 수 있는가에 대한 수치이다.
>
> Phred quality scores Q are defined as a property which is logarithmically related to the base-calling error probabilites P
>
> ![Q=-10\ \log _{{10}}P](https://wikimedia.org/api/rest_v1/media/math/render/svg/4bf1e60a0c90edd9ec883d812daef63fc4386d18)
>
> or
>
> ![P=10^{{{\frac  {-Q}{10}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1a30ecef1f2739d87f5451d0a748d257e48a4bef)

: BQSR 과정을 machine learning 알고리즘을 사용하여 NGS 장비의 고유한 특성에 의한 오류 (systematic error)에 의해 과다하게 높게 측정되거나 (over-estimated) 낮게 (under-estimated) 품질 점수를 조정함으로써 정확하고 균일한 염기 품질 점수를 제공한다.

: 이러한 균일 품질 점수를 제공하는 recalibration 모델은 baserecalibrator로 생성하고, ApplyBQSR로 score를 재조정 (overwriting) 한다.

: How BQSR work

 \> 먼저 BQSR을 돌리기 위해 필요한 것은 known single nuclieic polymorphisins (SNPs) 즉, dbsnp로 참조 유전체에 대하여 분석된 vcf가 있어야 한다. 처음 분석이라 vcf가 없을 때, GATK 홈페이지에서는 vcf 없으면 우선 없이 만들고 그것을 input으로 BQSR 돌리고 다시 vcf 만들어서 진행하라고 하지만, 실제 실행 시 그냥 NCBI에서 다운 받은 reference vcf를 사용하였다.

 \> 첫 번째로 fa/vcf reference와 비교하여 error를 찾고, 두 번째로 전체 read의 reported phred score를 계산한다. 세 번째는 empirical (경험적) phred score를 구해 reported phred score와 비교 및 model을 만들어 recalibration을 진행한다.

> 경험적(empirical) 이라는 의미는 실제 측정된 phred score를 이용하는 것이 아닌, 에러인지 (true), 아닌지 (false)를 true 개수/전체 개수 빈도확률로 나타낸 것을 의미한다.

: AnayzeCovariates는 recalibration 전과 후를 비교해주는 graph를 생성하는 tool이다.

<br>

# 6. GATK HaplotypeCaller 실행

 : Variant calling은 genotype(heterozygous와 homozyhous)과 SNPs를 찾는다. variant caller인 software인 GATK를 사용하여  alignment bam file에서 snp와 indel을 찾아낸다.

: 간단한 haplotypecaller 작동 방식

 \> 1. Active region을 결정한다.

> Active region은 변이가 발생하여 non-synomymous of genome 상의 위치를 의미

 즉, reference와 다른 allele들이 반복적으로 나오는 곳을 특정하여 active region을 정의한다.

 \> 2. Active region의 assembly를 통하여 haplotypes을 결정한다.

*  haplotypecaller는 각각의 active region에 대해 k-mer를 사용하여 De Brujin-like  graph를 만들고 active region을 제조합하며 (!=reassembly), 데이터에서 실제로 존재 할 수 있는 haplotypes 후보들을 만든다.
* 후보 haplotypes을 만들었다면, 발견하지 못했던 variant sites를 식별하기 위하여  Smith-Waterman algorithm을 사용한다.

 \> 3. PairHMM algorithm

  각각의 active region에 대해 haplotypecaller는 PairHMM algorithm을 사용하여 각각의 haplotype에 대한 각 read의 pairwise alignment를 통해 likelihood를 결정한 read-haplotypes matrix를 만든다.

 \> 4. matrix에서 가장 높은 evidance (확률)을 갖는 variant를 선택하여 변이를 동정한다.



==> 제가 이 부분은 통계 알고리즘이 들어가서 제대로 이해를 못해서요 

https://blog.naver.com/gieyoung0226/221942012994

https://bioinformaticsandme.tistory.com/65

이 두 블로그가 정리가 잘되있어서 보고 이해하시면 되요!!

<br>

# 7. GATK VariantFiltration 실행

: INFO and/or FORMAT annotation 기반으로 filter 하는 tool이다.

: FILTER/INFO/FORMAT 등 안에 해당하는 ex) FS/QD 같은 filter-name을 지정하여 조건을 준다.

 => -filter "FS > 30.0"

<br>

 # Annovar 실행 (ANNOtate VARiation)

: Annovar는 Annotation (주석달기) tool로써, option에 따라서 원하는 내용대로 다양한 Database에서 관련 내용을 annotation 할 수 있다. 간단히 말하면 Database 파일을 통째로 다운받아서, PErl script 기반의 annotate_variation.pl 또는 table_annovar.pl을 실행하면, 그에 맞는 내용을 찾아서 주석으로 달아준다.

: Annovar는 크게 3가지 방식으로 Annotation을 수행하는데

* **Gene-based annotation:** 단백질 코딩 지역의 변이인지 확인 
* **Region-based annotation**:  특정 게놈 영역의 변이인지 확인 
* **Filter-based annotation**: 특정 DB에서 존재하는 변이인지 확인 

이러한 방식은 조건 -operation 시 각 순서대로 Database에 따라 g,r,f,로 조건을 주면 된다.

: Annovar의 tool은 총 4가지이나 실제 사용한 것은 annotate_variation.pl과 table_variation.pl을 이다. 가장 기본적으로 사용되는 것이 annotate_variation.pl을 사용하여 annotation을 하는 것이나 table_annovar.pl을 사용하면 vcf 파일로 바로 쉽고 간단하게 분석이 가능하기 때문에 table_annovar.pl을 사용하여 분석하였다. annotatate_variation.pl은 database 다운 시 -builderver -downdb 조건을 줘서 이용하였다.

: Database는 총 6가지를 (refGene, cytoBand, exac03, avsnp147, dbnsfp30a, clinvar_20200316) 사용하였고, 각 database에 따라 -g,r,f를 지정하면 된다.

: 사용하보고 깨달은 것은 genotype만 확인 시 많은 database가 필요 없고 refGene, CytoBand, avsnp147만 있으면 될 것 같다.



