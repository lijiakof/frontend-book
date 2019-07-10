# 制作 SSL 免费证书
网站为了安全和获取更多 HTTP 相关功能，都会使用 HTTPS 协议，当然 HTTPS 需要有证书认证才能使用。有专门的机构提供 SSL 证书，例如：Symantec、 腾讯云 SSL、阿里云 SSL 等等，虽然它使用简单但是这些都是需要钱的，少则上千多则上万（还是每年），那么有没有给个人使用的免费的 SSL 证书呢？当然是有的 [Let's Encrypt](https://letsencrypt.org/) 是国外一家公共免费的提供 SSL 的项目，它一次签发提供有效期 90 天，当然我们可以通过程序来自动续约，虽然有点麻烦，但是省去一大笔费用。我们看如何制作吧：

* 安装 certbot 软件
* 生成证书
* 应用的网站

## Mac 制作方法
参考：https://certbot.eff.org/lets-encrypt/osx-other.html

### 先安装 homebrew 
进入官网：https://brew.sh/ ，它是一个 mac 的软件管理工具：

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 安装 certbot
certbot 是生成证书的命令行工具

```
brew install letsencrypt
```

### 生成证书
我们在这里介绍本地生成证书，如果是在 Web 服务器上，可以参考：https://certbot.eff.org/instructions

```
sudo certbot certonly --standalone
```

接下来下一步、下一步按部就班，但是如果你的域名解析的是 CNAME 类型的，这种方式会报错：

那么就需要这样：

* 第一步：输入一下命令行：

```
sudo certbot -d www.xxx.com --manual --preferred-challenges dns certonly
```

* 第二步：会让你将 _acme-challenge.www.xxx.com 域名用 TXT 的方式解析到 `wkOfTI4uw9JZaQYiS15tv1m4kZonXtBIUzJO8SOgTdY` 上。去相关的域名解析平台上操作；
* 第三步：继续上面命令的操作；
* 第四步：生成证书、私钥等到响应的目录中；

### 应用的网站
每个网站有不同的架构和部署结构，有的有负载均衡、有的是 Nginx、有的是 Tomcat，导入证书各有各的方式，自行处理。

## Liunx
TODO：

