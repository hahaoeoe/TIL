# Short title: Generative adverarial networks for bulk RNA-seq

> Full title: A practical application of generative adversarial networks for RNA-seq analysis to predict the molecular progress of Alzheimer's disease
>
> Jinhee park, Hyerine Kim 외 2명 

<br>



## 요약





<br>

## GAN; ­Generative Adversarial Network

**Generative**

­첫 단어인 ‘Generative’는 GAN이 생성(Generation) 모델이라는 것을 뜻한다. 생성 모델이란 ‘그럴듯한 가짜’를 만들어내는 모델이다.

**Adversarial**

­‘Adversarial’은 GAN이 두 개의 모델을 적대적(Adversarial)으로 경쟁시키며 발전시킨다는 것을 뜻한다. 위조지폐범과 경찰을 생각해보자. 이 둘은 적대적인 경쟁 관계다. 위조지폐범은 경찰을 속이기 위해 점점 지폐 위조 제조 기술을 발전시키고, 경찰은 위조지폐범을 잡기 위해 점점 위폐를 찾는 기술을 발전시킨다. 시간이 흐르면 위조지폐범의 위폐 제조 기술은 완벽에 가깝게 발전할 것이다.

이처럼 GAN은 위조지폐범에 해당하는 생성자(Generator)와 경찰에 해당하는 구분자(Discriminator)를 경쟁적으로 학습시킨다. 생성자의 목적은 그럴듯한 가짜 데이터를 만들어서 구분자를 속이는 것이며, 구분자의 목적은 생성자가 만든 가짜 데이터와 진짜 데이터를 구분하는 것이다. 이 둘을 함께 학습시키면서 진짜와 구분할 수 없는 가짜를 만들어내는 생성자를 얻을 수 있다. 이것이 GAN의 핵심적인 아이디어인 적대적 학습(Adversarial Training)이다.

**Network**

­ ‘네트워크(Network)’는 이 모델이 인공신경망(Artificial Neural Network) 또는 딥러닝(Deep Learning)으로 만들어졌기 때문에 붙었다. 이 글에서는 ‘인공신경망’과 ‘딥러닝’을 구분 없이 사용하겠다. 사실 적대적 학습이라는 개념을 구현하기 위해 반드시 딥러닝을 써야 하는 것은 아니다. 하지만 알파고 등 여러 사례에서 볼 수 있듯이 딥러닝은 강력한 머신러닝 모델을 가능하게 만드는 기술이다. 

<br>

=> 학습과정을 반복하면 분류 모델과 생성 모델이 서로를 적대적인 경쟁자로 인식하여 모두 발전하게 됩니다. 결과적으로, 생성 모델은 진짜 데이터와 완벽히 유사한 가짜 데이터를 만들 수 있게 되고 이에 따라 분류 모델은 진짜 데이터와 가짜 데이터를 구분할 수 없게 됩니다. 즉, GAN은 생성 모델은 분류에 성공할 확률을 낮추려 하고, 분류 모델은 분류에 성공할 확률을 높이려 하면서 서로가 서로를 경쟁적으로 발전시키는 구조를 이루고 있습니다.

<img src="https://files.slack.com/files-pri/T25783BPY-F9SHTP6F9/picture2.png?pub_secret=6821873e68" alt="그림2: 생성자 – 구분자 도식" style="zoom: 50%;" />

<br>

##  Latent space; 잠재 공간

> If I have to describe latent space in one sentence, it simply means a representation of compressed data.

­생성자는 랜덤 벡터 ‘z’를 입력으로 받아 가짜 이미지를 출력하는 함수다. 여기서 ‘z’는 단순하게 균등 분포(Uniform Distribution)나 정규 분포(Normal Distribution)에서 무작위로 추출된 값이다. 생성자는 이렇게 단순한 분포를 사람 얼굴 이미지와 같은 복잡한 분포로 매핑(Mapping)하는 함수라고 볼 수 있다. 생성자 모델에 충분한 수의 매개 변수가 있다면 어떤 복잡한 분포도 근사할 수 있다는 것이 알려져 있다.

­‘z’ 벡터가 존재하는 공간을 잠재 공간(Latent Space)이라고도 부른다. 여기서는 잠재 공간의 크기를 임의로 100차원으로 뒀다. 잠재 공간의 크기에는 제한이 없으나 나타내려고 하는 대상의 정보를 충분히 담을 수 있을 만큼은 커야 한다. GAN은 우리가 이해할 수는 없는 방식이지만 ‘z’ 벡터의 값을 이미지의 속성에 매핑시키기 때문이다. 뒤에 살펴볼 GAN의 파생 모델에서 잠재 공간의 의미를 더욱 자세히 이해할 수 있을 것이다.

<img src="https://files.slack.com/files-pri/T25783BPY-F9RFJ3VDJ/picture4.png?pub_secret=da0323f283" alt="그림4: 생성자는 단순한 분포를 복잡한 분포로 매핑하는 함수다." style="zoom: 50%;" />

