# Redis


   10  sudo yum update
   11  sudo yum install epel-release yum-utils
   12  sudo yum install http://rpms.remirepo.net/enterprise/remi-release-7.rpm
   13  sudo yum-config-manager --enable remi
   14  sudo yum install redis
   15  sudo systemctl enable redis
   16  ps -ef|grep redis
   17  sudo yum install redis
   18  sudo amazon-linux-extras install redis4.0
   19  sudo systemctl enable redis
   20  sudo systemctl start redis