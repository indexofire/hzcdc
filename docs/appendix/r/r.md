# R

## 安装

在 Ubuntu 16.04 中安装 R

```bash
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9
$ sudo add-apt-repository 'deb [arch=amd64,i386] https://cran.rstudio.com/bin/linux/ubuntu xenial/'
$ sudo apt-get update
$ sudo apt-get install r-base

# install packages as global
$ sudo -i R
$ sudo apt install rstudio
```

```R
# in R environment
> install.packages("some_package_name")
```
