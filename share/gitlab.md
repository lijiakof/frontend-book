# Gitlib
https://docs.gitlab.com/ee/README.html

## 安装
我们以 CentOS 为例：

1. 安装依赖

```
sudo yum install -y curl policycoreutils-python openssh-server
sudo systemctl enable sshd
sudo systemctl start sshd
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo systemctl reload firewalld
```

2. 安装 Postfix

```
sudo yum install postfix
sudo systemctl enable postfix
sudo systemctl start postfix
```

3. 安装

```
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
sudo EXTERNAL_URL="https://gitlab.example.com" yum install -y gitlab-ee
```

*参考：https://about.gitlab.com/install/*

## 配置 SSL
* external_url "https://gitlab.example.com"
* nginx['enable'] = true
* nginx['redirect_http_to_https'] = true
* nginx['ssl_certificate'] = "/etc/gitlab/ssl/xx.crt"
* nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/xx.key"

## 启动
* sudo gitlab-ctl start
* sudo gitlab-ctl stop
* sudo gitlab-ctl reconfigure

## CI

* sudo yum install gitlab-runner
* sudo gitlab-runner register
    * 通过 root 用户登陆 gitlab 站点，找到相关的 URL 和 token
* 在项目中添加 .gitlab-ci.yml 配置文件
* sudo gitlab-runner restart

```
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://xxxxxx/
Please enter the gitlab-ci token for this runner:
xxxxxx
Please enter the gitlab-ci description for this runner:
[ip-10-5-201-124.cn-north-1.compute.internal]: tripio gitlab runner
Please enter the gitlab-ci tags for this runner (comma separated):
x-tag,y-tag
Registering runner... succeeded
Please enter the executor: virtualbox, custom, docker-ssh, parallels, ssh, kubernetes, docker, shell, docker+machine, docker-ssh+machine:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded!
```

去除对 https 的校验
因runner运行时的执行者是gitlab-runner账户，需要在gitlab-runner账号下设置访问https类网站时，免验证

```
su - gitlab-runner
git config --global http."sslVerify" false
cat /home/gitlab-runner/.gitconfig 
[http]
    sslVerify = false 
```

安装 Node

```
curl --silent --location https://rpm.nodesource.com/setup_12.x | sudo bash -
sudo yum install -y nodejs
curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
sudo yum install yarn
```

## GitLab Pages
* pages_external_url "http://pages.example.com/"
* gitlab_pages['enable'] = false
* sudo gitlab-ctl stop
* sudo gitlab-ctl reconfigure
* sudo gitlab-ctl start

部署地址：/var/opt/gitlab/gitlab-rails/shared/pages
Nginx 配置：/var/opt/gitlab/nginx/conf/gitlab-pages.conf

