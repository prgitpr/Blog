wget  wget  https://cdn.mysql.com//Downloads/MySQL-8.0/mysql-8.0.22-1.el7.x86_64.rpm-bundle.tar

#httpd
systemctl start httpd
sed -i 's/AddType application\/x-gzip .gz .tgz/AddType application\/x-gzip .gz .tgz .parcel/g' httpd.conf
systemctl status httpd.service
netstat -an |grep 80

#jdk
export JAVA_HOME=/usr/java/jdk1.8.0_181-amd64
export PATH=$JAVA_HOME/bin:$PATH

#离线下载
wget https://archive.cloudera.com/cdh6/6.2.1/parcels/manifest.json
wget https://archive.cloudera.com/cdh6/6.2.1/parcels/CDH-6.2.1-1.cdh6.2.1.p0.1425774-el7.parcel.sha256
wget https://archive.cloudera.com/cdh6/6.2.1/parcels/CDH-6.2.1-1.cdh6.2.1.p0.1425774-el7.parcel.sha1

#查看时区
timedatectl |grep "Time zone"

wget https://archive.cloudera.com/cm7/6.2.1/repo-as-tarball/cm6.2.1-redhat7.tar.gz
# 下载openjdk
yum install java-1.8.0-openjdk
# 下载 flink
wget https://mirrors.tuna.tsinghua.edu.cn/apache/flink/flink-1.12.0/flink-1.12.0-bin-scala_2.12.tgz

#mysql
CREATE DATABASE scm DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON scm.* TO 'scm'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE amon DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON amon.* TO 'amon'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE rman DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON rman.* TO 'rman'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE hue DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON hue.* TO 'hue'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE metastore DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON metastore.* TO 'metastore'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE nav DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON nav.* TO 'nav'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE sentry DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON sentry.* TO 'sentry'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE navms DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON navms.* TO 'navms'@'%' IDENTIFIED BY 'sbsbsb';
CREATE DATABASE oozie DEFAULT CHARACTER SET utf8 DEFAULT COLLATE utf8_general_ci;
GRANT ALL ON oozie.* TO 'oozie'@'%' IDENTIFIED BY 'sbsbsb';
