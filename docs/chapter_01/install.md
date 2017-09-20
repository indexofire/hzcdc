# 安装软件

!!! note "关于本节"
    NGS 数据分析需要安装软件，在 Linux 下如何去管理好你的各种生物信息学软件呢？

安装，管理，发布等操作是用户连年累月需要面对的问题。Linux 让人非常愉快的一点在于软件包的管理。主流发行版的软件查询、安装、更新和卸载都非常方便，比如软件安装，大部分 Linux 发行版都有自身的包管理工具，例如 Ubuntu 的 apt-get，Arch 的 pac-man。这些工具一般都是系统级别的管理，软件版本考虑到稳定性等往往升级略慢（除了 Arch 这种），对于那些`软件版本控`，就会考虑手动下载源代码编译安装。但这样一方面下载、更新等操作比较麻烦，还要根据自己的发行版设置各种路径，可能要安装许多依赖库而开销更多的储存成本。另外经常会遇到各种软件对不同版本的第三方软件的需求，如果都放在系统路径里特别容易混乱。

因此出现了各种各样的解放 Linux 安装、管理与发布的工具。这里我们就基于 NGS 相关软件，Python 数据分析相关软件的角度，介绍一些优秀的管理工具。

## 1. Python 生态圈

Python 包管理工具 pip 可以方便的实现安装 python 软件包。

### 1.1 virtualenv & virtualenvwrapper

```bash
# 安装 virtualenv 和 virtualenvwrapper
$ sudo apt install python-virtualenv python-virtualenvwrapper
# 设置环境
$ mkdir -p .envs
$ echo 'export WORKON_HOME=$HOME/.envs' >> ~/.bashrc
$ echo 'source /usr/local/bin/virtualenvwrapper.sh' >> ~/.bashrc
$ source ~/.bashrc
# 新建一个名叫 myenv 的虚拟环境
$ mkvirtualenv myenv
# 进入该虚拟环境
$ workon myenv
# 在myenv环境中安装 numpy，与系统路径中的python包隔离
(myenv)$ pip install numpy
# 在myenv环境中更新 numpy
(myenv)$ pip install -U numpy
```

virtualenv 主要是用来构建虚拟的python运行环境，pip安装工具也只能安装python包，对于非python开发的应用软件来说就无能为力了。这时候就可以亮出另一个牛逼闪闪的武器`conda`

### 1.2 conda

相对于 virtualenv，conda 更为复杂一些。首先 conda 是一个跨平台的包管理工具，它与 apt-get 类似，但安装软件包到系统的 conda 虚拟路径中而非系统路径中，conda 有 channels 类似 apt-get 的。它也有类似 virtualenv 虚拟环境，但后者一般只能处理 python 包，而 conda 可以安装各种语言的预编译包。

