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



## 출처

[HTSeq] http://www.incodom.kr/HTSeq#h_83acc28eabd73c7c5ff5c98b103e98ce

