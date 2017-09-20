# SPAdes

## 1. 下载并安装 SPAdes

``` bash
$ wget http://spades.bioinf.spbau.ru/release3.10.1/SPAdes-3.10.1-Linux.tar.gz
$ tar zxvf SPAdes-3.10.1-Linux.tar.gz -C ~/app
$ sudo ln -s ~/app/SPAdes-3.10.1-Linux/bin/* /usr/local/sbin
```

## 2. 拼装基因组

```
$ spades.py --careful -k 21,33,55,77,99,127 -t 40 -1 SRR95386_1.fastq.gz -2 SRR955386_2.fastq.gz -o spades_output
```

SPAdes会尝试不同的Kmer，因此拼装时间也会根据Kmer选择数量成倍增加
