# SNP-Pipeline

## 安装

### 1. Ubuntu 通过 Conda 安装

```bash
# 安装系统依赖库
$ sudo apt install g++ gcc make gsl-bin libgsl0-dev prepython3.5-dev tabix
# 创建一个新的虚拟环境
$ conda create -n snp-pipeline python=3.5
$ source activate snp-pipeline

# 安装依赖软件
(snp-pipeline)$ conda install bowtie2 smalt samtools varscan \
> sra-toolkit bcftools picard numpy
# 添加 VarScan.jar 路径
(snp-pipeline)$ export CLASSPATH=~/software/picard/picard.jar:$CLASSPATH
(snp-pipeline)$ export CLASSPATH=VarScan.v2.4.2.jar:$CLASSPATH
# bioconda 里的版本不是最新版的，因此我们用 pip 从 pypi 安装最新版的 snp-pipeline
(snp-pipeline)$ pip install snp-pipeline
```

### 2. docker 安装

Dockerfile

```dockerfile
FROM ubuntu:latest
MAINTAINER Mark Renton <indexofire@gmail.com>

WORKDIR /tmp/
RUN apt-get update && apt-get install -y -q \
    bowtie2 \
	default-jre \
	g++ \
	gcc \
	gsl-bin \
	libgsl0-dev \
	make \
	python \
	python-dev \
	python-pip \
	smalt \
	samtools \
	wget \
	sra-toolkit \
	bcftools \
	tabix \
	bc
RUN apt-get clean


# install others
RUN wget http://downloads.sourceforge.net/project/varscan/VarScan.v2.3.9.jar -q \
    && cp VarScan.v2.3.9.jar /usr/bin/VarScan.jar \
    && wget http://www.niehs.nih.gov/research/resources/assets/docs/artbinmountrainier20160605linux64tgz.tgz -q \
	&& tar -zxf /tmp/artbinmountrainier20160605linux64tgz.tgz \
	&& cd /tmp/art_src_ChocolateCherryCake_Linux \
	&& ./configure \
	&& make \
	&& make install \
	&& cd /tmp/ \
	&& rm -r /tmp/*

# install snp-pipeline and snp-mutator via mirror
RUN pip install -i https://mirrors.aliyun.com/pypi numpy snp-pipeline biopython snp-mutator

# set varscan path and cpu threads
ENV CLASSPATH /usr/bin/VarScan.jar
ENV NUMCORES grep -c ^processor /proc/cpuinfo 2>/dev/null || sysctl -n hw.ncpu

# test lambda data
WORKDIR /test/
RUN copy_snppipeline_data.py lambdaVirusInputs testLambdaVirus \
	&& cd testLambdaVirus \
	&& run_snp_pipeline.sh -s samples reference/lambda_virus.fasta \
	&& copy_snppipeline_data.py lambdaVirusExpectedResults expectedResults \
	&& diff -q snplist.txt expectedResults/snplist.txt \
	&& diff -q snpma.fasta expectedResults/snpma.fasta \
	&& diff -q referenceSNP.fasta expectedResults/referenceSNP.fasta

ENTRYPOINT ["run_snp_pipeline.sh"]
CMD ["-h"]
```

创建 docker image，并查看镜像

```bash
~$ sudo docker build -t snp-pipeline:latest .
~$ sudo docker images
```



## 测试

```bash
# 用 lambda 数据进行测试
~$ source activate snp
(snp)~$ copy_snppipeline_data.py lambdaVirusInputs testLambdaVirus
(snp)~$ cd testLambdaVirus
(snp)~/testLambdaVirus$ run_snp_pipeline.sh -s samples reference/lambda_virus.fasta
(snp)~/testLambdaVirus$ copy_snppipeline_data.py lambdaVirusExpectedResults expectedResults
(snp)~/testLambdaVirus$ diff -q snplist.txt expectedResults/snplist.txt
(snp)~/testLambdaVirus$ diff -q snpma.fasta expectedResults/snpma.fasta
(snp)~/testLambdaVirus$ diff -q referenceSNP.fasta expectedResults/referenceSNP.fasta
# 如果 diff 没有结果输出表示运行结果与实际结果一致，程序测试没有问题
```

## 运行 snp-pipeline

```bash
# 以交互方式运行docker容器，并挂在系统/data到容器里的/data路径
~$ sudo docker run -v /data:/data -ti snp-pipeline:latest /bin/bash

# 进入docker
root@
```


## reference
1. http://snp-pipeline.readthedocs.io
