# 1st Study

> 200717 금요일 진행
>
> 김용진 선생님, 이충렬 선생님, 나 참여

<br>

## 사전 공부 및 준비

> Single-end, rpkm
>
> Paired-end, fpkm
>
> Splicing

<Br>

### RNA-seq

RNA-seq이란 transcriptome을 분석하여 발현의 차이를 확인 하는 분석 방법이다. Transcript에서 translation을 통해 단백질이 된다는 [central dogma](http://www.incodom.kr/central_dogma)에 입각하여 transcript 수가 많을수록 발현이 많이 된다고 판단하여 계산하는 방법이다. Transcriptome 대용량 시퀀싱 후 분석을 하는 RNA-seq을 통하여 새로운 것을 발견할 수도 있으며, 발현 값을 정량 할 수도 있다.

<br>

### RNA-seq 기본 원리

샘플에서 먼저 mRNA를 추출한다. 이를 cDNA로 합성한 이후 혹은 RNA 상태에서 조각을 낸 뒤 fragment 양 끝단에 adapter등 시퀀싱에 필요한 서열들을 붙여 라이브러리를 만든다. 그리고 대용량 시퀀싱을 진행한다. RNA에 붙어있는 poly A tail이나 라이브러리를 만들 때 사용된 adapter는 제거가 되고 [read](http://www.incodom.kr/read)라고 불리우는 시퀀싱 데이터가 생성이 된다. 이 샘플이 유래된 생물종(ex. Human, mouse, rat, etc.)의 서열을 reference로 삼아 하여 read를 reference 서열에 붙인다. 이 과정을 [mapping](http://www.incodom.kr/mapping)이라고 한다. 다음은 이러한 과정을 그림으로 나타낸 자료이다.

![RNA-seq](https://drive.google.com/a/insilicogen.com/uc?id=16Qw5blob_oPLxdt9RLRui0ysdYnOtrPU&export=download)

<br>

### NGS 전처리 과정

NGS 전처리 과정은 샘플 DNA 혹은 RNA가 NGS 장비에서 인식하여 염기서열분석을 할 수 있도록 가 공하는 과정이다. NGS 장비가 인식하기 위해서는 보통 샘플 DNA 혹은 RNA를 적당한 크기로 자른 후 양 끝에 특정한 염기서열을 가진 어댑터(adapter) 올리고뉴클레오티드(oligonucleotide)를 붙여준다. 이렇게 만들어진 산물을 라이브러리(library)라고 부른다.

![img](http://media.biocompare.com/m/37/donne-libraryconstruction2.jpg)

라이브러리 제작은 분절화(fragmentation), 말단수리(end-repair), 3′-말단 아데닌 추가(A-tailing), 어댑터 부착 (adapter ligation), 라이브러리 증폭(library amplification)으로 나누어진다. 이러한 과정에서 필요한 구성품들을 하나의 세트로 구성한 상용화된 제품들이 현재 활발히 출시되고 있으며, 일루미나, 써모사이언티픽, NEB 등 글로벌 기업의 제품과 함께 디엑솜(Dxome) 등 국내 기업 제품들이 NGS 검사실에서 사용되고 있다.

<br>



### Sequencing 시퀀싱

> 염기의 순서를 화학적으로 읽어 내는 것을 말하고, '염기서열해독'이라고 한다.

<br>

### Single-end / Paired-end sequencing

> RNA-Seq은 최근에 개발된 deep-sequencing 기술을 사용합니다. 일반적으로 하나의 RNA 집단은 한쪽 또는 양쪽 끝에 adaptor가 붙은 cDNA 조각들의 library로 전환됩니다. 그 후 염기서열들은 한쪽 끝에서부터 해독되거나(single-end sequencing) 또는 양쪽 끝에서 해독됩니다 (paired-end sequencing). 

**Single-end sequencing**: DNA 또는 RNA를 한쪽 끝에서만 시퀀싱하는 방법 (시퀀싱 기계가 염기쌍을 한쪽 끝에서부터 읽음). 이 방법은 경제적으로 빠르게 많은 양의 질적 데이터를 제공해 준다. Small RNA-seq과 Chromatin immunoprecipitation sequencing (ChIP-Seq)과 같은 방법에 좋은 선택이다. 

<br>

**Paired-end sequencing**: DNA 또는 RNA를 양쪽 끝에서 시퀀싱하는 방법 (시퀀싱 기계가 염기쌍을 양쪽 끝에서부터 안쪽으로 읽음). 이 방법은 align이 가능한 질적 데이터를 제공해 준다. 유전체 재배열(rearrangement), 반복 서열(repetitive sequence elements), 유전자 융합(gene fusion), 새로운 전사물(novel transcript) 발견에 용이하다. 같은 시간과 노력으로 두 배의 read 수를 생성할 수 있을 뿐만 아니라, read pairs로 정렬된 서열들로 더 정확한 alignment를 할 수 있고, single-end 데이터로는 불가능한 insertion-deletion (indel) variant도 찾을 수 있다. 

<br>

**Single-end vs. paired-end sequencing**

Single-end sequencing에서는 시퀀싱 기계가 서열의 조각을 오직 한 쪽 끝에서부터 읽어서 염기쌍 서열을 생성하는 반면, paired-end sequencing에서는 우선 한쪽 끝에서 특정 길이까지 읽고, 그다음 다른 쪽 끝에서 서열을 읽는다. Paired-end sequecning은 유전체 상에서 다양한 read들의 상대적인 위치를 찾는데 더 용이하다. 그러므로 single-end sequencing보다 gene insertion, deletion, inversion과 같은 구조적 재배열을 찾는데 더 효과적이다. 또한 repetitive 한 지역의 assembly를 향상할 수 있다. 그러나 Paired-end는 single-end보다 더 비싸고 많은 시간이 소요된다. 또한 모든 실험이 높은 수준의 정확도를 필요하지 않을 수도 있다.

<br>

### 전사체 발현향 계측; Normalized gene expression

>RPKM, FPKM, TPM은 생물정보학에서 상당히 쉽게 접할 수 있는 용어들이다. 여러 샘플의 RNA-seq을 발현 분석할 때 정규화(normalize)된 발현량을 의미하는 것으로 과거에는 RPKM이 많이 쓰였으나 점차 FPKM을 거쳐 TPM을 많이 사용하고 있다. 각 용어가 의미하는 바는 크게 다르지 않으며 샘플간의 차이 (주로 total depth)로 인한 발현량을 정규화하여 비교하기 위해 사용하게 되었다.

#### RPKM

**R**eads **P**er **K**ilobase per **M**illions mapped reads

과거에 single-end RNA-seq이 주로 생산되었을 때 단순히 read counts in transcripts, gene length, total mapped reads 만을 가지고 계산한 값이다.

![enter image description here](https://img.over-blog-kiwi.com/2/52/09/53/20170916/ob_589d7a_eqn8245.png)



\- 먼저 total mapped reads로 나누는 이유는 sample에 따라서 total depth가 다른 것을 normalize해주기 위함이다. 

sample A는 1,000,000개의 reads가 있고 sample B에는 2,000,000개의 reads가 있을 때, 같은 유전자에 같은 갯수의 reads가 붙었다고하면 sample A가 더 발현량이 높다고 계산하기 위함이다. 

(sample B가 PCR cycle이 한 번 더 진행되었기 때문에 전체 depth가 높은 것이라고 가정한 것이다. 단순히 reads count가 같다고 해서 발현량이 같은 것이 아니다!)

\- gene length로 나누는 이유는 샘플간의 비교가 아닌 유전자 간의 비교를 위함이다. 

gene A는 길이가 1000고 gene B는 길이가 2000일 때 위와 마찬가지로 같은 갯수의 reads가 붙었다면 gene A의 발현량이 더 높다고 추측할 수 있다. 

(길이가 2배이고 발현량이 같다면 gene B가 두 배 많은 reads를 생산 되었어야 한다!)

<br>

#### FPKM

**F**ragment **P**er **K**ilobase of transcript per **M**illion mapped reads

Paired-end RNA-Seq을 진행하면 2개의 read가 하나의 fragment에서 나오는데 transcript 단위에서의 (하나의 gene에서도 여러 [isoform](http://www.incodom.kr/isoform)의 [transcript](http://www.incodom.kr/transcript)가 존재 가능) expression unit으로 정의할 수 있다. 그러나 이 2개의 read가 반드시 mappable한 것은 아니다.

FPKM과 RPKM의 차이는 paired-end로 생산된 RNA-seq에서 나타난다. paired-end는 하나의 reads에서 두 개의 fragments가 나온다고 생각하면 된다. 즉 left fragements와 right fragments가 각각 계산되어 RPKM의 대략 두 배의 값을 가지게 된다.(paired-end의 두 fragments가 항상 같이 맵핑되는 것은 아니기 때문에 정확하게 두 배 일 수는 없다.) 

용어가 헷갈릴 수 있는데 일반적으로 paired-end는 reads를 두 개 가지고 있다고 말하는 경우도 있다. 하지만 여기서는 각각을 fragments로 계산하고 있음을 유의해야 한다.

![img](https://williamjeong2.github.io/bioinformatics/7-gene-expression-unit-explained/img/FPKM.png)

<br>

#### TPM

**T**ranscripts **P**er **M**illion

TPM에서는 total mapped reads를 사용하지 않으며 transcripts level에서의 값이 계산된다. gene level로 계산하고 싶으면 TPM값들을 모두 더하면 된다.

<계산하는 방법>

1. 각각의 transcipt에 대해 mapped reads / transcripts length / killobase 로 값을 구한다. (=normalized transcripts expression)

2. 1번에서 구한 값들을 모두 더한뒤 1,000,000으로 나눈다. (=scaling factor)

3. 각각 1번에서 구한 값들을 2번에서 구한 값으로 나눈다. (=TPM)

![Image](https://drive.google.com/a/insilicogen.com/uc?id=0B-nv4ogSsXisSTczRkhfVy1TbTA&export=download)

sample들 안에서 맵핑되는 reads의 비율을 보다 명확하게 말해준다.



![Comparison of normalization methods for real data. (A) Boxplots of log2(counts + 1) for all conditions and replicates in the M. musculus data, by normalization method. (B) Boxplots of intra-group variance for one of the conditions (labeled ‘B’ in the corresponding data found in Supplementary Data) in the M. musculus data, by normalization method. (C) Analysis of housekeeping genes for the H. sapiens data. (D) Consensus dendrogram of differential analysis results, using the DESeq Bioconductor package, for all normalization methods across the four datasets under consideration.](https://oup.silverchair-cdn.com/oup/backfile/Content_public/Journal/bib/14/6/10.1093/bib/bbs046/2/bbs046f1p.png?Expires=1597989775&Signature=BkMT093POpyOJERL46ZKz5o6aQGxSZLJNhD~l7mSLRztIic43H58lC-G2yBsJK-GRio1Ar0QSeAGaqjr7FBhjd3l1giH50PTvrIr6ieir~hJ-cDzKP5xLcHuSVpWI7~Y57saqLfWb-9KxQv3mvy8JDqfbI2n2hDX3jP~DlN9Zqkxu1V6FZTiWcDG-dxGp3a-USaiyYybj78xGbF9LBbN8PMjzlu~mbxzQLj7FvEM4u9apGyiSmLq46YpYGwT8DZv1qYsGhAOI-MiRabh23Yue0vsT84a6l~wNamfm925~cjhFzWkO7XlA4nYC-BJMO7~R54Vokg0YP2seeaydzcG6Q__&Key-Pair-Id=APKAIE5G5CRDK6RD3PGA)





<br>

### Splicing

> RNA-Seq은 이미 알려진 스플라이싱 결합부위에 매핑되는 단편서열을 정략적으로 셀 수 있을 뿐만 아니라, 미지의 새로운 스플라이싱 결합부위 탐색도 가능하다. 엑손결합부의(Exone junction)에 매핑된 단편서열의 검출로 스플라이싱 위치를 찾아 낼 수 있고, 드노버 서열조립을 통해 엑손 건너뛰기 같은 선택적 스플라이싱도 찾아 낼 수 있다.

\- RNA이어맞추기(스플라이싱; splicing)는 전사*를 통해 만들어진 미성숙 전령 DNA(pre-mRNA)에서 불필요한 부분인 인트론(intron)이 잘려나가고, 필요한 부분인 엑손(exon)끼리 서로 연결되는 과정을 의미한다. 

   \* 전사: DNA에 있는 정보를 RNA로 전환하는 과정. DNA를 틀로 삼아 이와 상보적인 RNA를  생성한다.

<br>

 한 개의 유전자에서 생성되는 mRNA는 여러 종류가 될 수 있으며 이 것은 차등적 스플라이싱(alternative splicing)이라는 기전에 의해 다른 조합의 엑손으로 이루어진 mRNA가 만들어 지는 것이다. 이것을 isoform이라 부르기도 하며 각 isoform 별 유전자 발현 차이를 NGS로 분석 하는 것이 가능하다. 또한 DNA 변화에 의해 스플라이싱에 문제를 일으켜 비정상적 스플라이싱(aberrant splicing)이 일어나면 온전한 mRNA가 만들어지 못하여 단백질 합성에 문제를 일으킬 수 있다. RNA 시퀀 싱으로 이러한 비정상적 스플라이싱 여부를 쉽게 확인할 수 있다.

스플라이싱 사이트에 발생한 변이는 RNA 스플라이싱을 방해하여 엑손의 손실이나 단백질 코딩 서열의 변경을 초래할 수 있다.

<br>

\- 선택적 RNA 이어맞추기(alternative RNA splicing)는 엑손들이 연결될 때 순서를 건너뛰면서 결합되어 다양한 조합의 단백질을 만들어내는 과정이다. 이를 통해 하나의 유전자에서 기능이 서로 다르거나 심지어는 상반되는 여러 개의 단백질이 만들어진다. 유전병, 노인성 질환 혹은 암이 발생하게 되면 이 이어맞추기 과정이 비정상적으로 변하는 것으로 알려졌으나, 어떠한 요인에 의한 것인지는 분명하지 않았다.





<br>



## 출처

[BRIC Bio통신원] 유전자발현의 새로운 조절 기전 규명...광주과학기술원 심해홍 교수팀 ( https://www.ibric.org/myboard/read.php?Board=news&id=262833 )

**[출처]** [2009.RNA-seq: 전사체학을 위한 혁신적인 도구](https://blog.naver.com/naturelove87/221423276676)|**작성자** [랩몬LM](https://blog.naver.com/naturelove87)

**[출처]**[Single-end, Paired-end & Mate-pair]( https://m.blog.naver.com/naturelove87/221477813889)|**작성자** [랩몬LM](https://m.blog.naver.com/naturelove87/221477813889)

[RNA-seq 분석방법]([http://www.incodom.kr/RNA-seq_%EB%B6%84%EC%84%9D_%EB%B0%A9%EB%B2%95#h_e5cbc7ae58ca36cf4ef5417555cfc27c](http://www.incodom.kr/RNA-seq_분석_방법#h_e5cbc7ae58ca36cf4ef5417555cfc27c)) | 출처 Incodom

[Next Generation Sequencing / Whole Genome Sequencing]( https://www.biocompare.com/Molecular-Biology/9187-Next-Generation-Sequencing/)) | 출처 Biocompare

<br>

\- RPKM/FPKM/TPM

https://bgreat.tistory.com/97

http://www.incodom.kr/Expression_profiling

https://williamjeong2.github.io/bioinformatics/7-gene-expression-unit-explained/



\- Splicing

출처: 'NGS(Next Generation Sequencing) 기반 유전자 검사의 이해 (심화용)' [식품의약품안전처 식품의약품안전평가원]



https://blog.naver.com/dic1224/221034364039