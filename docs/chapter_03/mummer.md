# MUMmer 比较基因组

!!! summary "本节简介"
    **MUMmer** 是一个快速的细菌核酸和蛋白序列比较工具，很多其他工具将其做为第三方比对工具应用。本节介绍 **MUMmer** 工具的安装和使用。

**基因组数据准备**

```
# 下载2个伤寒沙门菌的基因组序列
$ esearch -db nuccore -query "NC_003198" | efetch -format fasta > CT18.fasta
$ esearch -db nuccore -query "NC_004631" | efetch -format fasta > Ty2.fasta
```

## 1. 安装 MUMmer3

**1.1 预编译包**

如果是 ubuntu 系统，软件仓库里有 MUMmer3.23 版本的软件，可以直接用 apt 工具安装。

```
$ sudo apt install mummer
```

**1.2 编译安装**

下载源代码编译安装

```
$ wget https://nchc.dl.sourceforge.net/project/mummer/mummer/3.23/MUMmer3.23.tar.gz
$ tar zxvf MUMmer3.23.tar.gz
$ cd MUMmer3.23
$ make && sudo make install
```

## 2. 使用 MUMmer3

MUMmer 工具包含核心工具和脚本工具：

**常见问题**

如果你运行 mummerplot 脚本是发生下面的错误提示，其原因是高版本 perl 的[define](http://perldoc.perl.org/functions/defined.html)改动：

!!! failure "错误提示"
    Can't use 'defined(%hash)' (Maybe you should just omit the defined()?) at /usr/local/sbin/mummerplot line 884。

解决办法是修改 mummerplot

```
# 确认 perl 版本
$ perl -v
# 如果 perl 版本高于 5.6.1 需要修改 mummerplot 代码
$ perl -i -pe 's/defined \(%/\(%/' mummerplot
```

如果遇到 mummerplot 运行时遇到如下错误：

!!! failure "错误提示"
    WARNING: Unable to query clipboard with xclip
    set mouse clipboardformat "[%.0f, %.0f]"。

因为mummer3是调用gnuplot4来绘图，而目前新的版本一般是 gnuplot5，2者有兼容性差异。在mummerplot中找到 `$P_FORMAT .= "\nset mouse clipboardformat \"$MFORMAT\"";` 将其注释后运行 mummerplot

```
# 使用 run-mummer3 运行比对
$ run-mummer3 CT18.fasta Ty2.fasta result
```

!!! note "生成结果文件"
    - result.align
    - result.errorsgaps
    - result.gaps
    - result.out

```
~$ mummer -mum -b -c CT18.fasta Ty26.fasta > output
~$ mummerplot -x [0:5000000] -y [0:5000000] --png output
```

## 3. 安装 MUMmer4

MUMmer3.23 发布时间较早，很多依赖库更新会导致程序兼容性。目前 mummer 在 github 上有官方仓库，最新的版本已到4.0beta，这里我们用新版本进行安装。

```
$ wget https://github.com/gmarcais/mummer/releases/download/v4.0.0beta/mummer-4.0.0beta.tar.gz
$ tar zxvf mummer-4.0.0beta.tar.gz
$ cd mummer-4.0beta
$ ./configure
$ make && make install
```
