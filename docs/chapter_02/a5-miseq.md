# A5-miseq

[A5-miseq](https://sourceforge.net/projects/ngopt/) 是一个用 perl 脚本开发的针对细菌基因组 de novo assembly 的 pipeline 工具。它通过使用一系列第三方工具来实现优化miseq测序的拼接工作流。它使用的工具集包括：

* bwa
* samtools
* SGA
* bowtie
* Trimmomatic
* IDBA-UD
* SSPACE

这些工具都以及集成在 A5-miseq 中，不需要另外安装。为了避免不同版本的 samtools，bowtie 对结果产生的差异，建议采用虚拟环境如 docker 等来隔离运行环境。

## 1. 安装

### 1.1 下载预编译包安装

```
$ wget http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20150522.tar.gz
$ tar zxvf a5_miseq_linux_20150522.tar.gz -C ~/app
$ sudo ln -s `pwd`/a5_miseq_linux_20150522/bin/a5_pipeline.pl /usr/local/sbin
```

### 1.2 Docker镜像方式

```
~/docker$ mkdir -p a5-miseq && cd a5-miseq
~/docker/a5-miseq$ touch Dockerfile
```

编辑 Dockerfile 文件内容，将其修改如下：

```
FROM ubuntu:latest
MAINTAINER Mark Renton <indexofire@gmail.com>

RUN apt-get update -qy
RUN apt-get install -qy openjdk-7-jre-headless file

ADD http://downloads.sourceforge.net/project/ngopt/a5_miseq_linux_20150522.tar.gz /tmp/a5_miseq.tar.gz

RUN mkdir /tmp/a5_miseq
RUN tar xzf /tmp/a5_miseq.tar.gz --directory /tmp/a5_miseq --strip-components=1

ADD run /usr/local/bin/
ADD Procfile /
ENTRYPOINT ["/usr/local/bin/run"]
```

## 2. 使用 a5-miseq 进行基因组组装

```
$ perl a5_pipeline.pl SRR955386_1.fastq SRR955386_2.fastq output
```

## 3. 使用帮助

```
a5_pipeline.pl [--begin=1-5] [--end=1-5] [--preprocessed] <lib_file> <out_base>
```
