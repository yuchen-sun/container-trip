# Install docker
<!-- toc -->

- [centos](#Centos)
- [ububtu](#Ubuntu)
- [Other linux distributions](#Other-linux-distributions)

<!-- /toc -->

## Centos
卸载旧版本
```bash
 sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine
```
### 使用yum安装
设置存储库并安装最新版
```bash
sudo yum install -y yum-utils
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
```
### 安装特定版本
```bash
yum list docker-ce --showduplicates | sort -r |awk {'print $2'}| sed "s/3:/-/g" |sed "s/-3.el7//g"|grep -v ce|sed -n '/[1-2]/p'
```
```bash
#输出结果
-20.10.2
-20.10.1
-20.10.1
-20.10.0
-19.03.9
-19.03.8
-19.03.7
-19.03.6
-19.03.5
-19.03.4
-19.03.3
```
在上述命令的结果中选择一个替换<version>即可
```cli
sudo yum install docker-ce<version> docker-ce-cli<version> containerd.io
```

## Ubuntu
卸载旧版本
```bash
sudo apt-get remove docker docker-engine docker.io containerd runc
```
### 使用apt安装
设置库并安装
```bash
sudo apt-get update
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io   
sudo yum install docker-ce docker-ce-cli containerd.io
```
### 安装特定版本
```bash
apt-cache madison docker-ce |awk {'print $3'}
```
```bash
#输出结果
5:20.10.2~3-0~ubuntu-bionic
5:20.10.1~3-0~ubuntu-bionic
5:20.10.0~3-0~ubuntu-bionic
5:19.03.14~3-0~ubuntu-bionic
5:19.03.13~3-0~ubuntu-bionic
5:19.03.12~3-0~ubuntu-bionic
5:19.03.11~3-0~ubuntu-bionic
5:19.03.10~3-0~ubuntu-bionic
```
在上述命令的结果中选择一个替换<version>即可
```bash
sudo yum install docker-ce=<version> docker-ce-cli=<version> containerd.io
```

## Other linux distributions
[参考链接](https://docs.docker.com/engine/install/)