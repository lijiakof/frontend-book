# OpenVPN

## 安装 OpenVPN
* 更新 yum：sudo yum update -y
* 安装环境：sudo yum install epel-release -y
* 安装：sudo yum install openvpn -y
* 安装 easy-rsa
    * 下载：wget -O /tmp/easyrsa https://github.com/OpenVPN/easy-rsa-old/archive/2.3.3.tar.gz
    * 解压：tar xfz /tmp/easyrsa
    * 创建：sudo mkdir /etc/openvpn/easy-rsa
    * 拷贝：sudo cp -rf easy-rsa-old-2.3.3/easy-rsa/2.0/* /etc/openvpn/easy-rsa

## 配置
* 拷贝配置：sudo cp /usr/share/doc/openvpn-2.4.4/sample/sample-config-files/server.conf /etc/openvpn/
* 修改配置：/etc/openvpn/server.conf

```
// 设置：
;push "route 192.168.10.0 255.255.255.0"
push "route 10.5.0.0 255.255.0.0"
push "route 10.0.0.0 255.255.0.0"

// 改？
;push "dhcp-option DNS 208.67.222.222"
;push "dhcp-option DNS 208.67.220.220"

// 设置：同一证书可以重复登录
duplicate-cn

// 设置：用户
user nobody
group nobody
```

## 生成服务端证书
* 修改默认配置：sudo vim /etc/openvpn/easy-rsa/vars

```
export KEY_COUNTRY="US"
export KEY_PROVINCE="NY"
export KEY_CITY="New York"
export KEY_ORG="Organization Name"
export KEY_EMAIL="administrator@example.com"
export KEY_CN=yoursiteexample.com
export KEY_NAME=server
export KEY_OU=server
```

* 获取 root 权限： sudo su
* source ./vars
* 清空认证：./clean-all
* 建立认证：./build-ca
* 设置建立的服务器 key：./build-key-server server
* 建立 dh 协议：./build-dh
* 将认证文件拷贝到 keys 中：
    * cd /etc/openvpn/easy-rsa/keys
    * cp dh2048.pem ca.crt server.crt server.key /etc/openvpn
* 创建 ta.key：openvpn --genkey --secret /etc/openvpn/ta.key

## 修改网络设置
* sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
    * 如果是专用 VPN 首先查看当前规则：sudo iptables --table nat -L POSTROUTING
    * 如果存在规则删除：sudo iptables -t nat -D POSTROUTING 1
    * 添加专用规则：sudo iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -d 10.0.11.59/32 -o eth0 -j MASQUERADE
* sudo service iptables save
* 修改系统运行参数
    * vim /etc/sysctl.conf
    * 修改：net.ipv4.ip_forward = 1
    * sudo sysctl -p

## 生成客户端 Key
* cd /etc/openvpn/easy-rsa/
* sudo su
* source ./vars
* ./build-key-pass {username}
* ./build-ovpn-config {username}

```
#!/bin/sh

if [ x$# != x1 ]
then
  echo 'Usage : build-ovpn-config {username}'
else
  echo 'Begin to build ovpn config file for '$1

  echo 'client' > $1.ovpn
  echo 'dev tun' >> $1.ovpn
  echo 'proto udp' >> $1.ovpn
  echo 'remote 52.81.109.5 1194' >> $1.ovpn
  echo 'resolv-retry infinite' >> $1.ovpn
  echo 'nobind' >> $1.ovpn
  echo 'persist-key' >> $1.ovpn
  echo 'persist-tun' >> $1.ovpn
  echo 'comp-lzo' >> $1.ovpn
  echo 'verb 3' >> $1.ovpn
  echo 'remote-cert-tls server' >> $1.ovpn

  echo '<ca>' >> $1.ovpn
  cat keys/ca.crt >> $1.ovpn
  echo '</ca>' >> $1.ovpn

  echo '<cert>' >> $1.ovpn
  cat keys/$1.crt >> $1.ovpn
  echo '</cert>' >> $1.ovpn

  echo '<key>' >> $1.ovpn
  cat keys/$1.key >> $1.ovpn
  echo '</key>' >> $1.ovpn

  cp $1.ovpn keys/$1.ovpn
fi
```

## 启动
* sudo service openvpn start
* /var/log/message

整个安装过程非常的繁琐。

## 更快乐的安装方式
上面的方式太过于繁琐，并且一步没弄好都不能达到效果，幸运的是好心的工程师把整个安装过程做成了 shell 脚本，并且支持各个版本的系统：https://github.com/angristan/openvpn-install

只需要将脚本下载下来，然后运行`openvpn-install`一步一步按照提示安装就行了：

### 下载安装脚本
```
curl -O https://raw.githubusercontent.com/angristan/openvpn-install/master/openvpn-install.sh
sudo chmod 755 openvpn-install.sh
```

### 运行安装过程
```
sudo ./openvpn-install.sh
Welcome to the OpenVPN installer!
The git repository is available at: https://github.com/angristan/openvpn-install

I need to ask you a few questions before starting the setup.
You can leave the default options and just press enter if you are ok with them.

I need to know the IPv4 address of the network interface you want OpenVPN listening to.
Unless your server is behind NAT, it should be your public IPv4 address.
IP address: 10.5.211.148

It seems this server is behind NAT. What is its public IPv4 address or hostname?
We need it for the clients to connect to the server.
Public IPv4 address or hostname: 54.222.158.30
Checking for IPv6 connectivity...

Your host does not appear to have IPv6 connectivity.

Do you want to enable IPv6 support (NAT)? [y/n]: n

What port do you want OpenVPN to listen to?
   1) Default: 1194
   2) Custom
   3) Random [49152-65535]
Port choice [1-3]: 1

What protocol do you want OpenVPN to use?
UDP is faster. Unless it is not available, you shouldn't use TCP.
   1) UDP
   2) TCP
Protocol [1-2]: 1

What DNS resolvers do you want to use with the VPN?
   1) Current system resolvers (from /etc/resolv.conf)
   2) Self-hosted DNS Resolver (Unbound)
   3) Cloudflare (Anycast: worldwide)
   4) Quad9 (Anycast: worldwide)
   5) Quad9 uncensored (Anycast: worldwide)
   6) FDN (France)
   7) DNS.WATCH (Germany)
   8) OpenDNS (Anycast: worldwide)
   9) Google (Anycast: worldwide)
   10) Yandex Basic (Russia)
   11) AdGuard DNS (Russia)
   12) Custom
DNS [1-12]: 3

Do you want to use compression? It is not recommended since the VORACLE attack make use of it.
Enable compression? [y/n]: n

Do you want to customize encryption settings?
Unless you know what you're doing, you should stick with the default parameters provided by the script.
Note that whatever you choose, all the choices presented in the script are safe. (Unlike OpenVPN's defaults)
See https://github.com/angristan/openvpn-install#security-and-encryption to learn more.

Customize encryption settings? [y/n]: n

Okay, that was all I needed. We are ready to setup your OpenVPN server now.
You will be able to generate a client at the end of the installation.
Press any key to continue...

.....

Tell me a name for the client.
Use one word only, no special characters.
Client name: test

Do you want to protect the configuration file with a password?
(e.g. encrypt the private key with a password)
   1) Add a passwordless client
   2) Use a password for the client
Select an option [1-2]: 2
⚠️ You will be asked for the client password below ⚠️

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Generating a 256 bit EC private key
writing new private key to '/etc/openvpn/easy-rsa/pki/private/test.key.P5qngQvwWk'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf
Check that the request matches the signature
Signature ok
The Subject's Distinguished Name is as follows
commonName            :ASN.1 12:'test'
Certificate is to be certified until Dec 22 02:13:15 2022 GMT (1080 days)

Write out database with 1 new entries
Data Base Updated

Client test added, the configuration file is available at /home/ec2-user/test.ovpn.
Download the .ovpn file and import it in your OpenVPN client.
```

### 新增用户
```
sudo ./openvpn-install.sh
Welcome to OpenVPN-install!
The git repository is available at: https://github.com/angristan/openvpn-install

It looks like OpenVPN is already installed.

What do you want to do?
   1) Add a new user
   2) Revoke existing user
   3) Remove OpenVPN
   4) Exit
Select an option [1-4]: 1

```
后面的步骤同安装后半部分。

### 删除用户
```
sudo ./openvpn-install.sh
Welcome to OpenVPN-install!
The git repository is available at: https://github.com/angristan/openvpn-install

It looks like OpenVPN is already installed.

What do you want to do?
   1) Add a new user
   2) Revoke existing user
   3) Remove OpenVPN
   4) Exit
Select an option [1-4]: 2


Select the existing client certificate you want to revoke
    1) test
    2) test2
Select one client [1-2]: 1
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf
Revoking Certificate AF5A8F906E3085698BA96A340794A2D2.
Data Base Updated

Note: using Easy-RSA configuration from: ./vars

Using SSL: openssl OpenSSL 1.0.2k-fips  26 Jan 2017
Using configuration from /etc/openvpn/easy-rsa/pki/safessl-easyrsa.cnf

An updated CRL has been created.
CRL file: /etc/openvpn/easy-rsa/pki/crl.pem


Certificate for client test revoked.
```