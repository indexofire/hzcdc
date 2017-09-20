# Docker

![banner](../assets/images/9.1/banner.jpg)

## 安装 docker CE for Ubuntu

```bash
# 确保删除旧版本
$ sudo apt remove docker docker-engine docker.io

# Ubuntu 14.04 版本的用户要安装 linux-image-extra-*
$ sudo apt-get update
$ sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual

# 安装 docker ce
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
$ sudo apt-key fingerprint 0EBFCD88
$ sudo add-apt-repository "deb [arch=amd64] \
> https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
$ sudo apt update
$ sudo apt install docker-ce
```
