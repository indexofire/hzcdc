# 细菌基因组重组检测

细菌做为原核生物，不光以点突变带来遗传变化，重组也是重要的进化驱动事件。细菌以种
群的方式存在，因此种群间或种群内往往都存在着频繁的重组现象。

重组根据机制可以分为：

* 转换
* 转导
* 接合
* 溶源性转变

根据变化的序列特性可以分为：

* 同源性重组：由同源性较高的序列介导，替换后序列接近
* 基因水平转移：而对于非同源性序列的替换

一般而言点突变事件发生的频率是比价稳定的，可以用来做物种系统发育和进化关系分析，
而同源重组重组事件在进化上会改变细菌的遗传距离，如果将重组做为点突变来构建系统发
育会改变遗传进化结果。因此在做物种系统发育分析时，应将重组带来的碱基变化去除。

重组事件有可能会被误认为是正向选择压力（自然选择）事件，因此需要需要对其进行筛选。

## ClonalFrameML




## Gubbins

### 安装 Gubbins

```
# for ubuntu users
$ sudo apt install gubbins

# 发行版版本冻结在1.x上，如果要使用2.x，比如不想用fastml的可以从源码编译
$ sudo apt install libtool python3-setuptools python3-dev
$ git clone https://github.com/sanger-pathogens/gubbins
$ cd gubbins
$ ./autoreconf -i
$ ./configure
$ make && sudo make install
$ cd python && sudo python3 setup.py install
```

### 使用 Gubbins

#### 创建 fasta 格式的基因组比对文件
