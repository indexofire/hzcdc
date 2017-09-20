# Roary

## Roary 介绍

## 使用方法

### 1.

首先将所有要分析的gff格式的基因组文件放置到gff文件夹中，运行 roary 获得 Pangenome 分析，主要可以获得基因的分布情况。加上 `-e --mafft` 参数，可以调用 mafft 获得核心基因序列的多重比对结果。`-p 40` 为分析时条用的 threads 数量，根据计算机CPU具体情况设定。

``` bash
$ roary -e --mafft -p 40 gff/*.gff
```

### 2. 结果文件

在当前文件夹下可以生成相应的结果文件：

accessory_binary_genes.fa
accessory_binary_genes.fa.newick
accessory_graph.dot
accessory.header.embl
accessory.tab
blast_identity_frequency.Rtab
clustered_proteins
core_accessory_graph.dot
core_accessory.header.embl
core_accessory.tab
core_alignment_header.embl
core_gene_alignment.aln
core_gene_alignment.aln.reduced
gene_presence_absence.csv
gene_presence_absence.Rtab
number_of_conserved_genes.Rtab
number_of_genes_in_pan_genome.Rtab
number_of_new_genes.Rtab
number_of_unique_genes.Rtab
pan_genome_reference.fa
RAxML_bootstrap.raxml
RAxML_info.raxml
summary_statistics.txt

!!! note "summary_statistics.txt"
    test
