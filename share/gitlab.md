# Gitlib
https://docs.gitlab.com/ee/README.html

## 安装
https://about.gitlab.com/install/
* 安装依赖
    * sudo yum install -y curl policycoreutils-python openssh-server
    * sudo systemctl enable sshd
    * sudo systemctl start sshd
    * sudo firewall-cmd --permanent --add-service=http
    * sudo firewall-cmd --permanent --add-service=https
    * sudo systemctl reload firewalld
* 安装 Postfix
    * sudo yum install postfix
    * sudo systemctl enable postfix
    * sudo systemctl start postfix
* 安装
    * curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash
    * sudo EXTERNAL_URL="https://gitlab.example.com" yum install -y gitlab-ee

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

