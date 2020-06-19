# 전사체 분석 기초 이론

이 강의는 KGOL(K-Genome online Lectures)에서 진행하는 전사체 분석 기초 이론(강사: 김경윤 (주)인실리코젠)을 수강하며 작성한 내용입니다.

내용과 사진 대부분 동영상에서 보여준 ppt를 캡쳐한 것입니다.

<br>

## 제 1강 전사체 분석 기초편

전사체(transcriptome) 분석 개론

> 1. 생물정보 입문을 위한 기초 이론과 용어 설명
> 2. 교육의 특징

### 01. 전사체의 개요

**전사체의 정의**

Transcript(전사) + -ome(체, 집합) = Transcriptome(전사체)

전사체 = RNA 집합 (mRNA, long non-coding RNA 등 모든 RNA 포함)

전사체 분석 = 표현형질 특성, 조절 기능 이해, 질병 생물학적 기능 예측 등 여러 측면에서 중요

<br>

**세포 내 RNA 조성**

<br>

**유전자의 발현**

alternative RNA splicing 을 통해 여러가지 단백질 생성

기능적 유전자 산물을 만들어 내는 과정을 gene expression

<br>

**왜 RNA 서열에 집중하는가? (vs. DNA)**

* 기능적 연구가 가능

  > 유전체는 일정하지만, 유전자의 발현은 실험 조건에 뚜렷한 영향을 미침

* 일부 분자의 특성은 RNA 수준에서만 관찰할 수 있음

* 유전체 서열로부터 전사체 서열을 예측하는 것이 어려움

* 단백질 서열에 영향을 미치지 않는 돌연변이 해석이 가능

* 단백질 코딩 체세포 돌연변이의 우선 순위를 결정 가능

<br>

**전사체 분석을 통해 어떤 사실을 밝힐 수 있는가?**

> 새로운 참조 전사체 조립부터 기능 분석까지

* 참조 유전체 / 전사체가 없을 때 새로운 전사체를 조립
* 새로운 isoform 검색
* 전사체의 기능 검색과 분류
* 발현량 측정 및 서로 다른 환경에서 차등적으로 발현되는 유전자 분석
* 유전자의 온톨로지나 경로 확인

<br>

### 02. NGS와 전사체 연구 기술의 발전

**전사체 연구의 발전**

* 1990년대(1995년) 일부 RNA sequencing 하는 Gene expression profiling -> microarray 분석을 통해 시작
* 2008년 전후 NGS(차세대 염기서열 분석법) 대두 되면서 염기서열 분석 기술에 대한 혁신이 일어남 

<br>

**시퀀싱 기술의 진화**

* 1990년대는 하루에 1000개 정도 전기영동 분석을 통해 진행
* 자동화 기계가 생산되면서 100만개 부터 하루에 수억개 염기를 읽을 수 있는 시대가 됨

<br>

**대용량 시퀀싱 기술의 발달**

* 1세대: 생어
* 2세대: NGS
* 3세대: single moluecule을 sequencing 하는 방식으로 Nanopore 등 기술 장비가 있음
* 4세대: 반도체와 같은 형태로 seq 진행하는 새로 기술을 만들어 내고 있음

<br>

**NGS의 시작**

> Next Generation Sequencing

* Roche - GS FLX
* IIIumina - Genome Analyzer
* ABI - SOlid

=> NGS는 생어 seq과 다른 방식으로 진행

<br>

**NGS 플랫폼의 비교**

초창기 3개 장비에서 부터 많은 장비들이 출시가 됨

연구하는 방향이 비슷한 장비를 선택해서 연구해야 한다.

<br>

**NGS 응용 분야**

* DNA Level
* `RNA Level`
* Epigenetic Level
* Protein Level

<br>

**Microarry 원리**

* probe 형태와 design에 따라 유전자 발현 분석/유전자 염기서열 변이 분석할 수 있는 기술

* 주로 Gene expression Array는 mRNA를 분석

* Bid 사용하여 발현 측정

* MicroRNA와 non-coding RNA 에도 사용 가능

* 유전자 발현을 보려면 control과 실험 조건 sample에서 칩상에 얹어서 hybridization하면 색상으로 발현 차이를 알 수 있음

* 색상의 차이를 가지고 발현양을 계산

* Two color array와 One color array 방식이 존재

> 순서
>
> 1. Two color array **OR** One color array
> 2. Feature extraction
> 3. Quality Control
> 4. Normalisaion
> 5. Differential Expression analysis
> 6. Biological interpretation
> 7. Submit data to public respository 

