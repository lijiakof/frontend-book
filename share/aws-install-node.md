# 如何在 AWS 中安装 Nodejs
AWS 有强大的 yum，可以安装很多 Linux 程序，但是要安装 Nodejs 需要做一些设置

## 设置 EPEL 仓库为可用状态

```
sudo nano /etc/yum.repos.d/epel.repo
```

设置 enabled 为 1

```
[epel]
name=Extra Packages for Enterprise Linux 6 - $basearch
#baseurl=http://download.fedoraproject.org/pub/epel/6/$basearch
mirrorlist=https://mirrors.fedoraproject.org/metalink?repo=epel-6&arch=$basearch
failovermethod=priority
enabled=1 //<--修改这个部分
```

## 更新 yum 仓库

```
sudo yum update
```

## 安装 npm（同时会安装 Nodejs）

```
sudo yum install npm
```

## 检查版本

```
node -v
```