## Hadoop-istallation-guide
# step by step guide to install hadoop on vertualBox linux os

before starting upgrade ubuntu
>> sudo get-apt upgrade

Install java jdk 8

sudo apt install openjdk-8-jdk

To check it’s there cd /usr/lib/jvm

now go to hadoop.apache.org website download the tar file
(hadoop.apache.org — download tar file of hadoop.)
download Hadoop: https://www.apache.org/dyn/closer.cgi/hadoop/common/hadoop-3.3.4/hadoop-3.3.4.tar.gz

open .bashrc file and paste these commands
>> sudo nano .bashrc
(then paste all export mention below in .bashrc file at the bottom)
[ NOTE: Dont forget to change Hadoop version or java version in case you have used different from hadoop-3.3.4 or java-8-openjdk-amd64 ]

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
export PATH=$PATH:/usr/lib/jvm/java-8-openjdk-amd64/bin 
export HADOOP_HOME=~/hadoop-3.3.4/ 
export PATH=$PATH:$HADOOP_HOME/bin 
export PATH=$PATH:$HADOOP_HOME/sbin 
export HADOOP_MAPRED_HOME=$HADOOP_HOME 
export YARN_HOME=$HADOOP_HOME 
export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop 
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native 
export HADOOP_OPTS="-Djava.library.path=$HADOOP_HOME/lib/native" 
export HADOOP_STREAMING=$HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-3.3.4.jar
export HADOOP_LOG_DIR=$HADOOP_HOME/logs 
export PDSH_RCMD_TYPE=ssh

( ssh — secure shell — protocol used to securely connect to remote server/system — transfers data in encrypted form)

>> sudo apt-get install ssh



>> tar -zxvf ~/Downloads/hadoop-3.3.4.tar.gz 
(Extract the tar file)


>> cd hadoop-3.3.4/etc/hadoop

now open hadoop-env.h
>> sudo nano hadoop-env.hJAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 
(set the path for JAVA_HOME)

### core-site.xml
>> pintu@pintu:~$ cd hadoop-3.3.4/etc/hadoop/ && sudo nano core-site.xml
>> (copy paste below configuration at the bottom)
<configuration> 
     <property> 
     <name>fs.defaultFS</name> 
     <value>hdfs://localhost:9000</value>  </property> 
     <property> 
     <name>hadoop.proxyuser.dataflair.groups</name> <value>*</value> 
     </property> 
     <property> 
     <name>hadoop.proxyuser.dataflair.hosts</name> <value>*</value> 
     </property> 
     <property> 
     <name>hadoop.proxyuser.server.hosts</name> <value>*</value> 
     </property> 
     <property> 
     <name>hadoop.proxyuser.server.groups</name> <value>*</value> 
     </property> 
</configuration>

### hdfs-site.xml
>> pintu@pintu:~$ cd hadoop-3.3.4/etc/hadoop/ && sudo hdfs-site.xml
>> (copy paste below configuration at the bottom)
<configuration> 
    <property> 
    <name>dfs.replication</name> 
    <value>1</value> 
    </property> 
</configuration>

### mapred-site.xml
>> pintu@pintu:~$ cd hadoop-3.3.4/etc/hadoop/ && sudo mapred-site.xml
>> (copy paste below configuration at the bottom)
<configuration> 
     <property> 
     <name>mapreduce.framework.name</name>  <value>yarn</value> 
     </property> 
     <property>
     <name>mapreduce.application.classpath</name> 

    <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value> 
     </property> 
</configuration>

### yarn-site.xml
>> pintu@pintu:~$ cd hadoop-3.3.4/etc/hadoop/ && sudo yarn-site.xml
>> (copy paste below configuration at the bottom)

<configuration> 
    <property> 
    <name>yarn.nodemanager.aux-services</name> 
    <value>mapreduce_shuffle</value> 
    </property> 
    <property> 
    <name>yarn.nodemanager.env-whitelist</name> 

   <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREP END_DISTCACHE,HADOOP_YARN_HOME,HADOOP_MAPRED_HOME</value> 
    </property> 
</configuration>

ssh(setting passwordless ssh)

>> ssh localhost 
>> ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa 
>> cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys 
>> chmod 0600 ~/.ssh/authorized_keys 
>> hadoop-3.3.4/bin/hdfs namenode -format

format the file system

>> export PDSH_RCMD_TYPE=ssh

To start

>> start-all.sh(Start NameNode daemon and DataNode daemon) 
localhost:9870
>> pintu@pintu:~$ hadoop fs -mkdir /user
>> pintu@pintu:~$ hadoop fs -mkdir /user/pintu
>> pintu@pintu:~$ touch demo.csv
>> pintu@pintu:~$ hadoop fs -put demo.csv /user/pintu
>> stop-all.sh