<br>

**Microarray 한계점**

> Hybridization 기반의 접근은 높은 처리량과 상대적으로 저렴하지만 아래와 같은 제한 사항이 있다.

* 알려진 유전체 정보를 기반으로 해야함
* 교차-하이브리드화 (hybrid-hybridization)로 인해 바이어스를 받아 높은 백그라운드 레벨을 처리함
* 백그라운드 및 채도 시그널에 의해 제한된 발현 범위의 감지
* 여러 실험 조건에서의 발현 수준을 비교하는 것이 어려워서 복잡한 정규화 방법이 필요할 경우 있음

<br>

**RNA-seq원리**

2000년대 부터 시행하고 있는 RNAseq 방법은 전사체 염기서열 자체를 인식하는 방식

모든 전사체 발현량을 상대적으로 기술 가능

<br>

**RNA-seq 장점 (vs.Microarray)**

* 전사체가 검출 가능한 낮은 수준에서 발현된 유전자에 대한 감도 호가인 가능
* 기술적 변형이 적고, 재현성이 높음
* 이미 밝혀진 유전체 정보에 제한되지 않음(아직 유전체가 완성되지 않은 종에서 수행 가능)
* 새로운 전사 영역, 대립 유전자 특이적 발현과 같은 전사적 특징에 대한 정보 제공
* Microarray와 같은 바이어스 문제가 없음
* 다양한 응용 분석 가능: alternative splicing events, disease-associated single nucleotide polymorphisms (SNPs), gene fusions, single-cell RNA-seq 등

<br>

**Microarray와 RNA-seq의 종합적 비교**

* Microarray는 전자 chip상에 상보적 서열을 두고 연구하는 방식이라 한계 존재
* RNA-seq는 더 넓은 범위를 연구 가능
* RNA-seq는 isoform 전사체에 대해 새로 연구 가능

<br>

**전사체 연구를 위한 분석 기술**

* Serial/Cap analysis of gene expression

* Expressed Sequence Tag

* Microarray

* `RNA-seq`: 현재 가장 많이 사용

<br>

### 03. RNA-seq의 최신 동향

**Small non-coding RNAs(smRNAs)**

> 단백질 번역이 되지 않고 다양한 생물학적 기능을 하는 RNA를 의미
>
> 치료제 역할 / 잠재적인 bio marker로 역할

<br>

**Single Cell RNA Sequencing Workflow**

> 가장 활발히 연구되는 분야

* Solid Tissue
* Dissociation
* Single Cell Isolation
* RNA ; RT & Second-strand Synthesis
* cDNA =>IVT => Amplified RNA => RT PCR => Amplified cDNA **OR** 바로 cDNA에서 PCR => Amplified cDNA
* Amlified cDNA
* Sequencing Library
* Sequencing
* Single-cell Expression Profiles => Clusering
* Cell Types Idenrification

<br>

**Difference between bulk RNA-seq VS singel cell RNA-seq**

<img src="C:\Users\Owner\Desktop\캡처.PNG" style="zoom: 67%;" />