安装 conda 有多种方式。官方一般推荐第1，2种。你可以根据自己的操作系统平台以及python版本选择下载各个版本的 [Miniconda](https://conda.io/miniconda.html) 或 [Anaconda](https://www.continuum.io/downloads)。2者的区别主要在于前者提供了最小的conda运行环境，后者包含了更丰富的大量应用。安装了miniconda，也可以手动安装软件包实现anaconda的功能。

```bash
# 方式一：安装 miniconda for python3
$ wget https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh
$ bash Miniconda3-latest-Linux-x86_64.sh

# 方式二：安装 anaconda for python3
$ wget https://repo.continuum.io/archive/Anaconda3-4.4.0-Linux-x86_64.sh
$ bash Anaconda3-4.4.0-Linux-x86_64.sh

# 方式三：用系统工具 pip 安装 conda 到系统路径中
$ sudo pip install conda

# 方式四：virtualenv 中安装 conda
$ sudo apt install python-virtualenv python-virtualenvwrapper
$ mkdir -p .envs
$ echo 'export WORKON_HOME=$HOME/.envs' >> ~/.bashrc
$ echo 'source /usr/local/bin/virtualenvwrapper.sh' >> ~/.bashrc
$ mkvirtualenv conda && workon conda
(conda)$ pip install conda
```

前2种安装方式区别在于 anaconda 预装了一些常用的数据分析工具。而miniconda则是一个单纯的conda环境。因为 conda 是 python 开发的，所以可以用pip工具安装。方式三是用pip工具安装到系统级路径中。方式四是安装到虚拟环境中。

```bash
# conda 安装软件包 numpy，安装在conda主环境中
$ conda install numpy

# conda 建立在虚拟环境 py3 中安装软件包 numpy
$ conda create -n py3 numpy
# conda 在虚拟环境 py3 中安装软件 numpy
$ source activate py3
(py3)$ conda install numpy
```

#### 查看虚拟环境

```bash
# 显示所有虚拟环境以及root环境，路径前有 '*' 的为当前激活状态的虚拟环境
$ conda info --env
```

#### 查看已安装的软件包

```bash
# 显示 root 环境中安装的软件包
$ conda list

# 显示虚拟环境 py3 中的安装软件包
$ conda list -n py3
# 在conda虚拟环境中直接运行 conda lis 也可以查看该虚拟环境安装包
$ source activate py3
(py3)$ conda list

# 将当前环境安装包版本输出并在新环境中安装，由于 conda 是跨平台的，这样你在 mac 上测试好的依赖库和软件，可以直接同步 txt 到生产环境的 Linux 里，一步完成部署环境。
(py3)$ conda list -e > ~/py3.txt
(py3)$ conda create -n new_env --file ~/py3.txt
```

#### 查找可安装的软件包

```bash
# 搜索软件包 numpy
$ conda search -f numpy

# 搜索软件包名称中包含 numpy 的软件包
$ conda search numpy

# 搜索版本为 1.10 的 numpy 软件包
$ conda search --spec numpy=1.11.1

# 查看 numpy 软件包具体信息
$ conda info numpy

# 查看 conda 可安装的非 python 安装包
$ conda search --canonical | grep -v 'py[0-9]\{2\}' | awk -F'-' '{print $1}' | uniq
```

#### 安装软件包

```bash
# root 环境中安装 numpy
$ conda install numpy

# py3 虚拟环境中安装 numpy
$ source activate py3
(py3)$ conda install numpy

# conda 通过默认排序的 channels 来检索软件包，如果想指定用某个 channels 的软件包可以如下操作
$ conda install -c conda-forge
```

#### 添加镜像

conda 联网下载安装软件包往往速度比较慢，常会出现 HTTP 访问失败的情况。通过添加国内镜像来解决这个问题，国内目前最常用的镜像是清华大学维护的。

```bash
# 添加清华的 channels
$ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
$ conda config --set show_channel_urls yes
# 添加清华的 conda-forge channel
$ conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge/
```

#### 通过代理下载

但是清华大学的 mirror 只有 conda-forge 的包，很多其他安装包如 bioconda 的软件安装包速度就特别不理想了。虽然 pip 支持镜像服务器，但是对于非python软件包或者喜欢conda安装方式的该怎么办呢？比较简单的方式就是通过代理线路下载了。方法有：

1. 通过http/https代理
2. 通过socks5代理

对于有http/https代理的用户，可以在.condarc配置文件中写入代理设置。

```
proxy-servers:
  http: http://user:password@proxy-ip:port
  https: https://user:password@proxy-ip:port
```

如果你是科学上网，比如拥有ss建立的socks5的代理线路，可以通过proxychain-ng来实现快速终端代理。

```
$ git clone https://github.com/rofl0r/proxychains-ng.git
$ cd proxychains-ng
$ ./configure
$ make
$ sudo make install
# 命令前加上 proxychains4 就可以让命令通过 proxy 的线路来访问网络了。
$ proxychains4 conda install bwa
```

#### 编译软件包

```bash
$ conda install conda-build
```

### 1.3 conda-forge

### 1.4 bioconda

bioconda 是一个分发生物信息软件的频道，利用 conda，你基本上不用自行编译常见的生物信息学软件了。生活是不是变得更美好了呢？

```bash
# 打印添加的 channels 并按照优先级排序
$ conda config --get channels

# 添加 bioconda 常见所需 channels
$ conda config --add channels conda-forge
$ conda config --add channels defaults
$ conda config --add channels r
$ conda config --add channels bioconda
```

[conda-forge](https://conda-forge.github.io/) 是什么？

首先conda是由商业公司 Anaconda 领导发行的软件工具，你可以想象成 Docker Inc 控制的 Docker，Cannonial 控制的 Ubuntu 等。虽然软件都是基于开源协议，但是均有商业公司控制，有自己的商业盈利模式。因此就产生了由开源社区推动的 conda-forge。


## 2. Docker

* 提供一个虚拟的操作平台，供我们安装依赖不同版本系统的工具软件。
* 提供一个即时可用的应用软件或者流程的镜像，开发者把软件部署到系统镜像中， 使用者可以直接下载下来使用，省去了个人安装软件的烦恼。
* 提供一个系统资源分配手段，给不同用户的程序分配独立的计算资源。

### 2.1 Docker 基本概念

镜像 - Images: 可以认为是超级轻量级的虚拟机的快照。 镜像会有自己的唯一ID，名字和标签，比如ubuntu:latest, django:1.6等。 通常都是在已有的镜像（多数是Linux操作系统的镜像）的基础上构建自己的 具有新功能的镜像。

容器 - Containers: 可以认为是超级轻量级的虚拟机， 是镜像运行起来所处的可读写的状态。 容器里面可以安装、运行程序，还可以把安装好的程序存储起来获得新的镜像。与虚拟机很大的不同在于，一个容器通常只运行一个程序。在Docker中， 应用程序和数据文件是分开的，因此可以在不影响数据的情况下快速升级代码 或系统。

数据卷 - Volumes : 永久保存数据的磁盘空间。 Docker允许用户定义哪一部分是应用程序，哪一步分是数据，并且把他们分隔开。 这就保证了在Docker中容器的生命周期是短暂的，而数据的存储是永恒的。数据卷存储在运行Docker的宿主机上，对每个容器来说是特有的。 我们可以启动同一个镜像来产生多个容器，并且分别给他们分配一个数据卷。数据卷也可用于在不同的容器间共享数据。 具体参见http://blog.genesino.com/2016/09/docker-lamp/

联通 - Links: 容器启动后会分配有一个私有IP，其它容器可以通过这个IP地 址与这个容器通讯。假如有个正在运行的数据库容器 (dbapp)， 那么我们可以在网络服务器容器 (webserver)中通过指定端口连接dbapp与数据库容器通讯。

### 2.2 安装

```bash
# 在 Ubuntu 中安装。
$ sudo apt update
$ sudo apt install linux-image-extra-$(uname -r) linux-image-extra-virtual
$ sudo apt install
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
$ sudo apt update
# 在测试机器上可以安装最新版
$ sudo apt install docker-ce
# 在生产环境里可以指定安装版本
$ sudo apt install docker-ce=17.06.0
```

### 2.3 Docker 权限

### 2.4 使用 Docker

```bash
$ docker info
```

运行 Docker 需要有镜像和容器。镜像是容器的只读版本， 最基础的镜像是一个操作系统，是运行其他命令的基础。 因此我们需要先获取一个操作系统镜像，通常使用Ubuntu系统, CentOS系统和 Alpine (只有5M)。 我们也可以根据所要运行软件的需要，来获取不同的操作系统， 方便软件的安装。


```bash
# 搜索某个镜像，比如搜索名称中含有 ubuntu 的镜像
$ docker search image-names
```

```bash

```

## 3. 云平台

### 3.1 AWS EC2 教程

**Amazon Elastic Compute Cloud (Amazon EC2)** 是一种基于 AWS Web 的一项云服务，特点是可在云中提供大小可调的计算量。它的服务接口非常简单，您可以轻松获取和配置容量，可以完全控制您的计算资源，甚至可以直接挂载他人开发的生物信息学工具集合的镜像，减少部署时间。

**Amazon EC2** 启动新服务器实例的速度非常快，当您的计算要求发生变化时，您便可以快速扩展计算容量。服务按您实际使用的容量和计算量收费，还为开发人员提供了创建故障恢复应用程序以及排除常见故障情况的工具。总之，非常适合作为计算攻击来分析二代测序获得的大数据量分析。

**AWS** 的新客户可免费试用 **Amazon EC2**。注册后，新 AWS 客户在一年内每个月都将获得以下 EC2 服务：

1. 750 小时运行 Linux、RHEL 或 SLES t2.micro 实例的 EC2 使用时间
2. 750 小时运行 Microsoft Windows Server t2.micro 实例的 EC2 使用时间
3. 750 小时 Elastic Load Balancing 加上 15 GB 数据处理
4. 30 GB 的 Amazon Elastic Block Storage（以任意方式对通用型 (SSD) 或磁性介质型进行组合），附加 200 万次 I/O（采用磁性介质）和 5. 1 GB 快照存储
6. 适用于所有 AWS 服务的共计 15 GB 的带宽传出
7. 1 GB 区域数据传输

简而言之，新注册用户可以获得1年的有限免费使用。如果要偶尔使用的生产Instance，建议选择按按需实例。这样需要计算的时候打开，计算分析完成后关闭实例即可，可以最大程度上节约资源和开销。对于频繁使用的用户，可以申请`专用预留实例`，通过支付预付金，可以获得一定的折扣来减低成本。

#### 3.1.1 注册申请

首先要去 https://aws.amazon.com/cn/ 注册一个帐号，帐号申请过程中会要求填写一张信用卡，并会电话联系确认卡的有效性。

用新注册帐号登录后，可以看到 **Amazon Web Services** 页面，上面列出了 Amazon 所有的服务内容。点击`Compute & Networking`里的`EC2`，可以进入你的`EC2 Dashboard`。

要注意的一点是，顶部工具栏在用户工具右侧有一个地区选项下拉菜单，可以选择不同区域的服务器。个人使用感受，亚洲的服务器比如东京和新加坡的连接速度最快，但是美国的服务器速度也很不错，比一般的VPS商售卖的要快的多！至于选择哪一个地区，可以根据自己的网络实际情况来选择，但是如果你要使用一些`AWS Public Datasets`，比如`Human genome project`或者`Genbank`之类的数据，需要添加`volume`的`snapshot`，这些`snapshot`往往是限定区域的。比如`Genbank`数据你只能在`us-east`区域使用，也就是说你要用这个`snapshot`，你必须把你的`Instance`建在`US East(N.Virginia)`区域。

#### 3.1.2 建立 Instance
在`EC2 Dashboard`左侧列表里点击`Instances`，然后点击`Launch Instance`，会弹出如下图所示的一步步配置Instance的界面。

**2.1: Choose an Amazon Machine Image (AMI)**

在Image界面里选择*Ubuntu Server 14.04LTS*。

![Instance1](../assets/images/1.4/01.png)

**2.2: Choose an Instance Type**

Type 选择 `t2.micro`
是因为这个配置是1年免费的。缺点当然是计算资源比较低，而且当你的计算资源超限额后，会自动把 CPU 性能降低到大概 1/10 左右的情况。要注意的是虽然是免费的，但本质上是1个月720hour的免费计算。如果你同时开了多个 t2.micro Instances，没有在不计算的时候停机stop，仍旧会超出计算时间，被扣美金的。所以如果开了多个Instances，千万别忘了不用的时候关闭。

然后就可以点击`review and launch`。如果想再定制其他配置可以点击`Next: Configure Instance Details`。当需要自动加载 `docker`，或者添加数据`Volumes`的时候要到后面的步骤里设置。

![Instance2](../assets/images/1.4/02.png)

**2.3: Review Instance Launch`**

确认配置无误就点击 Launch，会弹出一个`Select an existing key pair or create a new key pair`选项窗口。这一步很重要，第一次建立新的 Instance 的话，下拉菜单选择`Create a new key pair`。然后给 key pair 取个好记的名字，比如**bac_ngs**，然后点击`Download Key Pair`，就会下载这个密钥文件到你的电脑上。名字叫**bac_ngs.pem**。这时候底部的`Launch Instance`按钮就变成可以点击状态，把你的鼠标按上去，你的Instance就开始初始化了。

#### 3.1.3 访问 Instance
如果你的本地电脑操作系统是Linux，可以直接开一个终端程序运行ssh来访问Instance，如果是Windows电脑，推荐使用putty来访问。

**3.1 Linux**

将下载的 bac-ngs.pem 文件放在一个妥善的位置上，这里放在~/.pem

```bash
$ mkdir -p .pem
$ mv ~/downloads/bac-ngs.pem ~/.pem
$ chmod 400 ~/.pem/bac-ngs.pem
```

现在使用openssh远程访问 AWS Instance 实例。远程的地址可以使用 Dashboard 的 Instance 界面里对应的Public Domain 或者 IP 信息。因为选择的是 ubuntu 操作系统，直接使用 ubuntu 作为用户名，使用 bac-ngs.pem 密钥文件来登录。

```bash
$ ssh -i ~/.pem/bac-ngs.pem ubuntu@your_public_ip
```

连接成功的话就能看到 ubuntu 的登录欢迎界面，就可以开始你的生物信息软件配置过程了。服务器和本地传递数据最方便的方法之一是使用scp命令，如果本地数据要传到服务器的`/home/ubuntu/data`位置上，可以用下面命令：

```bash
$ scp -i ~/.pem/bac-ngs.pem ~/data/your_data.fasta ubuntu@your_public_ip:/data
```

如果是要把服务器上`/home/ubuntu/data/001.tar.gz`文件传输给本地电脑的话，可以使用以下方式：

```bash
$ scp  ~/.pem/bac-ngs.pem ubuntu@your_public_ip:/data/001.tar.gz .
```

**3.2 Windows**

对于 windowsxp 或 windows7 可以使用 putty 来建立与 aws ec2 的 ssh 连接，参考这个[教程](http://ged.msu.edu/angus/tutorials-2013/log-in-with-ssh-win.html)。windows8 用户可以用 zoc 来访问，具体操作请自行搜索教程。

#### 3.1.4 Running docker in Amazon EC2

docker 是一种虚拟环境，可以很方便的在不同机器上配置环境，近年来快速发展，在云计算服务的领域被高度关注。事实上它的启动速度很快，配置也特别方便，很适合生物信息计算的自动化配置。它对生物学家最大的好处是直接使用他人创建的虚拟镜像，减少安装和部署的学习与工作曲线。或者根据自己的需求自己创建相应镜像，快速部署与使用。

##### 安装 docker

AWS 目前的各个 linux 发行版都已经支持 docker。你可以在上一部分创建好的 AWS EC2 Instance里安装docker:

```
ubuntu@public_ip:~$ sudo apt-get install docker.io
```

ubuntu 版本默认的安装包版本比较低，如果想安装最新版本的 docker，需要添加源：

```
$ sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 36A1D7869245C8950F966E92D8576A8BA88D21E9
$ sudo bash -c "echo deb https://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list"
$ sudo apt-get update
$ sudo apt-get install lxc-docker
```

##### 运行 docker**

接下来我们示例如何安装在docker hub 上他人开发的镜像，假设已经登录到 AWS 并已安装完 docker：

**1. 获取镜像**

首先我们用 docker 的搜索命令，查找含有名字 ngseasy-fastqc 的所有镜像。你会看到终端会打印出所有含有 ngseasy-fastqc 的镜像。

```
ubuntu@public_ip:~$ sudo docker search ngseasy-fastqc
```

因为 docker 镜像非常灵活，可以随时修改他人的镜像成为自己的，所以你旺旺能看到多个不同用户创建的相同镜像，这里我们能看到2个用户 snewhouse 和 compbio 创建的 ngseasy-fastqc 镜像。所以区别不同镜像是通过用*户名/镜像名:版本号*来区分的。如果不加版本号则已lastest作为默认。


```
NAME                                 DESCRIPTION
snewhouse/ngseasy-fastqc             NGSeasy fastqc
compbio/ngseasy-fastqc
```

下载镜像 snewhouse/ngseasy-fastqc。

```
ubuntu@public_ip:~$ sudo docker pull compbio/ngseasy-fastqc
```

**2. 启动容器**

启动容器有2种方式，我们需要在容器中进行交互，因此选择基于镜像新建容器并启动。

```
ubuntu@public_ip:~$ sudo docker run -t -i compbio/ngseasy-fastqc /bin/bash
root@32d858986db7:/# su pipeman
```

这样你可以开始输入 fastqc 运行程序。

##### 配置适合自己需求的镜像

**1. Base image**

首先我们需要建立 docker 账号，然后找一个 base image。这里我们用 ubuntu 来创建。首先搜索一下 docker hub 里的 ubuntu 官方镜像。

```
ubuntu@public_ip:~$ sudo docker search ubuntu | grep Official
```

**2. 创建Dockerfile**

docker 镜像在创建容器时，主要通过 Dockfile 来配置，本质上是一个类似 shell 脚本来建立部署的工作流。下面举一个最简单的安装 blast 的 dockerfile 例子。

Dockerfiles

```
FROM ubuntu:12.04
MAINTAINER My_Name my_email@mail.com

RUN apt-get update && apt-get dist-upgrade -y
RUN apt-get install -y blast
RUN apt-get autoclean && apt-get autoremove -y && rm -rf /var/lib/{apt,dpkg,cache,log}/
```

更完整的实例可以参考官方文档。

**3. 添加工具**

```
ubuntu@public_ip:~$
```

##### 进一步学习 Docker

如果想对 docker 有更深入的了解，可以先阅读 [Docker 从入门到实践](http://yeasy.gitbooks.io/docker_practice/)，或者阅读 [Docker 官方文档](https://docs.docker.com/)。关注 docker 技术的朋友推荐订阅

##### 可用镜像

国外有一些研究者建立了含有不同生物信息分析软件的 docker images，可以根据需要直接下载并运行，避免了在不同机器中部署的各种配置。比如：

1. [Docker Galaxy](https://github.com/bgruening/docker-galaxy-stable)
2. [KHP-Bioinformatics](https://github.com/KHP-Informatics/ngs)

---

## Reference
1. [Bioconda](https://bioconda.github.io)
2. [Anaconda Doc](https://docs.continuum.io/)
