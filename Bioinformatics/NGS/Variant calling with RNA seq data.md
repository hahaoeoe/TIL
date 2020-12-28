# Variant calling with RNA seq data 

**GATK RNAseq short variant discovery (SNPs + Indels)**

![img](https://drive.google.com/uc?id=1yLptERtjtDzx36vqAPZCMJgvbhASdsjq)

https://gatk.broadinstitute.org/hc/en-us/articles/360035531192-RNAseq-short-variant-discovery-SNPs-Indels-

## 관련 Tool 모음

1. Samtools
2. Picard
3. GATK
4. VEP

<br>

## VCF file 생성

1. bam file  (이미 hisat2로 mapping 끝난 결과 파일)
2. PICARD 다운로드
3. GATK 다운로드
4. VEP 다운로드 



* bam file을 samtools로 sort해서 .sorted.bam 파일 만들기



* picard MarkDuplicates 실행하기

  \>  Picard 알고리즘을 이용하여 PCR 증폭 과정에서 발생하는 중복 된 시퀀싱 리드(duplicate read)를 표시하여 제거하는 과정

  \> MarkDuplicates is important in removing PCR duplicates -- which can introduce bias in your variant calling. If you did not mark duplicates, you would risk having over-representation in your sequence of areas preferentially amplified during PCR. One way to think about it is that marking duplicates and removing them does not really have a detrimental effect on your overall depth of coverage -- but increases the quality/reliability of the areas you have covered.
  
  https://www.biostars.org/p/18784/
  
  \> Removing duplicates refers to multiple reads that match at the same position in the genome. This is different than one read (or read pair) mapping to multiple genome locations. What you are trying to do in duplicate removal is identify reads that are PCR duplicates, which is especially useful in SNP calling since you want to avoid double counting evidence from the same underlying biological sequence.
  
  MarkDuplicates finds sequence pairs that map to the same position, marking or removing the duplicates so you can work with unique pairs in downstream analys es. If you want them removed, use the REMOVE_DUPLICATES=true flag when running the program
  
  https://www.biostars.org/p/788/
  
  
  
  \> The program can take. either coordinate-sorted or query-sorted inputs, however the behavior is slightly different. When the input is coordinate-sorted, unmapped mates of mapped records and supplementary/secondary alignments are not marked as duplicates. However, when the input is query-sorted (actually query-grouped), then unmapped mates and secondary/supplementary reads are not excluded from the duplication test and can be marked as duplicate reads.
  
  If desired, duplicates can be removed using the REMOVE_DUPLICATE and REMOVE_SEQUENCING_DUPLICATES options.
  
  http://broadinstitute.github.io/picard/command-line-overview.html#MarkDuplicates
  
  
  
  
  
  
  
  \> https://gatk.broadinstitute.org/hc/en-us/articles/360037052812-MarkDuplicates-Picard-


```
java -jar picard.jar MarkDuplicates -I SRX.sorted.bam -O SRX.mdup.bam -M SRX.mdup.txt
```

 





* GATK SplitNCigarReads 실행하기

  > Split Reads with N in Cigar

  \> human Reference 는 grch38_tran/genome.fa 사용

  \> GATK에서 -R (reference) 사용하기 위해서는 samtools로 genome.fa.fai 생성 및 gatk CreateSequenceDictionary를 사용해서 genome.dict 생성해야 작동한다.

  \> https://gatk.broadinstitute.org/hc/en-us/articles/360050815972-SplitNCigarReads

  

* Picard AddOrReplaceReadGroups 실행

  \> 실행 하지 않고 바로 base quality recalibration 넘어가면 error

  \> 원래의 bam file에 Read group에 관한 정보가 없기 때문에 error 그러므로, @RG 정보 추가 필요

  [Add argument] https://gatk.broadinstitute.org/hc/en-us/articles/360037226472-AddOrReplaceReadGroups-Picard-

  [Read group] https://gatk.broadinstitute.org/hc/en-us/articles/360035890671-Read-groups

  https://support.sentieon.com/appnotes/read_groups/

  https://gatk.broadinstitute.org/hc/en-us/articles/360035532352

  

  \> Read goup은 시퀀싱 기계의 single run으로 부터 생성된 리드들의 묶음이다. 

  \> http://seqanswers.com/forums/showthread.php?t=11887

  > A ReadGroup will assign an origin to a set of reads in order to assign a specific genotype to this origin when making the SNP/InDel calling. Without this step, you will have a set of SNPs but you cannot assign them to a specific genotype... This AddOrReplace step is requested by GATK pipeline, as it supposed you will call genotype and not only SNP. If you need only a raw set of SNP, you can use PileUp format and VarScan utility Pileup2SNP.
  >
  > 
  >
  > The origin in my case can be either a lane, the name of the individual/organism. You can have eg 10 individuals tagged in a single lane, then mapped individually and then affected to a group (eg Indiv1, Indiv2...). Then all reads from a single individual are tagged by the same flag RG at the end of the SAM line. When you merge all those 10 SAM, each lane is tagged by an origin.
  > Then you asked for example to the GATK Genotyper to 'call the genotype'. It means that SNP will be identified, based on depth, quality, etc. And as each read can be affected to a specific individual, you can say obtain in the resultant VCF file an info saying 'Ok, Indiv1 has a A instead of a G at the position chr01:234554'.

  \> -SO coordinate 의 의미   

  > **Coordinate sorting** refers to **sorting** based on the mapping positions (and can only be used if you have mapper your reads onto a reference genome/assembly). This is the default **sorting** and is essential for **BAM** viewers and used for things like SNP analysis.

  

* Base Quality Recalibration을 진행

  \>  인터넷에서 다운 받아 실행 (common_all_20180418.vcf.gz 파일 다운) 

  \> Tool로서 BaseRecalibrator를 사용한다.

  \> 결과로  recalibration table를 생성한다.

  \> https://gatk.broadinstitute.org/hc/en-us/articles/360050815072-BaseRecalibrator


  Illumina 기기 이전부터, Sanger sequencing을 할 때에도 Phred-score 로 염기서열 읽은 것에 대한 수치를 기록해 왔다. 우리는 이러한 Phred-score 기반으로 변이가 맞는 가 아닌가를 베이지안 추론을 진행하게 된다. 그러기 때문에 Illumina 기기 특유의 bias를 줄이지 않는다면 계통적 오류(systemic error)에 의한 위양성(false positive)를 얻게 된다.  물론 위음성(false negative)도 생긴다.

\* 잠깐 프레드 스코어는 해당 결과를 얼마나 믿을 수 있는가에 대한 수치이다.

Phred quality scores ![Q](https://wikimedia.org/api/rest_v1/media/math/render/svg/8752c7023b4b3286800fe3238271bbca681219ed) are defined as a property which is logarithmically related to the base-calling error probabilities ![P](https://wikimedia.org/api/rest_v1/media/math/render/svg/b4dc73bf40314945ff376bd363916a738548d40a).[[2\]](https://en.wikipedia.org/wiki/Phred_quality_score#cite_note-phred-score-2)

![Q=-10\ \log _{{10}}P](https://wikimedia.org/api/rest_v1/media/math/render/svg/4bf1e60a0c90edd9ec883d812daef63fc4386d18)

or

![P=10^{{{\frac  {-Q}{10}}}}](https://wikimedia.org/api/rest_v1/media/math/render/svg/1a30ecef1f2739d87f5451d0a748d257e48a4bef)

For example, if Phred assigns a quality score of 30 to a base, the chances that this base is called incorrectly are 1 in 1000.

출처: https://solution4u.tistory.com/13 [Data science with the life ]



* ApplyBQSR

  \> BQSR의 두 번째 과정으로  recalibration table을 기반으로 input 된 bam file을 recalibrated BAM file로 output 한다.

  \> https://gatk.broadinstitute.org/hc/en-us/articles/360050814312-ApplyBQSR



\>> BaseRecalibrator로 recalibrarion 모델을 만들고 ApplyBQSR로 score를 재조정한다.

\>> Base quality score는 시퀀싱 머신에서 각 base마다 발생하는 error의 추정치를 의미하며,  Base Quality Score Recalibrarion은 이러한 각각의 base score를 다시한번 Recalibration하여, 좀 더 정확한 base quality score를 부여하는 과정이다.

https://bioinformaticsandme.tistory.com/77



* GATK HaplotypeCaller 실행

  \> 실행하여 vcf file을 생성
  
  \> Call germline SNPs and indels via local re-assembly of haplotypes
  
  \> https://gatk.broadinstitute.org/hc/en-us/articles/360050814612-HaplotypeCaller



* Variant Filtering

  \> Filter variant calls based on INFO and/or FORMAT annotations

  \> https://gatk.broadinstitute.org/hc/en-us/articles/360050815032-VariantFiltration

<br>

## VEP annotation 실행







<br>



```
java -jar picard.jar AddOrReplaceReadGroups I=star_output.sam O=rg_added_sorted.bam SO=coordinate RGID=id RGLB=library RGPL=platform RGPU=machine RGSM=sample 

java -jar picard.jar MarkDuplicates I=rg_added_sorted.bam O=dedupped.bam  CREATE_INDEX=true VAL
```





<br>

## Annovar Annotation

> https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/
>
> 논문 중 필요한 부분 발췌 

The tab-delimited output file contains a line combining the original input information with additional annotation information, such as ‘exonic’ (genomic function), ‘NOC2L’ (the gene being affected), ‘[NM_015658](https://www.ncbi.nlm.nih.gov/nuccore/NM_015658)’ (the transcript being affected), ‘synonymous SNV’ (functional role of the coding variant), ‘c.2334G>C’ (the nucleotide change in the transcript) and ‘p.A328G’ (the amino acid change in the protein).



In general, ANNOVAR offers three types of annotations: gene-based annotation, region-based annotation and filter-based annotation ([Fig. 1](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/figure/F1/)). Gene-based annotation refers to the manner in which a variant affects known genes, by inferring several types of information: (i) whether the variant is exonic, intronic, splicing, 3′-untranslated region (UTR), 5′-UTR, intergenic, etc.; (ii) when the variant is exonic, what functional role it has on protein coding—synonymous, nonsynonymous, frameshift insertion/deletion, etc.; and (iii) what transcripts are affected and what changes occur in the corresponding amino acids. Region-based annotation can be useful when we want to know whether the variants overlap with certain regions of interest, such as conserved genomic elements[28](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R28), cytogenetic bands, microRNA target sites[29](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R29) and Encyclopedia of DNA Elements (ENCODE)-annotated regions[30](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R30). Furthermore, we can use filter-based annotation to annotate and filter a list of variants, such as finding the alternative allele frequency for variants in the 1000 Genomes Project[31](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R31) and the US National Institutes of Health–National Heart, Lung, and Blood Institute (NIH-NHLBI) ESP6500 exome-sequencing project[32](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R32); finding the SIFT[33](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R33) and PolyPhen[34](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R34) scores for nonsynonymous variants; and identifying the presence or absence of a variant in the dbSNP database[35](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4718734/#R35).



The three different types of annotations supported by ANNOVAR are gene-based, region-based and filter-based annotations. Different annotations focus on different aspects of each variant: gene-based annotation tells its relationship and functional impact on known genes; region-based annotation tells its relationship with different specific genomic regions, such as whether it falls within a known conserved genomic region; and filter-based annotation gives a variety of information on this variant, such as population frequency in different populations and various types of variant-deleteriousness prediction scores, which can be used to filter the common and probably nondeleterious variants.



<br>

## VCF -> whatshap -> shapeit -> genotycaller

먼저 Creating phased references in FASTA format 하기

```bash
bcftools consensus -f hg38.fa compressed.vcf.gz > consensus.fa
```

https://www.biostars.org/p/317218/

```bash
bcftools consensus -H 1 -f reference.fasta phased.vcf.gz > haplotype1.fasta
```



haplotypecaller를 통해 vcf 생성 후

bcftools를 통해 vcf를 GT 부분만 추출해서 

추출한 vcf file을 whatshap으로 pahsing 한 후 

bcftools로 merge 해서 

shapit으로 pahsing 한 후

genotpyecaller로 완성









<출처>

http://www.incodom.kr/GATK

https://2wordspm.com/2019/09/23/ngs-dna-seq-pipeline-gatk-best-practice-code-part1-fastq-to-bam/

https://www.nature.com/articles/s41598-020-70527-8

https://solution4u.tistory.com/13

https://blog.naver.com/gieyoung0226/221942012994





https://discussions4562.rssing.com/chan-67237868/all_p88.html

https://qcb.ucla.edu/wp-content/uploads/sites/14/2016/03/GATK_Discovery_Tutorial-Worksheet-AUS2016.pdf

https://yeamanlab.weebly.com/uploads/5/7/9/5/57959825/snp_calling_pipeline.pdf



```
java -jar GenomeAnalysisTK.jar -T VariantFiltration -R hg_19.fasta -V input.vcf -window 35 -cl
```

```
java -jar GenomeAnalysisTK.jar -T VariantFiltration -R hg_19.fasta -V input.vcf -window 35 -cluster 3 -filterName FS -filter "FS > 30.0" -filterName QD -filter "QD < 2.0" -o output.vcf 
```

```
java -jar GenomeAnalysisTK.jar -T SplitNCigarReads -R ref.fasta -I dedupped.bam -o split.bam -rf ReassignOneMappingQuality -RMQF 255 -RMQT 60 -U ALLOW_N_CIGAR_READS
```