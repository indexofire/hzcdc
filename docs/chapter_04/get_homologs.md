# get_homologues

[get_homologues][] 是一个用 perl 编写的用来鉴定细菌 core-genomes 和 pan-genomes 的开源生物信息学工具。

[get_homologues][] 依赖多个 perl 库和第三方软件，且 bioconda 中并没有打包可以直接下载安装。幸好源代码包里包含了 install.pl 安装脚本，直接运行即可。

## 算法应用

[get_homologues][] 包含了3套不同的算法应用来获得结果：

* 

## 分析案例

我们以 Bacillus cereus 基因组 assembly 数据库为例分析该物种的 Pangenomics。

```
# 抓取 gbk 格式的 assembly 文件在 NCBI FTP 上的下载路径
$ esearch -db assembly -query "Bacillus cereus[ORGN] AND latest[SB]" | \
> efetch -format docsum | \
> xtract -pattern DocumentSummary -element FtpPath_RefSeq | \
> awk -F"/" '{print $0"/"$NF"_genomic.gbff.gz"}' > bcereus.path

# 用 wget 工具下载
$ wget --limit-rate 300k --no-passive-ftp -i bcereus.path

# get_homologues 分析
$ get_homologues.pl -d . -n 40
```

get_homologues 可以默认使用 blast 进行序列相似性搜索，为了加快速度，也可以使用 diamond。

```
# -X 调用 diamond 进行序列相似性搜索
$ get_homologues.pl -d gbk_folder -n 40 -X
```




[get_homologues]: (http://eead-csic-compbio.github.io/get_homologues)

## Reference
1. Bacterial Pangenomics, Methods and Protocols, Chapter14
2. [get_homologues manual](http://eead-csic-compbio.github.io/get_homologues/manual/manual.html)
3.
