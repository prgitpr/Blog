# Flink compile
1.官网有介绍 [https://ci.apache.org/projects/flink/flink-docs-stable/flinkDev/building.html](https://ci.apache.org/projects/flink/flink-docs-stable/flinkDev/building.html)<br>
2.踩了一个坑,在编译过程中  Maven Error: Too many files ... <br>
  在mvn命令中加上-Drat.skip=true进行编译即可<br>
3.完整编译命令：mvn clean install -DskipTests -Drat.skip=true -Dcheckstyle.skip <br>
mvn clean install -DskipTests -Drat.skip=true -Dcheckstyle.skip  -Dfast -T 4 -Dmaven.compile.fork=true
提速版本的提交命令

4.Java的版本要是8版本，不然编译成功后，程序也运行不了<br>
5.编译期间 因为编译过程中出现 包下载失败 导致的编译失败，这种多试试<br>
6.第1次编译 花了11 小时，有点恐怖。。之后差不多 1小时 版本 Flink 1.10.1

7.C:\Users\admin\AppData\Local\Temp