> ­생성자는 단순한 분포를 복잡한 분포로 매핑하는 함수다.

<br>

## Interpolation; 보간법

> Interpolation은 GAN의 개념이 Ian Goodfelllow를 통해 2014년 처음 세상에 나온 후 ([Goodfellow et al. 2014](https://arxiv.org/abs/1406.2661)), 딥러닝을 접목시킨 DCGAN 논문에서 처음으로 진행되었던 실험([Radford et al. 2015](https://arxiv.org/abs/1511.06434)).
>
> 이 논문에서는 이 실험에 대해 `Walking in the latent space.` 즉 `잠재공간을 걷다.` 라고 표현

모델이 학습한 잠재 공간은 매우 높은 차원이기 때문에 사람은 그 영역을 직관적으로 이해할 수 없다. 하지만 **Interpolation**을 통해서 우리는 간접적으로나마 **유의미한 잠재공간이 존재한다는 것을 확인**할 수 있고, 나아가 그곳을 **탐색**한다는 의미로써 "잠재공간을 걷다." 라는 은유적인 표현을 쓴 것이다.

**Interpolation**은 사전에 따르면 '보간법'으로, 위키백과에서는 "수치해석학의 수학분야의 용어로써, 알려진 데이터 지점의 고립점 내에서 새로운 데이터 지점을 구성하는 방식"이라고 정의한다.

<br>

=> z는 랜덤 노이즈, 잠재 변수 z의 공간을 탐색하는 방식을 잠재 공간 보간(latent space interpolation)이라고 부른다.

The generator model in the GAN architecture takes a point from the latent space as input and generates a new image.

The latent space itself has no meaning. Typically it is a 100-dimensional hypersphere with each variable drawn from a [Gaussian distribution](https://machinelearningmastery.com/statistical-data-distributions/) with a mean of zero and a standard deviation of one. Through training, the generator learns to map points into the latent space with specific output images and this mapping will be different each time the model is trained.

The latent space has structure when interpreted by the generator model, and this structure can be queried and navigated for a given model.

Typically, new images are generated using random points in the latent space. Taken a step further, points in the latent space can be constructed (e.g. all 0s, all 0.5s, or all 1s) and used as input or a query to generate a specific image.

A series of points can be created on a linear path between two points in the latent space, such as two generated images. These points can be used to generate a series of images that show a transition between the two generated images.

Finally, the points in the latent space can be kept and used in simple vector arithmetic to create new points in the latent space that, in turn, can be used to generate images. This is an interesting idea, as it allows for the intuitive and targeted generation of images.

<br>

## Amyloid-β (Aβ) accumulation

> 유전적 AD의 원인으로는 세 개의 유전자가 지목된다. 그 중 하나는 1991년에 알려진 amyloid precursor protein(APP) 을 암호화하는 유전자이다. 그리고 그보다 중요한 원인으 로 지목되는 유전자가 presenilin-1(PSEN1)과 presenilin2(PSEN2)를 암호화하는 유전자로, PSEN1과 PSEN2는 각각 γ-secretase 복합체를 구성하며, 이에 관하여 152가지의 병 원성 돌연변이가 발견되었다. 세 유전자에서의 돌연변이에 의 해 뇌에서는 beta-amyloid(Aβ)가 비정상적으로 과잉 생성 되고 또 축적되는데, 이러한 현상이 궁극적으로 AD를 초래 하게 된다. Aβ가 축적되면 senile plaques를 형성하는데, 이 plaque가 매우 강한 신경독성을 띠기 때문에 뇌의 뉴런에 손 상을 입히는 것이다. AD의 원인으로서 지목되는 또 다른 유 력한 물질로는 Tau protein이 있다. 과인산화 된 Tau는 서로 엉겨 붙어서 신경세포체 내에서 neurofibrillary tangle을 형 성하고, 이 tangle이 microtubule을 해체시킴으로써 뉴런의 기능을 파괴하는 것이다.

 Alzheimer’s Disease 증세를 유발하는 독성 뇌 단백질 아밀로이드베타(Amyloid-beta)는 뇌에 축적됨으로써 신경세포가 손상을 입고 뇌 세포조직이 과도한 자극을 받게 된다는 것이다. 뇌 세포조직에 만성적인 염증이 발생함으로써 점진적으로 신경세포가 손상을 입게 되고 Alzheimer’s Disease 의 증상이 유도되는 것으로 알려져 있다. 

<img src="https://sarahabdellahbrownuniversityalzheimers.weebly.com/uploads/5/2/5/3/52531031/normal-vs-alzheimers-brain-1_orig.jpg" alt="Main Characteristics of Alzheimer's - Neuroscience In Action ..." style="zoom: 50%;" />

<br>

**치매 관련 베타 아밀로이드와 타우 단백질 _타우 단백질 축적은 베타 아밀로이드 축적과 독립적으로 진행**

알츠하이머병의 원인으로 지목되고 있는 단백질로 타우(tau)와 베타 아밀로이드를 비롯하여 여러 가지 단백질이 있다. 이들 중에서 타우와 베타 아밀로이드는 모두 뇌신경세포에 있는 단백질로 타우는 세포 내부에, 베타 아밀로이드는 세포의 표면에 나타난다. 베타 아밀로이드와 타우 단백질이 잘못 접하면 베타 아밀로이드는 서로 뭉쳐 플리그(extracellular amyloid plaques)를 형성하고 타우는 서로 엉키면서(intraneuronal neurofibrillary tangles) 신경세포를 파괴, 치매를 일으키는 것으로 알려져 있다.

알츠하이머병 환자의 부검 및 신경영상 연구(nrutoimaging studies)뿐만 아니라 유전학적 데이터를 통해 알수 있는 사실은 Aβ 플라크 침착이 선행하고 다음 피질 타우 단백의 축적(accumulation of cortical tau pathology)과 함께 신경퇴행성 변화(tau-mediated neurodegeneration)가 나타난다고 판단했다. 이런 점을 고려하여 알츠하이머병 치료제(AD therapeutics)의 개발은 주로 뇌에서 Aβ를 제거하는 데 집중되어 왔다. 

그러나 최근에는 AD 마우스 모델 및 환자 유래 인간유도 만능줄기세포(human induced pluripotent stem cell, iPCS) 모델로 밝혀진 전임상실험 결과들은 타우 단백질로 인해 초래된 조직병리가 Aβ 축적과 독립적으로 진행될 수 있고,  AD 및 비정상 대사경로에 대한 유전적 위험인자로 발생함을 나타내고 있다. 



<br>

## Transition curve; 완화 곡선







<br>

##  Omics; 오믹스

오믹스(omics)는 전체를 뜻하는 말인 옴(-ome)과 학문을 뜻하는 접미사 익스(-ics)가 결합된 말로, 어떤 특정 학문 분야를 말하기보다는 개별 [유전자](https://terms.naver.com/entry.nhn?docId=5141493&ref=y)(gene), 전사물(transcript), [단백질](https://terms.naver.com/entry.nhn?docId=5141336&ref=y)(protein), 대사물(metabolite) 연구에 대비되는 총체적인 개념의 데이터 세트를 바탕으로 하는 생물학 분야라고 할 수 있다. ‘~옴’은 우리말로 전체를 지칭하는 ‘~체’로 번역된다. 오믹스(omics)는 [인간유전체 사업](https://terms.naver.com/entry.nhn?docId=5141544&ref=y)(human genome project) 이후 새롭게 등장한 학문 분야로 몇 가지 측면에서 전통적 생물학 연구와 대비된다. 오믹스에서 다루는 데이터는 대규모-대용량(high-throughput) 기술들로 생산되기 때문에 개별 물질을 연구 대상으로 하는 전통적인 생물학과 달리 데이터의 전산학적 처리가 필수적이다. 또 수학적, 통계적 기법이 연구에 적극 활용된다. 이러한 요구로 생겨난 새로운 학문 분야가 바로 [생물정보학](https://terms.naver.com/entry.nhn?docId=5141510&ref=y)(bioinformatics)이다.

<br>



## Data normalization

1. Database의 경우, Data Normalization 은 사용자가 데이터베이스를 적절하게 활용할 수 있도록 데이터베이스 내의 데이터를 재구성하는 프로세스 유형 중 하나로써, 주로 중복성을 제거하거나, 데이터를 논리적으로 그룹화 하는 것을 의미
2.  Machine Learning에서의 Data Normalization은 데이터에 있는 숫자 열의 값을 왜곡하지 않으면서 공통 척도(scale)로 변환하는 것을 의미. 반드시 필요한 것이 아닌, 데이터의 feature들이 서로 다른 범위를 가지고 있는 값일때 사용한다. Data Normalization을 했을 경우 데이터가 같은 scale에서 존재하기 때문에 학습 진행시에 앞뒤로 진동하지 않고 경사 하강이 더 빠르게 수렴될 수 있도록 한다. 만약 Data Normalization을 하지않은 경우(필요한 경우인데) 경사 하강은 앞뒤로 진도하며, local minimum에 빠질 위험 또한 존재한다. 때문에 Data Normalization은 모델의 전체 성능을 더 좋게 해준다.

<br>



## DEGs analysis; Differentially Expressed Genes analysis

> 유전자 발현 값을 측정하고 통계적으로 처리하여 대조군과 비교군 간에 발현이 유의한 유전자 후보군을 선발하는 분석(동일한 유전자의 평균 발현량이 서로 다른 조건에서 유의하게 다른지를 분석하는 방법론)

 마이크로어레이 기술은 한 번의 실험으로 수천에서 수만 개의 유전자 발현을 측정할 수 있다. 마이크로어레이 자료를 이용한 분석 중에서 가장 기본적인 것이라 할 수 있는 것이 차별발현유전자의 분석이다. 이 분석은 동일한 유전자의 평균 발현량이 서로 다른 조건에서 유의하게 다른지를 분석하는 방법론이다. 

가장 많이 사용하는 방법론 중의 하나는 SAM(significant analysis of microarray) 방법으로, 기존의 T-test를 변형시킨 분석방법으로 variance shrinkage 방법과 bootstrapping 기법을 적용하여 마이크로 어레이 자료의 특성에 맞는 분석을 시행한다. 서로 다른 두 군에 대한 계산뿐만 아니라 여러 개의 군집으로 나누어 있는 경우에도 적용가능하고 생존 정보가 있는 자료의 분석도 가능하다. 최근에는 마이크로어레이 실험의 대안으로 많이 시행하고 있는 RNA-seq의 분석도 적용할 수 있도록 개발되었다. 

SAM과 함께 많이 사용되고 있는 방법은 Cyber-T 방법이다. 이 방법 역시 T test를 기반으로 하지만 베이지안(Bayesian) 통계 기법을 사용하여 서로 다른 군의 분산을 추정하는 특징을 가지고 있다. 그리고 이웃하는 유전자의 분산을 고려하여 유전자 발현량의 분산을 추정하고 있다. 특히, 이 방법은 샘플 수가 적은 자료에서 탁월한 성능을 발휘하는 것으로 알려졌다. 

<br>

=> 대조검체에 비해 실험 검체에서 발현량이 유의하게 증가 혹은 감소한 유전자의 목록을 얻고자 하는 것이다. Microarray 실험을 통해 얻어진 유전자 발현 값들의 평균치를 비교하여 차이가 나는 유전자를 골라내야 하는데, 이때 사용되는 통계 기법으로는 두그룹간 비교에서는 T분포를 이용한 T-test, 세그룹이상의 비교에서는 F분포를 이용한 ANOVA가 가장 많이 사용된다. 하지만, 실제 유전자 발현량의 분포는 T분포나 F분포를 정확히 따라가지 않는 경우가 많아서 SAM이라는 방법이 널리 사용된다. SAM은 microarray분석에 특화된 T-test라고 이해하면 된다.

<br>



## TPM; Transcript per million reads

> RNA expression normalization 방법 중 하나
>
> 일반적으로 RNA-seq data는 DEG분석을 하기에 앞서 정규화(normalization) 된다. 아래와 같은 이유로 사용한다.
>
> * 샘플간의 비교
> * 유전자(gene) 비교
> * 통계 모델을 RNAseq 데이터에 대해 사용하기 위해



RNA - Seq 을 통해 mapping되어 있는read의 수를 가지고 각 샘플의 유전자별 혹은 transcript별로 발현 정도를 확인 할 수 있다. 하지만 mapping된 read의 개수로 발현량을 정의하기에는 샘플별로 시퀀싱 데이터 크기가 다를 수도 있고, 유전자나 transcript의 길이에 따라 mapping된 read의 수도 다르기 때문에 객관적인 값이라고 보기 힘든면이 있다. 그렇기 때문에 차등발현 유전자의 발현값 계산은 이러한 오차를 줄여 조금 더 객관적인 값을 보여줄 수 있도록 정규화(normalization)를 하도록 만들어졌다.FPKM, RPKM, TPM 는 샘플간의 차이 (주로 total depth)로 인한 발현량을 정규화하여 비교하기 위해 사용하게 되었다. (참고 , RPKM을 많이 사용했으나 점차 FPKM을 거쳐 TPM을 많이 사용하고 있다.)

FPKM, RPKM, TPM 값을 계산하는 방법에 대해 알아봅시다.

**(1) read count** : 하나의 유전자의 위치에 assemble 된 read들의 숫자를  센 값. read count Normalization , 서로 다른 샘플들을 하나로 묶어서 normalization 진행 하므로 , 다른 샘플 간에 어떤 유전자가 다르게 발현 되었는지 확인 할때 사용합니다.

RPKM, FPKM, TPM  의 경우, 예를 들어서 60 milion reads가 나온 샘플 A와 30 milion reads가 나온 샘플 B의 같은 유전자 C가 어느 샘플에서 더 많이 발현되었는지를 비교가능합니다. 각 샘플에 대해 normailzation을 진행합니다. 즉 , 하나의 데이터 안에서 유전자의 길이, 전체 library의 양에 따라 normalization을 합니다. 그러므로 하나의 샘플에서 어떠한 유전자가 더 or 덜 발현 되었는지 확인할때 사용합니다.

**(2) RPKM** : Reads per kilobase of transcript per milion mapped reads 과거에 single end RNA seq 이 주고 생산되었을때, 단순히 read counts in transcripts, gene length , total mapped reads 만을 가지고 계산한 값이다. 

과거에 single end RNA seq 이 주고 생산되었을때, 단순히 read counts in transcripts, gene length , total mapped reads 만을 가지고 계산한 값이다. 



**(3) FPKM** : Fragments per kilobase of exon per milion FPKM과 RPKM의 차이는 paired-end로 생산된 RNA-seq에서 나타난다. paired-end는 하나의 reads에서 두 개의 fragments가 나온다고 생각하면 된다. 즉 left fragements와 right fragments가 각각 계산되어 RPKM의 대략 두 배의 값을 가지게 된다.(paired-end의 두 fragments가 항상 같이 맵핑되는 것은 아니기 때문에 정확하게 두 배 일 수는 없다.) 



**(4) TPM** :  Transcripts per milionTPM은 transcript(혹은 gene)단위를 기준으로 한다. 

R/FPKM과 비슷하지만 RNA population에서 transcript length의 분포까지 설명한다. 이 방법 없이는, 다른 transcript length distribution을 가지는 두 RNA pools를 비교할 때 bias가 생길 수 있다.



<br>

## DESeq2

> 차등 발현을 계산할 수 있는 툴 중 하나
>
> DESeq2는 DEG분석의 대표적인 방법 중의 하나로, 차세대 염기서열분석(Next Generation Sequencing)으로부터 얻는 read count data를 분석하는 R 패키지이다.

<br>

**유전자 발현 차이 분석(Differential gene expression analysis)**

Differential expression analysis(그림 1b)는 샘플 간 비교된 유전자 발현 값을 필요로 한다. RPKM, FPKM, TPM은 샘플 간 비교를 위해 sequencing depth가 보정된 결과이다. 이들은 normalization 방법에 의존적이고 과발현되거나 발현에 영향을 주는 feature로 인해 transcript의 이질성이 높은 샘플에서는 효과적이지 않다. TMM, DESeq, PoissonSeq, UpperQuartile 등은 변이가 크거나 발현이 매우 큰 feature는 무시한다. 샘플 간 비교에 영향을 주는 또 다른 요소는 샘플이나 조건에 따른 transcript 길이의 차이, 위치에 따른 coverage biases, fragment size 평균값, GC content 차이(DEAseq package를 통해 보정)가 있다. NOISeq R package은 bias를 확인할 수 있는 다양한 종류의 diagnostic plot을 제공하고 각 경우에 따른 다양한 normalization이 가능하다. 샘플 특이적 normalization 이후에도, 여전히 batch effect가 존재하는데 이는 적절한 실험 설계나 COMBAT, ARSyN과 같은 batch 보정 방법을 통해 최소화할 수 있다. 위의 방법은 microarray data를 위해 개발되었지만 RNA-seq 보정에도 유용하다(STATegra project, unpublisbed).
RNA-seq qunatification은 read count에 기반하기에, 발현 차이 분석을 위해 Poisson 또는 negative binomial 확률 분포를 계산한다. Negative binomial distribution (gamma-Poisson distribution으로도 알려짐)은 Possion distribution의 일반화로, random sampling 변수 외의 overdispersion과 같은 변수를 고려한다. 그러나, 별개의 distribution을 이용하는 것은 작은 read count로 인한 sampling 변수가 크지 않은 경우 반드시 필요한 것은 아니다. 데이터의 variance 구조를 통해 normalizing 하는 방법은 별개의 분포를 사용하는 것만큼 효과적이다. 게다가, 광범위한 normalization은 (TMM, batch removal과 같은) 데이터의 고유한 성질을 잃게 하고 continuous distribution을 닮게 한다.
edgeR은 input으로 raw read count와 가능한 bias의 원인을 통해 통계 모델을 만들어 nor-malization과 발현 차이 분석을 연계하고 있다. 다른 방법들은, 가능한 biases를 모두 제거한 nor-malized 데이터를 얻은 이후 발현 차이 분석을 하기도 한다.

 **`DESeq2`** *는 edgeR과 같이 negative bi-nomial을 기준 분포로 삼고 고유한 normalization 방법을 제공한다.* 

baySeq와 EBSeq은 역시 nega-tive binomial model에 기반하고 있는 Bayesian 방식을 사용하는데 실험 그룹 간의 차이를 표현하고 각 유전자의 확률을 계산한다. 다른 방식으로는 data transformation 방법이 있는데 small read count로 인한 sampling variance를 고려하여 독립적인 유전자 발현 분포를 생성하고 이를 regular linear model로 분석한다. NOISeq 또는 SAMseq과 같은 non-parametric 방식은 최소한의 가정을 통해 inferential analysis를 위한 null distribution을 실제 data에서 측정한다. 반복 실험이 없거나 적은 두 샘플간의 비교와 같은 작은 규모의 연구에서는 negative binomial distribution을 이용한 측정은 noise가 많을 수 있다. 더 간단한 Poisson distribution을 이용하는 DEGseq 또는 empirical distribution을 사용하는 NOISeq을 대안으로 사용할 수 있다. 그러나 이 두 방법은 biological replication이 없는 경우 population inference가 불가능 하여 p-value 계산이 불가능하다. 반복 실험이 없는 RNA-seq은 exploratory value만 가질 수 있다. Sequencing 비용이 점차 감소하는 것을 고려하여, RNA-seq 실험 시 최소한 3번의 biological replication을 추천한다.

![img](http://www.ibric.org/upload/geditor/201706/0.25927700_1498615114.jpg)

> 그림 1. RNA-seq의 포괄적인 수리 분석(computational analysis) 로드맵

<br>

**시각화 (Visualization)**

유전자 발현 차이 분석을 위한 software package들은 (DESeq2, DEXseq) 내장된 시각화 기능들이 있다.

<br>

## Cholesterol synthesis





<br>





## synapse plasticity 시냅스 가소성

신경계에서 정보는 시냅스를 거쳐 한 신경세포에서 다른 신경세포로 전달된다. 개체의 경험을 통해서 시냅스의 연결강도는 변화될 수 있으며 때로는 새로운 시냅스가 형성되기도 하고 기존의 시냅스가 소멸되기도 한다. 이러한 시냅스의 변화를 시냅스 가소성이라 하며 학습과 기억을 담당하는 중요한 기전으로 생각된다. 시냅스의 전달 강도는 강화되기도 하고 약화되기도 한다. 장기강화(Synaptic long-term potentiation) 또는 장기저하(Synaptic long-term depression)로 일어날 수 있으며, 이를 시냅스의 양방향 가소성(Bidirectional plasticity)이라 한다. 

**장기 강화(long-term potentiation, LTP)** 

해마 신경세포들의 시냅스 연결 강도는 고빈도의 시냅스 자극에 의해서 강화될 수 있다. 그림에서처럼 뇌 절편 상에서 해마의 관통경로(perforant pathway)에 위치시킨 자극 전극을 이용하여 과립세포(granule cell)로 들어가는 시냅스를 자극하면 과립세포의 흥분성 시냅스후 전위(excitatory postsynaptic potential)가 유도된다. 연속적이며 높은 빈도의 시냅스 자극이 주어진 후에 흥분성 시냅스후 전위의 크기는 2배 이상 커지며 이러한 시냅스의 강화현상이 수 시간 동안 유지될 수 있음을 관찰할 수 있었다. 이 실험의 결과는 고빈도의 자극에 의해서 시냅스의 전달 효율이 증가하여 장기간 유지될 수 있음을 뜻하며, 이러한 현상을 장기 강화 현상이라 한다. 해마뿐만 아니라 뇌의 여러 지역에서 장기 강화 현상이 관찰된다.

> 신호를 받는 신경세포의 말단부분(synapse)에 변화가 생겨 자극에 의한 반응이 과해지는 현상



**장기 억제(long-term depression)** 

시냅스 연결 강도는 강화되는 방향으로만 변화하는 것이 아니라 억제될 수도 있다. 시냅스전 신경세포에 대해 낮은 빈도의 자극이 주어지면 시냅스의 효율이 감소하기도 한다. 장기 억제는 장기 강화와 마찬가지로 뇌의 여러 지역의 시냅스에서 관찰된다.

<br>



## RLD data ; regularized logarithmic data





<br>



## Data augmentation

> Overfitting 에 대한 문제를 해결하기 위한 대표적인 방법 중 하나가 데이터의 양을 늘리는 것이다. 그러나 학습 데이터를 늘리는 것이 쉽지 않으며, 학습 데 이터가 늘어나면 학습 시간이 길어지기 때문에 효율성을 반드시 고려해 야 한다. 따라서 data argumentation 이라는 기법을 사용해서 overfitting의 문제를 극복하였다.
>
> Augmentation은 원래 데이터를 부풀려서 성능을 더 좋게 만든다는 의미

제한적인 데이터를 사용하는 머신러닝에서는 훈련 데이터에 약간의 조작을 가해 비슷하지만 다른 이미지를 만들어 훈련하곤 한다. 특히 데이터가 엄청나게 중요해진 딥러닝에서는 이런 데이터 변조(augmentation)이 꼭 필요하게 되었다. 특히 실전에 가까운 응용일 수록 데이터 변조를 어떻게 했느냐가 또 하나의 중요한 포인트가 되곤 한다. 변조를 거친 데이터와 그렇지 않은 데이터의 성능을 비교하는 것은 문제가 된다.

데이터 변조와 구분되어야 할 것이 데이터 전처리(preprocessing)이다. 전처리는 데이터 전체에 공통적으로 적용되는 알고리즘이다. 모든 데이터가 공통적으로 거치는 동일한 과정이 전처리이다. 한편, 각 데이터마다 다르게 / 같은 데이터더라도 매번 다르게 적용하는 것이 데이터 변조이다.

훈련 때 데이터 변조를 사용했다면, 테스트 데이터에도 데이터 변조를 사용할 수 있다. 예를 들어 훈련 때 좌우반전을 사용했었더라면 테스트 데이터들에 대해서도 좌우반전시킨 데이터를 테스트할 수 있다. 이 경우, 성능을 높이기 위해 동일한 테스트 데이터를 여러 번 다른 방식으로 변조시킨 후 이 결과를 종합해 해당 데이터를 맞췄는지를 볼 수 있다.

<br>

심층 신경망(deep neural networks)은 은닉층(hidden layer)을 많이 쌓아 매개변수(parameter)를 늘리는 방식으로 모델의 표현력을 높였습니다. 많게는 수백만 개에 이르는 매개변수를 제대로 훈련하기 위해서는 정말 많은 데이터가 필요한데요, 여기에는 데이터가 현실을 충분히 잘 반영할 정도로 다양하며 그 품질이 우수해야 한다는 단서가 붙습니다.

![img](http://t1.kakaocdn.net/braincloud/homepage/article_image/aae39e38-4145-43b8-abc5-1ec9ea529f1c.png)

> [ [그림 1 ](https://medium.com/datadriveninvestor/how-did-you-scrap-this-data-d10706d0dcd3)] 데이터의 양과 알고리즘 성능 간의 관계. 딥러닝에서는 데이터가 많을수록 그 성능이 비례해 증가하는 경향이 있다. © [Arjun Kava](https://medium.com/@arjunkava91)

<br>

매개변수를 훈련할 충분한 학습 데이터를 확보하지 않으면 모델의 성능을 저해하는 [과적합(overfitting)](https://thebook.io/006958/part04/ch13/02/) 문제가 상대적으로 더 쉽게 발생합니다([그림 2]). 과적합은 모델이 훈련 데이터에만 지나치게 적응해 테스트 데이터 또는 새로운 데이터에는 제대로 반응하지 못하는 현상을 가리킵니다. 좀 더 쉽게 설명하자면, 고양이의 정면 사진만 배운 네트워크는 고양이의 옆면 사진을 입력받으면 이를 고양이로 인식하지 못하는 거라고 보면 되겠습니다. 어떤 사진으로도 고양이를 잘 인식할 수 있도록 하기 위해선 다양하고 많은 데이터로 네트워크를 훈련하는 게 무엇보다 중요한 이유입니다.

하지만 양질의 데이터를 대량 확보하는 데 비용이 큰 걸림돌이 될 때가 많습니다. 데이터 종류에 따라서는 확보할 수 있는 분량에도 제약이 따를 수도 있습니다. 대표적으로, 의료 영상 데이터는 의사가 의료영상에서 어느 부분이 장기이고 종양인지를 일일이 기록한 정답 데이터(ground truth data) 획득 비용이 많이 들어 다량의 학습 데이터셋을 구축하기가 쉽지 않습니다.

이처럼 딥러닝 모델을 충분히 훈련하는 데 필요한 데이터를 확보하는 기법 중 하나로 어그먼테이션(Augmentation)이 소개되고 있습니다. 어그먼테이션은 [그림 3]에서처럼 적은 양의 훈련 데이터에 인위적인 변화를 가해 새로운 훈련 데이터를 대량 확보하는 방법론을 의미합니다. 예를 들어, 이미지를 상하좌우로 뒤집거나(flipping) 자르는(cropping) 방식으로 새로운 이미지 데이터를 확보하는 거죠. 현실 세계에서도 실제로 존재할 법한 데이터를 생성함으로써 좀 더 일반화된 모델을 얻는 걸 목표로 합니다.

![img](http://t1.kakaocdn.net/braincloud/homepage/article_image/dd1d76c0-4dfa-4fae-a99d-d3a4b80e766f.png)

> [ [그림 2](https://hackernoon.com/memorizing-is-not-learning-6-tricks-to-prevent-overfitting-in-machine-learning-820b091dc42) ] 데이터가 많아질수록 과적합 문제가 발생할 가능성이 낮아진다. © [Julien Despois](https://hackernoon.com/@juliendespois)

<img src="https://www.aitimes.kr/news/photo/201804/11629_11423_146.png" alt="적대적 생성 신경망(GAN)을 이용한 프레임 워크, 무니트(MUNIT) 공개 ..." style="zoom:50%;" />

> [ [그림 3 ](https://raw.githubusercontent.com/aleju/imgaug-doc/master/readme_images/examples_grid.jpg?raw=true)] 다양한 방식으로 변형해 새로 획득한 이미지를 학습 데이터로 활용한다. 





<br>

## 단어

* dissect 해부하다, 나누다.
* pathological 병적인, 병리학의
* recapitulate 1. 요점을 되풀이하다, 요약하다. 2. <개체 발생이><진화의 모든 단계를> 반복하다
* examine 조사[검토]하다, 심사[검사]하다.
* transition curve 완화 곡선
* alteration 변화, 변경
* progressive 진보적인 / 점진적인, 꾸준히 진행되는
* mediate 1. (중개하여) …을 달성하다, 성립시키다, 가져다 주다. 2. 〔분쟁·논쟁 따위〕를 조정하다, 해결하다. 3. (…을/…사이를) 조정하다, 중개하는 수고를 하다.
* convergence 집중, 집합점, 수렴, 집합
* extensively 널리, 광범위하게
* manipulate 조작하다, 처리하다, 조종하다.
* transition 이행, 변화, 과도기
* epidermal cell 상피세포
* differentiation 분화
* what if ~면 어쩌지?
* contextual (글의) 전후 관계상의, 문맥상의
* hinder 저해[방해]하다, ~을 못하게 하다.
* transient 일시적인, 순간적인
* circumvent 피하다.
* tricky 까다로운, 힘든, 복잡한, 곤란한
* labor-intensive 노동집약; 생산의 한 요소인 노동에 대한 투자(投資)가 자본에 대한 투자보다 큰 생산방법
* preparation 준비
* inevitablely 불가피한, 필연적인 / 반드시 있는, 언제든지 예상할 수 있는
* intrinsic 고유한, 본질적인
* augmentation 증가, 증대
* reliability 신뢰도, 확실성
* variation 변화
* causality 인과관계; 일반적으로 어떤 사실과 다른 사실 사이의 원인과 결과관계
* coexpression 공발현
* dissect 해부하다.
* dissect (특히 인체·동식물을) 해부하다. / (문제·이론 등을) 분석하다, 세밀히 조사하다.
* myelination 수초화
* embryonic 초기
* fibroblast 섬유아세포 ; 섬유성 결합조직의 중요한 성분을 이루는 세포로 조직절편으로 관찰하면 편평하고 길쭉한 외형을 가지며 흔히 불규칙한 돌기를 보인다. 
* transcriptome 전사체(RNA)
* synapse plasticity 시냅스 가소성
* reciprocal 상호간의
* modulation 조정
* conversion 전환, 변환
* progression 진행
* methodological 방법론적인
* augmentation 논증
* recapitulate 재현하다.

<br>

## 출처

**GAN**

https://dreamgonfly.github.io/2018/03/17/gan-explained.html

> GAN & Latent space 

https://www.samsungsds.com/global/ko/support/insights/Generative-adversarial-network-AI-2.html

<br>

**Latent space**

https://towardsdatascience.com/understanding-latent-space-in-machine-learning-de5a7c687d8d?gi=4e7ae0bda2cf

http://terryum.io/korean/2016/05/05/FeatureSelection_KOR/

https://machinelearningmastery.com/how-to-interpolate-and-perform-vector-arithmetic-with-faces-using-a-generative-adversarial-network/

<br>

**Interpolation**

https://jeinalog.tistory.com/16

<br>

**Omics**

[네이버 지식백과] [오믹스](https://terms.naver.com/entry.nhn?docId=5141548) [omics] (생화학백과)

<br>

**Gene expression and regulation**

https://blog.naver.com/vaneco/221965086089

<br>

**Data normalization**

[출처] [Data Normalization은 무엇이고 왜 필요한가요?](https://blog.naver.com/sjg02122/221887719309)|**작성자** [수제햄버거](https://blog.naver.com/sjg02122)

<br>

**DEGs**

http://blog.daum.net/bhumsuk/4858532

마이크로어레이 자료의 분석 / 질병관리본부 국립보건연구원 유전체센터 바이오과학정보과 조성범

<br>

**RNA expression normalization**

[FPKM, RPKM, TPM 계산 방법](https://blog.naver.com/aldud123kr/221563323099)|**작성자** [ominiv](https://blog.naver.com/aldud123kr)

https://blog.naver.com/irisfullip/221690721506

<br>

**DESeq2**

[BRIC View, RNA-seq 자료 분석법 모범 사안 조사, 김장근], https://www.ibric.org/myboard/read.php?Board=report&id=2776

<br>

**amyloid beta**

알츠하이머 병(Alzheimer’s Disease)의 연구동향 / 조지훈 (전남대학교 의과대학 의과학과 의생명교실) 외 3명

https://blog.naver.com/PostView.nhn?blogId=hyouncho2&logNo=221766836443&parentCategoryNo=&categoryNo=51&viewDate=&isShowPopularPosts=true&from=search

<br>

**synapse plasticity**

[네이버 지식백과] [시냅스가소성](https://terms.naver.com/entry.nhn?docId=5669889) [synaptic plasticity] (동물학백과)

<br>

**Data augmentation**

http://blog.naver.com/PostView.nhn?blogId=4u_olion&logNo=221437862590&parentCategoryNo=&categoryNo=45&viewDate=&isShowPopularPosts=true&from=search

[CNN 관련] https://deepestdocs.readthedocs.io/en/latest/003_image_processing/0030/ 

https://www.kakaobrain.com/blog/64

<br>