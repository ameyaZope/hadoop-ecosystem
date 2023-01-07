# Hadoop

# Setup Hadoop on Local
1. Download the hadoop binary from https://hadoop.apache.org/releases.html (3.2.3, 3.3.4)
2. Ensure that you have java8 installed and the JAVA_HOME variable set in your zshrc file
   1. Configurations
      1. etc/hadoop/core-site.xml
      ```xml
       <configuration>
           <property>
               <name>fs.defaultFS</name>
               <value>hdfs://localhost:9000</value>
           </property>
       </configuration>
      ```
      
      2. etc/hadoop/hdfs-site.xml
      ```xml
      <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
      </configuration>
      ```
      
      3. etc/hadoop/mapred-site.xml
      ```xml
      <configuration>
        <property>
            <name>mapreduce.framework.name</name>
            <value>yarn</value>
        </property>
        <property>
            <name>mapreduce.application.classpath</name>
            <value>$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/*:$HADOOP_MAPRED_HOME/share/hadoop/mapreduce/lib/*</value>
        </property>
      </configuration>
      ```
      4. etc/hadoop/yarn-site.xml
      ```xml
      <configuration>
        <property>
            <name>yarn.nodemanager.aux-services</name>
            <value>mapreduce_shuffle</value>
        </property>
        <property>
            <name>yarn.nodemanager.env-whitelist</name>
            <value>JAVA_HOME,HADOOP_COMMON_HOME,HADOOP_HDFS_HOME,HADOOP_CONF_DIR,CLASSPATH_PREPEND_DISTCACHE,HADOOP_YARN_HOME,HADOOP_HOME,PATH,LANG,TZ,HADOOP_MAPRED_HOME</value>
        </property>
      </configuration>
      ```
3. Set up password less ssh
```shell
    cd ~/.ssh
    ssh-keygen (when you run this command, put files in default location by hitting enter and use default no password)
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys
```
4. Format namenode 
```shell
    bin/hdfs namenode -format
```
5. Extract the earlier download binary in step 1
6. Set up environment variables
```shell
    export HADOOP_HOME={{path where you extracted the binary, HADOOP_HOME/bin will be the bin path}}
    export HADOOP_CONF_DIR=$HADOOP_HOME/etc/hadoop
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export YARN_HOME=$HADOOP_HOME 
```
7. Start Hadoop on local
```shell
    $HADOOP_HOME/sbin/start_all.sh 
```
8. View hadoop UI at http://localhost:9870
9. View yarn UI at http://localhost:8088