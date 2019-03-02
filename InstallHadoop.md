
To install Hadoop in ubuntu:

Option 1:
1. use cloudera cloudera-quickstart-vm-5.13.0-0-virtualbox
2. then click on  cloudera-quickstart-vm-5.13.0-0-virtualbox-disk1.vmdk.
3. you will see that all the hadoop components are automatically installed.

Option 2:

Install Manually:
1. install jdk 
  ```sudo apt-get install default-jdk```
2. install ssh
  ```sudo apt-get install ssh```
  generate the key : ```ssh-keygen -t rsa -P ""```
  add the keys into autherized_keys ```cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys```
3. get the gz file
```wget http://apache.mirrors.tds.net/hadoop/common/hadoop-2.7.3/hadoop-2.7.3.tar.gz```
4. untar the file
``` tar -zxvf hadoop-2.7.3.tar.gz```
5. in the hadoop-env.sh file set the java home like 
``` export JAVA_HOME=$(readlink -f /usr/bin/java | sed "s:bin/java::")```
6. add below lines into core-site.xml
```
<configuration>
 <property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
 </property>

 <property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:54310</value>
  <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
 </property>
</configuration>
```
7. add below lines into mapred-site.xml
```
<configuration>
 <property>
  <name>mapred.job.tracker</name>
  <value>localhost:54311</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
 </property>
</configuration>
```
8. add below in hdfs-site.xml
```
<configuration>
 <property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
 </property>
 <property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
 </property>
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
 </property>
</configuration>
```
9. go to bin folder and run hadoop namenode -format
10. start hadoop sbin/startall.sh
11. run jps which should display all datanodes, namenodes etc
```
9026 NodeManager
7348 NameNode
9766 Jps
8887 ResourceManager
7507 DataNode
```

12. Hadoop Web Interfaces
Let's start the Hadoop again and see its Web UI:
shamim@laptop:/usr/local/hadoop/sbin$ start-all.sh

http://localhost:50070/ - web UI of the NameNode daemon
