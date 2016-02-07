# etc.hadoop
etc.hadoop configuration files and installation notes for pseudo distributed multiple user setup

```
References:
http://tecadmin.net/setup-hadoop-2-4-single-node-cluster-on-linux/#
http://www.michael-noll.com/tutorials/running-hadoop-on-ubuntu-linux-single-node-cluster/
http://whiteboxdeveloper.blogspot.com/2014/04/creating-multiple-users-in-hadoop.html
http://pl.postech.ac.kr/~maidinh/blog/?p=170
```

```
## hadoop status
overview:  http://localhost:50070/
cluster and application:  http://localhost:8088/ 
secondary node:  http://localhost:50090/
data node:  http://localhost:50075/
```

```
## system info
system user: hadoop hadoop:hadoop
installation directory : /home/hadoop/hadoop
tmp directory : /home/hadoop/hadooptmpdir
chmod 777 /home/hadoop/hadooptmpdir


## .bashrc for system user hadoop
# hadoop
export HADOOP_HOME=/home/hadoop/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
# export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
# export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native"
export PATH=.:$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin


## system user user1
system user: user1 user1:wheel
export PATH=.:$HOME/bin:/home/hadoop/hadoop/bin:$PATH
hadoop fs -mkdir /user/user1
hadoop fs -chown -R user1:wheel /user/user1
hadoop fs -ls -R /user
```

```
#### compile hadoop to get rid of cannot load native library issue####
http://pl.postech.ac.kr/~maidinh/blog/?p=170
For CentOS
Install Maven
# cd /home/maidinh/maven
# wget http://mirror.apache-kr.org/maven/maven-3/3.2.5/binaries/apache-maven-3.2.5-bin.tar.gz
# tar xvf apache-maven-3.2.5-bin.tar.gz
Update .bashrc
export JAVA_HOME=/usr/java/latest
unset M2
unset M2_HOME
export PATH=/home/maidinh/maven/apache-maven-3.2.5/bin:$PATH
# source ~/.bashrc
# mvn -version
Install ProtocolBuffer 2.5.0
# wget https://protobuf.googlecode.com/files/protobuf-2.5.0.tar.gz
# tar xvf protobuf-2.5.0.tar.gz
# cd protobuf-2.5.0
# ./configure
# make
# sudo make install
# protoc --version
Install cmake, zlib, openssl, snappy
# sudo yum install cmake zlib-devel openssl-devel snappy-devel
Compile Hadoop
# tar xvf hadoop-2.6.0-src.tar.gz
# cd hadoop-2.6.0-src
# mvn package -Pdist,native -DskipTests -Dtar -Dmaven.javadoc.skip=true -Drequire.snappy -Drequire.openssl
Check results in hadoop-dist/target/
```
