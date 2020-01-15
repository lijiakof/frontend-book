# ELK

## 系统框架
ELK是Elasticsearch、Logstash、Kibana的简称，是我们搭建的日志分析系统的核心部分。另外为了方便日志的收集我们启用了另一个工具Filebeat

### Elasticsearch
Elasticsearch是一个基于Lucene库的搜索引擎。它提供了一个分布式、支持多用户的全文搜索引擎，具有HTTP Web接口和无模式JSON文档。Elasticsearch是用Java开发的，并在Apache许可证下作为开源软件发布。官方客户端在Java、.NET（C#）、PHP、Python、Apache Groovy、Ruby和许多其他语言中都是可用的。

### Logstash
Logstash是一个用来搜集、分析、过滤日志的工具。它支持几乎任何类型的日志，包括系统日志、错误日志和自定义应用程序日志。它可以从许多来源接收日志，这些来源包括 syslog、消息传递（例如 RabbitMQ）和JMX，它能够以多种方式输出数据，包括电子邮件、websockets和Elasticsearch。

### Kibana
Kibana是一个基于Web的图形界面，用于搜索、分析和可视化存储在 Elasticsearch中的数据。它利用Elasticsearch的REST接口来检索数据，允许用户创建他们自己的数据的定制仪表板视图，并以特殊的方式查询和过滤数据。

### Filebeat
Filebeat是本地文件的日志数据采集器，可监控日志目录或特定日志文件（tail file），并将它们转发给Elasticsearch或Logstatsh进行索引、kafka等。带有内部模块（auditd，Apache，Nginx，System和MySQL），可通过一个指定命令来简化通用日志格式的收集，解析和可视化。

## 系统落地
基于自己目前的需求我们把框架大致描述如下


Filebeat部署在日志产生的机器，实时监控增量日志并发送给Logstash集中处理，Logstash通过不同的filter过滤并处理日志到需要的格式，然后发送给Elasticsearch，通过Kibana就可以实现日志的查看和分析。Elasticsearch可以做集群部署，Logstash发送日志时就要发送给所有机器。

## 安装
目前日志的数据量还不算太大，因此打算对Elasticsearch+Logstash+Kibana进行单机部署，环境CentOS Linux 7

### 安装JDK
Elasticsearch需要依赖Java环境，因此需要先安装JDK

```
$ sudo yum install java-1.8.0-openjdk
$ java -version
openjdk version "1.8.0_191"
OpenJDK Runtime Environment (build 1.8.0_191-b12)
OpenJDK 64-Bit Server VM (build 25.191-b12, mixed mode)
```

### 安装 Elasticsearch
1、选择下载合适的编译版本

```
$wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.3.1.tar.gz
```

2、解压文件

```
$ tar xzvf elasticsearch-6.3.1.tar.gz
```

3、查看目录结构

```
LICENSE.txt
NOTICE.txt
README.textile
bin
config
data
lib
logs
modules
plugins
```

4、修改配置文件

```
$ vi config/elasticsearch.yml
```

找到 # network.host 一行，修改成以下：
```
network.host: localhost
```

5、创建用户组和用户
因为Elasticsearch不能用root账户启动，因此我们得新建一个用户

```
$ groupadd elasticsearch
$ useradd -g elasticsearch elasticsearch
```

6、启动Elasticsearch

```
$ cd bin
$ ./elasticsearch
```

7、验证

```
$ curl 'localhost:9200/'
{
  "name" : "qc9y6mz",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "HzDB3zAsTPKCYRsISfBqaQ",
  "version" : {
    "number" : "6.3.1",
    "build_flavor" : "default",
    "build_type" : "tar",
    "build_hash" : "eb782d0",
    "build_date" : "2018-06-29T21:59:26.107521Z",
    "build_snapshot" : false,
    "lucene_version" : "7.3.1",
    "minimum_wire_compatibility_version" : "5.6.0",
    "minimum_index_compatibility_version" : "5.0.0"
  },
  "tagline" : "You Know, for Search"
}
```

8、创建服务
为了保证每次开机Elasticsearch服务能自行启动，我们创建一个systemctl的服务

```
$ cd /usr/lib/systemd/system
$ touch elasticsearch.service
$ vi elasticsearch.service
[Unit]
Description=ElasticSearch Service
After=network.target
[Service]
User=elasticsearch
Group=elasticsearch
Type=simple
ExecStart=/home/elk/elasticsearch-6.3.1/bin/elasticsearch
Restart=on-failure
[Install]
WantedBy=default.target
```

设置开机自启动

```
$ systemctl enable elasticsearch.service
$ systemctl start elasticsearch.service
```

