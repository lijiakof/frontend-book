# Hasura

* 安装环境
* 第一步：获取 docker-compose 文件
* 第二部：启动 Hasura GraphQL engine & Postgres
* 第三部：打开 Hasura 控制台

## 安装环境
### Docker

1、安装相关包

```
$ sudo yum install -y yum-utils \
    device-mapper-persistent-data \
    lvm2
```

2、使用稳定版源

```
$ sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
```

3、安装 Docker 引擎

```
$ sudo yum install docker-ce docker-ce-cli containerd.io
```

4、如果安装特殊版本

```
$ yum list docker-ce --showduplicates | sort -r
docker-ce.x86_64  3:18.09.1-3.el7                     docker-ce-stable
docker-ce.x86_64  3:18.09.0-3.el7                     docker-ce-stable
docker-ce.x86_64  18.06.1.ce-3.el7                    docker-ce-stable
docker-ce.x86_64  18.06.0.ce-3.el7                    docker-ce-stable

$ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
```

5、启动

```
$ sudo systemctl start docker
$ sudo systemctl status docker
$ sudo docker run hello-world

# 开机启动
$ sudo systemctl enable docker
```

6、问题：

```
Error: Package: 3:docker-ce-19.03.5-3.el7.x86_64 (docker-ce-stable)
           Requires: container-selinux >= 2:2.74
Error: Package: containerd.io-1.2.10-3.2.el7.x86_64 (docker-ce-stable)
           Requires: container-selinux >= 2:2.74
```

7、aws 安装

```
$ sudo syum install -y docker 
```

### Docker Compose

1、下载 docker-compose

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

国内速度慢，换成下面的：

```
sudo curl -L "https://get.daocloud.io/docker/compose/releases/download/1.25.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

2、增加可自行权限

```
$ sudo chmod +x /usr/local/bin/docker-compose
```

3、创建链接

```
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```

4、测试

```
$ docker-compose --version
```

### 国内镜像源加速

```
$ sudo vim /etc/docker/daemon.json

{
    "registry-mirrors": [
        "https://dockerhub.azk8s.cn",
        "https://hub-mirror.c.163.com"
    ]
}
```

## 第一步：获取 docker-compose 文件

```
# in a new directory
wget https://raw.githubusercontent.com/hasura/graphql-engine/master/install-manifests/docker-compose/docker-compose.yaml
```

–no-check-certificate

## 第二部：启动 Hasura GraphQL engine & Postgres

```
$ docker-compose up -d
```

## 第三部：打开 Hasura 控制台
在浏览器中输入：http://localhost:8080/console