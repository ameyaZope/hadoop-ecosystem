# Data Warehouse : Hortonworks Data Platform (HDP)

This document is incomplete. 

This document covers my experience with hortonworks data platform. 

-------------

## Hadoop
In easy words, Hadoop is a distributed file system that allows any sort of computation over files stored in the distributed file system. Hadoop consists of 2 major things
1. The Distributed File System (also called HDFS or Hadoop Distributed File System) - Stores files and provides read , writes, update and delete capabilities on those files, just like any file system. The difference between this file system and the normal unix file system is that HDFS auto manages data replication and splitting up of large files across commodity hardware. A user need only specify replication factor, the HDFS will ensure that those many copies are available on the entire cluster of servers in a fault tollerant manner. The ability of HDFS to store large files over multiple small commodity hardware paves way for horizontal scaling instead of vertical scaling. Before hadoop if you had a file of size 50TB and and you have 10 servers of 10 TB data storeage each, you would have to buy new vertically larger server to store the 50TB file. With a distributed file system, you can now store that 50TB file over the already availble 10 servers of 10 TB each. It is due to this distributed nature and inbuilt replication that this file system is more reliable than the traditional unix file system. 

2. Implementation of MapReduce algorithm (this implementation is co-incidently also called MapReduce) - It is because of this implementation that we can simply provide the function that needs to be applied on data stored in the HDFS. Simply put it abstracts away the painful complexity of applying functions on the distributed nature of the data in HDFS.


## What is HDP ?
The Hortonworks Data Platform (HDP) is a security-rich, enterprise-ready, open source Apache Hadoop distribution based on a centralized architecture (YARN). Simply put HDP allows you to use Hadoop on Production. In other terms HDP is a production ready data warehouse that uses hadoop as its base layer.

So basically HDP open source frameworks allows a person to set up his own data warehouse. 

A data warehouse typically gurarantees the following 

1. Data will never be lost :  Achieved via Redundancy. (keeping copies of the same data over different hardwares)
2. Should allow to run Analytics queries on the data. 
3. Multi Tenancy (Good to have) : Different teams should be able to access backed up data , run queries on that backed up data via commodity shared hardware. There should be fair distribution among shared resources.
4. Data Governance and Security 


A data warehouse consists of the following parts
### Storage Layer : Apache Hadoop(HDFS) or Apache Kudu
Hadoop distributed File System on the surface is just like any other file system. You have a file browser, you can copy-paste files, move files and create new files. HDFS replicates the data uploaded to it for redundancy. Essentially if you have a 10TB file that you want to store and you have 50 1TB hard disks, you can use HDFS to store that file as it will split up the file in appropriate sizes and distributes the files on teh 50 hard disks. 



### Computation Layer : MRv1, MRv2, spark, tez
You can run jobs map reduce jobs via
1. Traditional MapReduce (V1 is outdated and new versions only support V2)
2. Spark Jobs
3. Tez Jobs

Before Going forward, remember that MapReduce is way of executing functions over some data. This way of splitting into Map and Reducing functions was provided by folks over at google. They also provided an open source implementation of the same and called it MapReduce. As is with every piece of software developed, over time it was realised that there could be some optimisations to the implemntation keeping the algorithm same. Hence new falvours of the MapReduce algorithm(spark, tez) were published in order to optimise the implementation of the very same algorithm

### Map Reduce Algorithm
Any function/algorithm/job can be split inot two parts
1. Mapping 
2. Reducing
The input is provided to the mapping part and it generates an intermediary output. This output is then provided to the reducing part and the reducing part generates the final output. Look at the very popular word count example to understand more. 


#### Traditional MapReduce Jobs Impementation
Any job performed via this method can be split into two parts, Mapping part and then the Reducing part. 
1. The data is read from the disk at the start of the mapping part.  
2. THe mapper function is applied on the data and some output is generated which is again written down to disk
3. The data is read at the start of the reducing part. The reducer funtion is then applied to the intermediate data and the final output is written down to disk.

Note that the above implementation of MapReduce is shipped with the hadoop ecosystem itself. 

You can create map reduce jobs in Java, Python 

Executing Python MR Jobs via Hadoop Implementation

Hadoop Streaming is a utility that comes with the Hadoop distribution. It can be used to execute programs for big data analysis. Hadoop streaming can be performed using languages like Python, Java, PHP, Scala, Perl, UNIX, and many more. The utility allows us to create and run Map/Reduce jobs with any executable or script as the mapper and/or the reducer. For example:

```

$HADOOP_HOME/bin/hadoop  jar $HADOOP_HOME/hadoop-streaming.jar

-input myInputDirs

-output myOutputDir

-mapper /bin/cat

-reducer /bin/wc
```



As you can clearly see there are allot of disk reads. Infact, this type of MR jobs will involve the maximum reads and writes to disk and hence is the slowest of the three types of jobs available


#### Spark Jobs
Any Job performed via this way although uses the same Mapping and Reducing steps , it does not write the output of the mapper funtion to dsik. Instead, it keeps that data in memory which serves to reduce the number of disk io operations. Hence these types of jobs are generally faster than the hadoop jobs. 

#### Tez Jobs



### Querying Structured Data : Apache Hive, ApacheImpala

#### Apache Hive

Apache is a SQL data warehouse. It allows you to store and query massive amounts of data. Think of Hive like a SQL database, just that this database is not meant for serving live queries. Hive uses HDFS as underlying file system to store data. 



### Additional Tools to make Life Easy : Cloudera Hue, Apache Zeppelin, Sqoop

#### Apache Zeppelin
Apache Zeppelin is a user friendly UI for quering databases/storing and running any code (queries, python code, etc). This provides 

#### Sqoop (Deprecated)
Sqoop stands for 'SQL to HadOOP and vice versa'. As the name suggests, this tool allows the user to transfer datat between SQL and hadoop environemnt(hadoop, hive, etc)


### Data Governance and Security for Administration Authentication Authorization Auditing Data Protection : Apache Ranger, Apache Knox, Apache Atlas, HDFS Encryption

#### Apache Ranger
Ranger provides intricate access control over any data stored in hadoop.

#### Apache Knox
Apache Knox is a gateway server to the HDP ecosystem. 



### Scheduling : Apache Oozie

Oozie is a workflow scheduling system which runs hand-in-hand with the HDP ecosystem.



### Operations : Apache Ambari, Cloudbreak

#### Apache Ambari
Ambari essentially provides view of the cluster from hardware/resource standpoint. Basically it provides health status, metrics of each server within the cluster. It will provide health status of the hive cluster , total number of nodes in that cluster , total cpu utilisation, etc. 

Ambari also allows you to quickly set up an HDP ecosystem without going through the hassle of multitude of configs and errors that once must face while setting up HDP ecosystem.