### 安装 Kibana
1、选择下载合适的编译版本（版本号跟Elasticsearch保持一致）

```
wget https://artifacts.elastic.co/downloads/kibana/kibana-6.3.1-linux-x86_64.tar.gz
```

2、解压文件

```
$ tar xzvf kibana-6.3.1-linux-x86_64.tar.gz
```

3、查看目录结构

```
LICENSE.txt
NOTICE.txt
README.txt
bin
config
data
node
node_modules
optimize
package.json
plugins
src
webpackShims
yarn.lock
```

4、修改配置文件

```
$ vi config/elasticsearch.yml

server.host: "localhost"
xpack.security.enabled: false
```

5、创建服务

```
$ cd /usr/lib/systemd/system
$ touch kibana.service
$ vi kibana.service
[Unit]
Description=Kibana Service
After=network.target
[Service]
Type=simple
ExecStart=/home/elk/kibana-6.3.1-linux-x86_64/bin/kibana
Restart=on-failure
[Install]
WantedBy=default.target
```

设置开机自启动

```
$ systemctl enable kibana.service
$ systemctl start kibana.service
```

6、Nginx反向代理
由于Kibana我们配置的启动监听localhost:5601，因此需要通过Nginx做一下反向代理才能访问
配置Nginx

server {
    listen       8001;
    location / {
        proxy_pass http://localhost:5601;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


### 安装 Logstash
1、选择下载合适的编译版本（版本号跟Elasticsearch保持一致）

```
wget https://artifacts.elastic.co/downloads/logstash/logstash-6.3.1.tar.gz
```

2、解压文件

```
$ tar xzvf logstash-6.3.1.tar.gz
```

3、查看目录结构

```
CONTRIBUTORS
Gemfile
Gemfile.lock
LICENSE.txt
NOTICE.TXT
bin
config
data
lib
logs
logstash-core
logstash-core-plugin-api
modules
patterns
tools
vendor
x-pack
```

4、创建配置文件

```
$ cd /home/elk/logstash-6.3.1/config
$ touch logstash.conf
```

根据需要创建配置，需要结合Filebeat的配置

```
input {
     # 监听地址和端口，接收来自Filebeat的数据
    beats {
        host => "192.168.31.29"
        port => "5044"
    }
}
filter {
    # 匹配nginx日志数据
    if [fields][service] == "web" {
        grok {
            match => { "message" => ["%{IPORHOST:[nginx][remote_ip]} - %{DATA:[nginx][user_name]} \[%{HTTPDATE:[nginx][time]}\] \"%{WORD:[nginx][method]} %{DATA:[nginx][url]} HTTP/%{NUMBER:[nginx][http_version]:float}\" %{NUMBER:[nginx][response_code]:int} %{NUMBER:[nginx][body_sent][bytes]:int} \"%{DATA:[nginx][referrer]}\" \"%{DATA:[nginx][agent]}\""] }
            remove_field => "message"
        }
        date {
            match => [ "[nginx][time]", "dd/MMM/YYYY:H:m:s Z" ]
            target => "@timestamp"
            remove_field => "[nginx][time]"
        }
        useragent {
            source => "[nginx][agent]"
            target => "[nginx][user_agent]"
            remove_field => "[nginx][agent]"
        }
        geoip {
            source => "[nginx][remote_ip]"
            target => "[geoip]"
        }
        urldecode {
            all_fields => true
        }
    }
    # 匹配接口访问数据
    if [fields][service] == "api-access" {
        grok {
            patterns_dir => ["/home/elk/logstash-6.3.1/patterns"]
            match => { "message" => ["\[%{CTIME:[logdate]}\] \[%{WORD:[category]}\] %{WORD:[scheme]} - %{IPORHOST:[remote_ip]} - \"%{WORD:[method]} %{DATA:[path]} HTTP/%{NUMBER:[http_version]:float}\" %{NUMBER:[response_code]:int} %{NUMBER:[content_length]:int} %{NUMBER:[response_time]:int} \"%{DATA:[referrer]}\" \"%{DATA:[agent]}\""] }
            remove_field => "message"
        }
        useragent {
            source => "[agent]"
            target => "[user_agent]"
            remove_field => "[agent]"
        }
        date {
            match => ["[logdate]", "yyyy-MM-dd'T'HH:mm:ss.SSS"]
            target => "@timestamp"
            remove_field => ["logdate"]
        }
        geoip {
            source => "[remote_ip]"
            target => "[geoip]"
        }
        urldecode {
            all_fields => true
        }
    }
    # 匹配接口访问错误数据
    if [fields][service] == "api-error" {
        grok {
            patterns_dir => ["/home/elk/logstash-6.3.1/patterns"]
            match => { "message" => ["\[%{CTIME:[time]}\] \[%{WORD:[category]}\] %{WORD:[type]} - %{WORD:[method]}:%{DATA:[path]} - %{NUMBER:[response_code]:int} - \[%{DATA:[opeator]}\] - %{NUMBER:[system_code]:int} - %{DATA:[system_msg]} - %{DATA:[system_payload]} - %{GREEDYDATA:[system_data]}"] }
            remove_field => "message"
        }
        date {
            match => ["[logdate]", "yyyy-MM-dd'T'HH:mm:ss.SSS"]
            target => "@timestamp"
            remove_field => ["logdate"]
        }
        urldecode {
            all_fields => true
        }
    }
    # 匹配接口操作数据
    if [fields][service] == "api-info" {
        grok {
            # 自定义正则匹配路径
            patterns_dir => ["/home/elk/logstash-6.3.1/patterns"]
            match => { "message" => ["\[%{CTIME:[time]}\] \[%{WORD:[category]}\] %{WORD:[type]} - %{WORD:[method]}:%{DATA:[path]} - %{NUMBER:[response_code]:int} - \[%{DATA:[opeator]}\] - %{NUMBER:[system_code]:int} - %{DATA:[system_msg]} - %{DATA:[system_payload]} - %{GREEDYDATA:[system_data]}"] }
            remove_field => "message"
        }
        date {
            match => ["[logdate]", "yyyy-MM-dd'T'HH:mm:ss.SSS"]
            target => "@timestamp"
            remove_field => ["logdate"]
        }
        urldecode {
            all_fields => true
        }
    }
}
output {
    if [fields][service] == "web" {
        # 输出到elasticsearch 索引logstash-web-%{+YYYY.MM.dd}
        elasticsearch {
            hosts => ["localhost:9200"]
            index => "logstash-web-%{+YYYY.MM.dd}"
               manage_template => true
        }
    }
    if [fields][service] == "api-access" {
        # 输出到elasticsearch 索引logstash-api-access-%{+YYYY.MM.dd}
        elasticsearch {
            hosts => ["localhost:9200"]
            index => "logstash-api-access-%{+YYYY.MM.dd}"
               manage_template => true
        }
    }
    if [fields][service] == "api-error" {
        # 输出到elasticsearch 索引logstash-api-error-%{+YYYY.MM.dd}
        elasticsearch {
            hosts => ["localhost:9200"]
            index => "logstash-api-error-%{+YYYY.MM.dd}"
            manage_template => true
        }
    }
    if [fields][service] == "api-info" {
        # 输出到elasticsearch 索引logstash-api-info-%{+YYYY.MM.dd}
        elasticsearch {
            hosts => ["localhost:9200"]
            index => "logstash-api-info-%{+YYYY.MM.dd}"
            manage_template => true
        }
    }
}
```

5、创建服务

```
$ cd /usr/lib/systemd/system
$ touch logstash.service
$ vi logstash.service
[Unit]
Description=Logstash Service
After=network.target
[Service]
Type=simple
ExecStart=/home/elk/logstash-6.3.1/bin/logstash -f /home/elk/logstash-6.3.1/config/logstash.conf
Restart=on-failure
[Install]
WantedBy=default.target
```

设置开机自启动

```
$ systemctl enable logstash.service
$ systemctl start logstash.service
```

### 安装Filebeta
1、选择下载合适的编译版本

```
wget https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.2-linux-x86_64.tar.gz
```

2、解压文件

```
tar xzvf filebeat-7.4.2-linux-x86_64.tar.gz
```

3、查看目录结构

```
LICENSE.txt
NOTICE.txt
README.md
data
fields.yml
filebeat
filebeat.reference.yml
filebeat.yml
kibana
module
modules.d
```

4、修改配置文件

```
$ vi filebeat.yml

filebeat.inputs:
- type: log
  paths:
    - /var/log/nginx/access.log
  fields:
    service: "web"  # 自定义字段，区分类型
output.logstash:
  hosts: ["192.168.31.29:5044"]  #logstash地址
```

5、创建服务

```
$ cd /usr/lib/systemd/system
$ touch filebeat.service
$ vi filebeat.service
Description=Filebeat Service
After=network.target
[Service]
Type=simple
ExecStart=/home/filebeat-7.4.2-linux-x86_64/filebeat -e -c /home/filebeat-7.4.2-linux-x86_64/filebeat.yml
Restart=on-failure
[Install]
WantedBy=default.target
```

设置开机自启动
```
$ systemctl enable filebeat.service
$ systemctl start filebeat.service
```

其他服务器上的filebeat的配置类似
