# DESeq2

`cts` : count matrix called

`coldata` : table of sample information called / `batch` and `condition` should be columns of coldata

`design` : how to modle the samples

<br>

coefficients: 단항식 다항식에 대하여 쓰인다. 단항식에서는 지목된 변수 이외의 부분(나머지 인수 전체)이 그 변수의 계수가 된다. 예를 들어 단항식 3ax2에서 3은 ax2의 계수, 3a는 x2의 계수이다.

dispersion: 확산, 분산

comprehensive: 종합적인, 포괄적인

take into account: ~을 고려하다



<br>

## Ensembl

```
ensembl <- useMart("ensembl")
```

The `useMart()` function can now be used to connect to a specified BioMart database, this must be a valid name given by `listMarts()`.

```
ensembl = useMart("ensembl",dataset="hsapiens_gene_ensembl")
```





## 결과 파일

R 기반인  차등 발현 유전자(Differentially Expressed Genes, DEG) 분석 프로그램 DESeq2는 CSV 파일을 결과로 줍니다. CSV 파일은 테이블로서 다음과 같은 열을 가지고 있습니다.



\* **GeneName** - 유전자 이름 (gene ID) 또는 transcipt ID

\* **baseMean** - 모든 샘플에서 가져온 정규화된 카운트 값(normalizaed count values)의 평균을 크기 인자(size factor)로 나눈 값

\* **log2FoldChange** - 효과 크기에 대한 추정치 (The effect size estimate). 이 값은 대조군과 비교군 사이의 유전자 또는 Transcript의 발현이 얼마나 변했는지를 나타냄. 이 값은 log2 스케일로 계산됨.

\* **lfcSE** - Log2 fold change 추정치에 대한 표준 오차 추정치(standard error estimtate)

\* **stat** - 유전자 또는 transcript에 대한 테스트 통계 값

\* **pvalue** - 유전자 또는 transcript의 테스트에 대한 P-value

\* **padj** - 유전자 또는 transcript의 다중 테스트에 대한 Adjusted P-value



## 출처

[결과 파일] [[R\] DESeq2  결과 파일](http://blog.naver.com/naturelove87/222070135374)|**작성자** [랩몬LM](http://blog.naver.com/naturelove87)

[ensemble] https://www.bioconductor.org/packages/devel/bioc/vignettes/biomaRt/inst/doc/biomaRt.html



