# Lustre安装 (目前未包括lnet的配置)
# 基本设置（所有节点都需要操作）
不建议在Ubuntu系统上装Lustre,内核不好控制.

### 增加本地库
```bash
sed -i '/^SELINUX=/s/.*/SELINUX=disabled/' /etc/selinux/config
yum install -y yum-utils deltarpm createrepo
vim /etc/yum.repos.d/localrepo.repo
[lustre-server]
name=lustre-server
baseurl=file://{PATH_of_repo}/lustre-server
enabled=0
gpgcheck=0
proxy=_none_

[lustre-client]
name=lustre-client
baseurl=file://{PATH_of_repo}/lustre-client
enabled=0
gpgcheck=0

[e2fsprogs-wc]
name=e2fsprogs-wc
baseurl=file://{PATH_of_repo}/e2fsprogs-wc
enabled=0
gpgcheck=0
```

## Server端的安装（MGS MDS OSS 节点）
### 1. 安装e2fs内核补丁
```bash
yum --nogpgcheck --disablerepo=* --enablerepo=e2fsprogs-wc \
install e2fsprogs2. 
```

### 2. 安装kernel补丁
```bash
yum --nogpgcheck --disablerepo=base,extras,updates \
--enablerepo=lustre-server install \
kernel \
kernel-devel \
kernel-headers \
kernel-tools \
kernel-tools-libs \
kernel-tools-libs-devel
```

### 3. 安装完Kernel后需要重启Reboot

### 4. 安装lustre 组件与内核模块
```bash
yum --nogpgcheck --enablerepo=lustre-server install \
kmod-lustre \
kmod-lustre-osd-ldiskfs \
lustre-osd-ldiskfs-mount \
lustre \
lustre-resource-agents
# 加载内核模块
modprobe -v lustre
modprobe -v ldiskfs
```

## Client端的安装（需要挂载lustre文件系统的节点）
### 1.安装内核kernel
```bash
yum install \
kernel \
kernel-devel \
kernel-headers \
kernel-abi-whitelists \
kernel-tools \
kernel-tools-libs \
kernel-tools-libs-devel
```
### 2.重启Reboot

### 3.安装lustre客户端
```bash
yum --nogpgcheck --enablerepo=lustre-client install \
kmod-lustre-client \
lustre-client

modprobe -v lustre
```

## 创建Lustre 文件系统
### 创建MGS（MGT）服务器
```bash
# 单节点时，--servicenode=mds1@tcp0 省略就行  
# /dev/vdb 会被格式化 
# --fsname=lustrefs  会用于在客户端显示
mkfs.lustre --fsname=lustrefs --reformat --mgs --servicenode=mds1@tcp0  /dev/vdb

mkdit /mgs
mount -t lustre /dev/vdb /mgs
```   
### 创建MDS(MDT) 服务器
```bash
#--index=0  按顺序递增即可，便于内核debug
mkfs.lustre --mdt --fsname=lustrefs --index=0  --mgsnode=mdsip@tcp0 --reformat /dev/vdc

#开启quota
tune2fs -O project /dev/vdb

#挂载目录
mkdir /mdt
mount -t lustre /dev/vdc /mdt
```
### 创建OSS(OST) 服务器
```bash
mkfs.lustre --fsname=lustrefs --ost --reformat --index=0 --mgsnode=mdsip@tcp0 /dev/vdb

#开启quota
tune2fs -O project,quota /dev/vdb

mkdir /ost1
mount -t lustre /dev/vdb /ost1
```

### 客户端挂载
```bash
mkdir /lustre

mount -t lustre mdsip:/fsname /lustre
```