![Single Cell Set-Up: Sample Preparation Tips | Biocompare: The ...](https://media.biocompare.com/m/37/article/o/345311fig1.png)

* 평균을 구하는 부분이 다름
* 조직에서 발현되는 일반적인 특성을 확인하기 위해 sample을 덩어리 채로 seq 진행
* 유전자 발현 값을 확인을 할 때 확연히 다름
* 비슷한 cell 끼리 population을 볼 수 있는 것이 single cell RNA-seq

<br>

**Summary**

1. 전사체란, 모든 RNA 분자의 집합을 의미하며, 표현형질의 특성과 그 조절기전의 이해, 유전정보의 기능 분석, 질병이나 생물학적 기능 이상의 예측 등 여러가지 측면에서 중요한 의미를 가진다.
2. 전사체 분석을 위하여 Microarray와 RNA-seq 기술이 대표적으로 이용되고 있으며, 장단점을 이해하고 연구 목적에 맞는 기술을 활용하자.
3. 최근 small RNA-seq과 single cell RNA-seq과 같은 기술도 전사체 분석을 위해 이용되고 있다.

<br>

## 제 2강  RNA-seq 실험 디자인과 NGS 시퀀싱

> RNA-seq 분석 파이프라인
>
> RNA-seq을 위한 실험 디자인 고려사항
>
> NGS 라이브러리 구축 및 데이터 생산

### 01. RNA-seq 분석 파이프라인

1. Pre-analysis

* 실험 디자인과 seq 디자인

* 핵심 분석 단계 준비

2. Core-analysis

* Transcriptome profling을 통해 DEGs 분석하여 해당하는 유전자 기능 profile 까지 진행

3. Advanced-analysis

* Visualization
* Other RNA-seq
* Integration

![img](http://www.ibric.org/upload/geditor/201706/0.25927700_1498615114.jpg)

<br>

**Step 1 - 실험 디자인과 NGS 시퀀싱**

> 실험 디자인 중요

1. NGS sequencing

* Paired end sequencing(HiSeq) 진행

* Total RNA 양을 확인 하는 것이 중요

**Step 2 - 데이터 전처리**

2. Preprocessing

* Adapter trimming
* Quality trimming

**Step 3 - NGS reads 맵핑**

3. Mapping

* 참조 유전자에 mapping 하고, sample 별로 expression profiling을 준비하는 작업

**Step 4 - 발현량 계산**

4. Estimate expression abundance

* Mapping 된 read 들을 바탕으로 해서 발현양 값을 구하는 방식이 진행
* Transcript 기기에 따라 발현양 계산 오류를 줄이기 위해 여러가지 계산 방식이 존재
* 가장 많이 활용하는 방식이 RPKM(Read Per kilbase per Million mapped reads)
* RPKM = C/LN

> C: Number of mappable read on a feature
>
> L: Length of feature (kb)
>
> N: total number of mappable reads (millions)

* 전반적으로 계산 공식을 통해 sample 별 해당 유전자가 얼만큼 발현 됬는지 숙지

**Step 5 - 차등발현 유전자 추출**

5. DEG selection

> Test for differential expression

* edgeR
  * Variation of Fisher exact test / TMM
  * Returns exact P valude from derived probabilities 
* Cuffdiff
  * Ratio of normalized counts between 2 conditions
  * t-test to calculate P value / FPKM

6. Functinoal annotation

* 어떤 기능과 연관관계가 있어, 생물학적 의미를 가지고 있는지 확인

<br>

### 02. RNA-seq을 위한 실험 디자인

**RNA-Seq 실험 디자인 예시**

1. 관찰

2. 실험 목적 설정

* 가설을 통할 실험 목적을 설정
* ex) 갈색지방세포 분화 감수성과 연관된 유전적 요인은 무엇일까?

3. 실험 디자인 및 수행

* Low thermogenic activity / High thermogenic activity

<br>

**실험 디자인을 위해 먼저 생각해보아야 하는 것**

* 최종 실험 목적이 무엇인가?

  > Trnascriptome assembly?
  >
  > Differential expression analysis?
  >
  > Idenrify rare transcripts?

* 어떤 성격을 지닌 실험군 인가?

  > Large, complex genome?
  >
  > Introns and high degree of alternative splicing? (To detect splicing)
  >
  > No reference genome or transcriptome> (To find novel genes / transcipts)  

<br>

**실험 디자인 고려사항**

* Biological consideration: 실험이 생물학적으로 어떤 고려사항을 가지고 있는 지에 따라 결정
* Economical Considerations: sample 개수가 예산 안에서 결정되기 때문
* Technical Considerations: Skills / Hardware

등등이 존재

<br>

**샘플의 반복 수**

기술 적인 변수: 하나의 sample을 여러번 반복하여 replicate

생물학적인 변수: 우리가 통제할 수 없는 부분이기 때문에 반복 실험이 필요

<br>

* Biological replicates
  * 진정한 생물학적 반복 수에 대해 지속 논쟁이 계속되고 있음
  * 반복 수는 가능한 한 높아야 함
  * RNA-seq 실험에서 최소 3번의 반복 실험이 포함되는 것을 추천
* Technical replicates
  * 동일한 RNA 샘플에서 다른 라이브러리 준비
  * PCR 증폭 등과 같은 라이브러리 제작 단계에서의 batch 효과 고려
  * 동일한 플로우 셀을 사용
  * 대부분 시퀀싱 프로토콜에 의한 기술적 변동은 매우 낮고, 잘 제어되는 편

<br>

**라이브러리 구축**

> 시퀀싱을 위해 cDNA 라이브러리를 준비하는 단계(플랫폼마다 다를 수 있음)

1. RNA isolation: RNA는 조직으로부터 단리되고 DNase와 혼합된다.
2. RNA selection / depletion: 관심있는 신호를 분석하기 위해 단리된 RNA를 그대로 유지하고, 폴리 A 꼬리를 갖는 RNA에 대해 여과하여 mRNA만을 포함하고, rRNA를 제거시키는 것과 같이 RNA를 여과한다.
3. cDNA synthesis: DNA가 증폭하는데 더 안정적이고 성숙한 DNA 시퀀싱 기술을 활용하므로 RNA는 cDNA로 역전사함.

<br>

**시퀀싱 서열의 길이**

> 시퀀싱 길이 고려 (긴 리드가 isoform 분석에 좋음)

* 단일 방향(single end: SE) 또는 양방향(Paired-end: PE)으로 fragment 읽는 방법이 있음
  * IIIumina 장비 많이 사용
* PE가 de nove transcript discover와 isofrm 발현 분석에 더 유리함
* 최상의 sequencing 조건은 분석 목표에 따라 다름
* 상대적으로 더 저렴한, short SE read 방법은 transcriptome이 잘 정리된 생명체의 유전자 발현 분석을 위해서 충분한
* 긴 read와 PE read 방법은 transcriptome이 잘 정립되지 않은 생명체를 분석할 때 더 좋은 결과를 얻을 수 있음

<br>

**시퀀싱 depth**

> 시퀀싱 뎁스 고려(라이브러리를 얼마나 많이 읽을 것인가 고려)
>
> 500만 read면 충분하다

* 라이브러리 크기 (각 샘플의 sequencing된 read 수)
* 샘플을 더 많이 읽을수록 더 많은 transcript 발견과 정량이 정확함
* 최적의 sequencing depth는 실험의 목적에 따라 다름 - 전사체의 목잡성에 달려있음
* 알려진 실험 결과들은 sequencing depth가 높을수록 정량과 검출에 유리하지만 off-targer과 noise를 증가시키는 단점도 존재함.

<br>

### 03. NGS 시퀀싱

**RNA 추출 및 quality check**

* RNA quality가 좋아야 sequencig에도 좋아진다.

* RIN = RNA integrity number ; 0(안좋)~10(좋)

  > 10이면 RNA 크기에 맞춰 peak가 단일그래프로 나온다
  >
  > 1이면 peak가 복잡하게 나온다.

<br>

**라이브러리 준비**

<br>

**NGS 시퀀싱**

> 어떤 Application 과정을 진행하느냐에 따라 장비를 선택하여 sequencing을 진행한다.

1. DNA/RNA Samples  => Quality Control Metrics
2. Libraray Construction => Quality Control Metrics
3. Sequencing (ex. IIIumina HiSeq2500 **OR** IIIumina MiSeq를 장비로 사용)

<br>

### 04. NGS 데이터 품질 검사

> 데이터를 생산하고 나면 품질 검사를 진행해야 한다.

<br>

**NGS 시퀀싱**

1. Experimental design => untreated  / treated
2. Isolate RNA
3. Prepare library
4. Sequence
5. FASTQ files => Single reads / Paired end reads

![image-20200619131634013](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619131634013.png)

<br>

**NGS 데이터 생산**

* FASTQ format: 시퀀스와 quality score를 모두 결합한 시퀀싱 데이터를 공유하기 위한 공통 파일 형식

![image-20200619132009027](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619132009027.png)

<br>

**시퀀싱 데이터의 품질 검사**

* Sequencing 오류, contamination 확인
  * 염기서열 quality (% Q30 (phred score) 정도면 굉장히 좋은 quality)
  * GC content, adaptor 유무, 지나친 k-mer 존재 유무, duplicated read 등 확인
  * 허용 범위의 duplication, k-mer, GC content 정도는 실험과 생명체에 ㅡ이해 결정되나 이 값들은 동일 실험에서는 샘플 별로 균일해야만 함
  * Trimming 과정 포함 등 진행

<br>

**Summary**

1. RNA-seq 분석은 RNA 추출 후 NGS 시퀀싱을 통해 대용량 염기서열을 생산한 다음, 참조 유전체에 서열을 맵핑하여 발현량을 계산하여 차등발현 유전자를 확인한다.
2. 실험 디자인을 할 때 고려사항은 연구대상의 유전학적인 이슈, 생물학적인 실험과정을 검토하여 샘플의 반복 수, 시퀀싱 라이브러리 구축, 시퀀싱 depth등을 고려하여야 한다.
3. NGS 플랫폼으로부터 생산된 raw 데이터는 fastq 포맷으로 서열을 퀄리티를 기반으로 하여 품질검사를 통해 고퀄리티의 서열들을 바탕으로 분석을 시작할 수 있다.

<br>

## 3강 Reads mapping과 발현 프로파일

> Reference 유/무에 따른 reads mapping
>
> Expression level 계산 방법

<br>

### 01. Reads mapping

**RNA-seq 분석 로드맵**

> 2강에서 pre-analysis에 관해 설명
>
> 3강 부터 core-analysis: low data를 reference에 mapping하여 발현양 조사
>
> 4강 발현된 유전자의 level을 가지고 차등발현 유전자 확인

<br>

**RNA-seq 분석의 유형**

Reference genome-based

* RNA-seq 실험이 수행되는 종에 대해 이미 알려진 유전체 정보가 제공되는 경우
* 이를 통해 리드가 참조 유전체에 맞춰 정렬되고 transcript를 재구성하는 능력이 크게 향상됨
* Human을 포함한 대부분의 model organism에 해당

<br>

Reference genome-free

* 분석 하고자 하는 종에 대한 유전체 정보가 없는 경우
* 이 경우 de novo 접근 방식을 사용하여 리드를 assemble하는 단계가 필요함
* 이러한 유형의 RNA-seq는 파라미터에 의존적이고, 좋은 결과를 내기가 어려움

<br>

**RNA-seq 맵핑의 3가지 전략**

1. Align to reference genome
2. Align to transcriptome: transcript 수준의 정보가 공개 되어 있으면 gene 모델을 입혀서 sequencing data를 입히면 profile이 가능하게 된다.
3. De novo assembly: sequencing된 read들 가지고 de novo를 통해 transcript의 참조 유전체를 만들고 이를 바탕으로 law data mapping 하고 profile

<br>

**전사체의 de novo assembly**

![image-20200619135234678](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619135234678.png)

<br>

**맵핑을 위해서 어떤 read aligner를 사용해야 할까?**

<RNA-seq 데이터 분석을 위한 모범 사례>

![image-20200619135521595](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619135521595.png)

<br>

**새로운 transcript 검출**

Short read를 이용하여 novel transcript 검출하는 것은 RNA-seq 주요 도전 과제 중 하나

* Short read는 splice junction에 위치하는 경우가 적기 때문
* PE read와 높은 coverage는 소량 발현되는 유전자의 재구성에 유리
* 반복 실험은 false-positive를 줄이기 위해 중요함

 <br>

**Reads 맵핑 결과 - SAM/BAM**

SAM file: sequence 정보를 저장하는 파일, fast 포맷과 다르게 reference seq mapping 한  정보이기 때문에 reference seq 정렬 하고 있음, TEXT 형태

BAM file: SAM과 동일한 동일하나 Binary 형태로 되어있음, 압축 효과

<br>

**맵핑 데이터의 품질 확인**

> 최대한 clean data를 얻어 mapping을 진행하는데 이때, 비율을 가지고 데이터 품질을 확인

Check list

* Clean read statitics
* Mapping rate(% input clean read)
  * Reference species (85~95%)
  * De novo(70%~)

<br>

### 02. Expression Quantification

**발현량은 어떻게 확인하는 것인가?**

RNA-seq의 일반적인 응용은 gene 또는 transcript 발현을 측정하는 것

* 이 응용의 기본이 되는 값은 각 transcript에  mapping된 read 수
* 발현이 많이 되면 seq 된 read도 많다 라는 가정하에 read들의 개수로 발현양을 이야기 함

<br>

**발현량 계산의 문제**

각 transcript에 mapping된 read의 수 = 유전자의 exon region의 read, fragments 수

그러나, 다음과 같은 문제점 발생

* 실제로 library 사이즈가 굉장히 크거나 gene이 커서 여러개의 exon을 가지고 있는 경우 굉장히 seq을 많이 하면 비율 적으로도 seq 많이 됬을 것이고, gene이나 transcript가 길면 길 수록 read도 많이 생산될 가능성을 커버 하지 못함
* 유전자의 길이가 달라 오류가 생기는 부분을 보정 필요

<br>

**발현량 계산의 정규화**

![image-20200619142405950](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619142405950.png)



RPKM/FPKM 등을 사용

<br>

**RPKM/FPKM**

![image-20200619142652223](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619142652223.png)



=> pair and sequencing을 하면 forwaed / reverse 방향 한쌍의 pair로 mapping되어 진행되기 때문에 하나로 인정하게 됨

<br>

**Counts to expression levels**

1. RPKM/FPKM(Reads (Fragments) Per Kilobase per Million(mapped reads))
   * 샘플간, 혹은 샘플에서 유전자들 간의 비교를 위해 고안됨
   * 인용수가 엄청난 이 논문은 Sequencing depth와 Gene, transcript의 길이에 대해서 raw count들의 수치를 보정
   * 가장 많이 사용된다.
2. TPM (transcripts per million)
   * R/FPKM과 비슷하지만 RNA population에서 transcript length의 분포까지 설명
   * 이 방법 없이는, 다른 transcript length distribution을 가지는 두 RNA pools를 비교할 때 bias가 생길 수 있음
3. TMM (Trimmed menas of M values)
   * TPM과 마찬가지로 비교하고자 하는 RNA pools의 다른 조성에 대해 보정
   * TMM은 TPM이나 R/FPKM과 다르게 하나의 sample에서는 사용될 수 없음. 샘플 간의 normalization method

<br>

**발현량 분석을 위한 소프트웨어**

![image-20200619143517731](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619143517731.png)

<br>

**재현 가능성의 확인**

> 발현양 계산하면 차등 유전자 계산 하기 전, 재현 가능성 확인 방법도 중요
>
> 실험 디자인을 위해 반복 실험을 진행했는데, technical한 replication 부분은 재현 가능성이 높고, bilogical replication이 중요하다. 실험 조건 별 유전자 차이가 있다면 같은 조건의 biological replicate는 PCA 분석 시 뭉쳐 나타나야지만 실험적으로 진행했던 반복 수가 의미가 있다는 것
>
> PCA 그래프를 그려 3개의 반복 SAMPLE 들 간 Sample들이 멀리 떨어져 있다. 이러한 의미는 이 그룹의 유전자 발현은 크게 의미가 없을 수 있다.
>
> Mapping 이후에 재현 가능성을 확인 할 수 있는 PCA 그래프 혹은 clustering 하여 샘플들 간 유사성 확인하여 아웃라이어 샘플들을 제외해야지만 DEGs를 뽑아내는 데 있어 도움이 된다.

<br>

**Summary**

1. RNA-seq 데이터의 reads 맵핑은 참조 유전체 정보의 유/무에 따라 분석 방법이 다르다.
2. 맵핑 결과는 SAM/BAM 파일로 생산된다. 더불어 맵핑된 리드의 비율을 확인하여 품질을 검사한다.
3. 맵핑된 리드를 바탕으로 유전자 발현량을 계산할 때는 normalization을 수행하여야 샘플 간 발현 비교 분석이 가능하다.
4. PCA 분석을 통해 biological replicate의 재현성 확인이 필요하다.

<br>

## 4강 차등발현 유전자(DEG) 발굴

> Differential Expression 테스트 통계 모델
>
> 클러스터 분석 및 시각화

<br>

### 01. 차등발현 유전자(DEG) 분석

> Core-analysis 중에 Differential expression 해당하며 핵심 단계이다.

<br>

**차등 발현 유전자의 정의**

> 특정 실험 조건에서 어떤 유전자들의 발현이 높아지거나 낮아질까?

* Differential Expressed Genes (DEGs)는 다른 실험 조건 사이에서 유의하게 차등적으로 발현하는 유전자들을 의미함

![image-20200619144839745](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619144839745.png)

<br>

**얼마나 발현이 차이가 날까?**

유전자 발현량은 두 조건에서 서로 비교하려 할 때 Fold change 값을 이용하고 있다.

얼마나 차이가 나는지 확인 할 수 있다.

DEG 찾는데 의미 있는 값이다.

<br>

**Fold change**

![image-20200619145632570](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619145632570.png)

* 두 조건의 값이 같으면 FC값이 1이 된다.

* 1을 기준으로 해서 비대칭 값을 가진 것이 문제가 되는데, 이를 해결하는 방법 중 하나는 FC를 사용하지 않고 log2FC 값을 사용하여 대칭적인 값을 얻고, 음수가 나올 수 있도록 계산하는 방법을 이용하고 있다.

<br>

**통계적으로 유의한 차등 발현 유전자의 검출**

![image-20200619145924916](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619145924916.png)

<br>

**Hypothesis for DE testing**

![image-20200619150347723](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619150347723.png)

* 두 가지 가설을 채택하여 gA: A 조건에서 g 유전자 평균발현 (B는 B조건)
* h0는 귀무가설 => 두 유전자의 발현이 같다
* h1 는 두 유전자의 발현이 다르다
* Test 진행하여 p-value 값을 통해 하나의 가설 기각

<br>

**Multiple testing correction**

1. Bonferroni (FWER) => 가장 많이 사용 
2. Turkey
3. Dunkan
4. FDR => 유의하다고 판단되는 것들 중에 틀릴 확률을 고정시키는 새로운 p-value를 정의하는 방법으로 이 또한 많이 사용함

![image-20200619150725011](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619150725011.png)

<br>

**차등 발현 유전자 검출을 위한 프로그램**

![image-20200619150944466](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619150944466.png)

<br>

**차등발현 유전자 발굴을 위한 프로그램의 비교**

![image-20200619151052897](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619151052897.png)

* 어떤 통계 모델을 사용하는지, 어떤 데이터를 사용하는지 확인하고 실험과 맞는 DEG 프로그램을 사용하자 

<br>

**차등 발현 유전자 분석 결과**

![image-20200619151259605](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619151259605.png)

* DEG list는 최종적으로 유의한 list만 선별하여 이후의 분석으로 활용하는 데이터로 생산될 것

<br>

### 02. 클러스터(Cluster) 분석

**다양한 샘플에서의 DEG 패턴을 확인하고 싶다면?**

* Hierarchical clustering (HCL), where the distance between elements is used to build the clusters.
* K-means clustering (KMC), where clusters are represented by a vector. The number of clusters is fixed and elements are assigned based in its distance to the vector

control vs case 이런 단순한 분석이 아니라 다양한 처리 군을 통한 조건일 경우 차등발현 유전자의 비슷한 패턴 확인하기 위해서는 군집화 방법이 필요한데 대표적인 것이 HCL, KMC 두가지 방법이 있다.

<br>

 **Hierachical clustering**

![image-20200619151814141](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619151814141.png)

* 샘플들 간의 유사성을 평가하는 유사성 척도를 정해서 군집화 하는 방법
* heatmap을 통해 색으로 구분, 군집의 의미있는 부분을 패턴으로 확인

<br>

**K-means clustering**

![image-20200619152024754](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619152024754.png)

* 최종 군집의 갯수를 정해서 최종 군집으로 나눌 수 있는 모든 가능한 방법을 점검하여 최적화된 군집을 형성하는 방법
* 최종적으로 4개의 패턴을 보여주세요 주문하면 전반적인 실험 조건에 따라 유전자 발현 패턴 분석을 통해 크게 4개 그룹으로 나눠 보여주는 방식
* 시계열 분석에 더 효과적으로 활용된다.

<br>

**분류(Classification)와 군집(Cluster)**

보통 분류와 군집은 둘다 생물통계학에서 패턴 인식 방법들로 군집화 하는 방법

분류 같은 경우 sample group 정보가 이미 정해져 있어, 그룹 별로 나누는 규칙을 찾는 방법

군집은 group 정보가 정해져 있지 않기 때문에 유사한 sample 을 묶어 group을 만들어 가며 group 나누는 규칙을 찾는 방법

<br>

**Summary**

1. Differential Expressed Genes (DEGs)는 다른 실험 조건 사이에서 유의하게 차등적으로 발현하는 유전자들을 의미한다.
2. 다양한 통계 테스트를 반영하여 유의한 DEG를 선별하는 것이 중요하다.
3. 클러스터링 방법을 이용하면 다양한 샘플 조건에서의 DEG 패턴을 확인할 수 있다.

<br>

## 5강 차등발현 유전자의 기능적 해석

> Gene Set Enrichment Analysis 개념
>
> Pathway 분석 툴

<br>

**RNA-seq 분석 로드맵**

core-analysis 중 Interpretation 과정으로 Functional profiling (annotation) 이다.

선발한 차등발현 유전자가 특정 조건에서 왜 발현 차이가 나는지가 생물학적 의미를 찾기 위한 단계로, 생물학적 기능과 네트워크 정보를 포함하고 있는 다양한 데이터로 부터 정보를 입히는 과정이다. 

다양한 데이터를 활용하는 것이 관건이다.

<br>

**GSEA(Gene Set Enrichment Analysis)**

발현값의 차이가 유의하게 나타나는 사전 정의된 gene set을 통계적으로 찾는 방법

* 사전 정의된 gene set가 두 생물학적 상태 사이에서 통계적으로 유의하고 일치하는 차이를 타나내는지 여부를 결정하는 계산 방법

사전 정의된 gene set?

* 동일한 기능 혹은 생물학적 경로에 관여하거나, 염색체 상에서 가장 가까운 위치에 있는 유전자들을 문헌이나 데이터베이스를 기반으로 묶음
* 사전 정의된 gene set들은 MSigDB(Molecular Signatures DataBase)에 있음

![image-20200619153413403](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619153413403.png)

<br>

**Gene Ontology(GO)**

* 유전자 온톨로지 (GO) 지식베이스는 유전자 기능에 관한 세계 최대 정보원
* 모든 유전자에 ID가 할당되고, 서로 다른 종간의 비교나 단백질의 비교를 통해 진화 관계나 그 기능을 유추하는데 도움이 되도록 도움
* 생물학적인 기능에 대해 계층적으로 배열된 일련의 개념과 그 관계를 정립
* 세계적으로 혼돈을 피하기 위해 국제 협의체(consortium)를 구성하여 모든 연구자들이 일관되게 사용할 수 있도록 정의한 공통 통제용어 체제

<br>

**Concepts in GO**

유전자가 관련된 세포 기작, 유전자가 가지는 분자 기능, 유전자가 가지는 세포 내 위치  구조화 해서 카테고리를 분류

![image-20200619153932550](C:\Users\Owner\AppData\Roaming\Typora\typora-user-images\image-20200619153932550.png)

<br>

**DAVID**

Functional annotation

* DAVID는 웹 기반의 annotation tool
* 기능적인 annotation에 국한되어 있는 것이 아니라, 여러가지 bioinformatics에 필요한 tools을 제공
* DAVID 2.x는 새로 개발된 "DAVID Gene Concpt"라는 것을 기반으로 가장 크게 통합된 knowledgebase를 포함하고 있음
* Functional annotation에 국한 된것이 아니라 Gene Set Enrichment Analysis 나 GO 데이터 베이스에 연관하여 사용한다던 지 툴 이 포함되어 있기 때문에 기능 연구에서 많이 사용하는 툴로 사용되고 있다.

<br>

**OmicsBox**

Read부터 annotation까지 간편하게 분석

* 7000개 이상의 과학 논문 인용 횟수를 가진 Blast2GO 소프트웨어 기능이 추가됨
* BLAST와 InterProScan과 함께, 여러 functional annotation으로 단백질 기능 예측

<br>

* Gene Ontology mapping
* Blast2Go annotation
  * COG
  * KEGG pathway
  * Enrichment Analysis

<br>

**KEGG pathway**

분자 상호 작용, 반응 및 관계 네트워크에 대한 지식을 타나내는 데이터베이스

* 유전자, 단백질, RNA, 화학 화합물, 글리칸, 화학 반응과 함께 질병 유전자와 약물 표적을 포함한 많은 요소를 통합하는 경로 지도의 모음
* 일반적인 경로 지도 외에도 대사의 전반적인 그림을 보여주는 글로벌 맵도 포함
* 유사한 기능이나 형태를 나타낼 뿐만 아니라, 특정 경로와 관련된 유전자들의 대사 관계 및 경로에 관련된 조절 기능을 수행하는 유전자의 위치나 상호작용들을 나타냄

<br>

**Cytoscape**

상호작용 네트워크 시각화와 다양한 데이터의 통합

* 유전자 상호작용과 생물학적 경로에 대한 네트워크를 annotation, 발현 프로파일 및 다양한 데이터와 통합 할 수 있는 오픈 소스 소프트웨어
* 다양한 표준 네트워크와 주석 파일을 지원
* 20개 이상의 DB, 6000개 이상의 네트워크를 지원
* python, R등 여러 언어로 만들어진 결과 데이터를 삽입할 수 있음
* 200개 이상의 앱을 지원하여 높은 확장성을 가짐

<br>

**IPA(Ingenuity Pathway Analysis)**

생물학적 기능과 pathway, 분자적 네트워크를 시각적으로 표현하는 소프트웨어

* 전문가 큐레이션 DB를 사용
* 관심있는 유전자와 관련된 canonical pathway 가시화
* 관심 있는 pathway의 상위 조절자 검색 기능
* 하위 신호에서 일어나는 변화를 시각화 (질병과 연관된 fuction 확인 등)
* 유전자 간 네트워크 상호작용 분석 기능
* 사용 solution 중 국 내외 모두 많이 사용하고 있는 소프트웨어
* 최종적으로 차등발현 유전자의 생물학적인 의미를 찾을 수 있는 solution

<br>

**Summary**

1. 차등발현 유전자의 생물학적인 기능을 찾는 과정을 functional annotation이라고 한다.
2. Function annotation은 이미 밝혀진 다양한 유전자들의 기능 및 네트워크 정보 등을 활용하여 수행한다.
3. GSEA, GO, KEGG, DAVID 등을 활용한 결과들을 통해 특정 조건에서 차등 발현하는 유전자들의 생물학적 의미를 도출할 수 있다.





