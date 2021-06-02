#命令
1.yum install -y java-1.8.0-openjdk-devel.x86_64 不需要设置环境变量
orcle jdk 安装
2. 环境变量
 - export JAVA_HOME
 - export $PATH=$JAVA_HOME:$PATH
 - source ~/.bashrc 
3. 安装谷歌浏览器
- wget https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm
- yum -y install redhat-lsb libXScrnSaver
- yum -y localinstall google-chrome-stable_current_x86_64.rpm

$? 执行有没有问题，成功=0， 其它 ！=0 
搞定了orcle 的安装问题
https://www.oracle.com/java/technologies/oracle-java-archive-downloads.html
rpm -qa |grep 查看 安装了哪些
rpm -e 删除一些软件

uname -a
alias 别名





