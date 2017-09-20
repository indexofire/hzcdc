# Python 数据科学速览

![banner](../assets/images/a2.2/banner.png)

数据科学的快速发展，2大语言 Python 和 R 成为了

现在让我们坐上这辆 Python 轻轨，出发！

## 第一站. "木星"姐妹花的奇异之旅

没错，第一站我们就到了美丽的"木星" - Jupiter 的姐妹花 Jupyter。

```bash
$ conda create env jupyter python=3.6
$ source activate jupyter
# 进入 jupyter 虚拟环境
(jupyter)$ cd $CONDA_PREFIX
```

ipython 意思为 interactive python，比 python idle 来说丰富了许多交互的功能。

运行 jupyter notebook

```bash
# 选择一个目录保存 jupyter 文件
(jupyter)$ mkdir -p tutorial && cd tutorial
(jupyter)$ jupyter notebook
```

这时候你的浏览器就会开启一个tab页面，默认的访问地址是 localhost:8888 也就是本机 tutorial 目录。可以看到目前 tutorial 文件夹没有内容，没关系我们来创建一个 jupyter notebook(.ipynb)文件。

## 第二站. 熊猫馆里的三贱客

```bash
(jupyter)$ conda install pandas matplotlib numpy
```

前面我们已经介绍了 jupyter，接下来我们就要在 jupyter 里使用 pandas, matplotlib 和 numpy 这三兄弟进行数据可视化。

## 第三站. 海上钢琴师的美妙演奏

```bash
(jupyter)$ conda install seaborn
```

## 第四站. 一切都被虚化的背景世界

```bash
(jupyter)$ conda install bokeh
```

## 第五站. 雄心勃勃的阴谋家

```bash
(jupyter)$ conda install plotly
```



## python packages

[Awesone data visualization tools in python](https://www.youtube.com/watch?v=OC-YdBz8Llw&t=594s) 这个视频简单总结的关于数据可视化的各个python packages关系和来源。


## Reference
