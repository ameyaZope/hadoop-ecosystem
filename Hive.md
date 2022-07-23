# Hive

# Setup Hive on Local
1. Download the hive binary from https://hive.apache.org (3.1.2, 3.1.3, 4.0.0-1 aplha)
2. Extract the binary 
3. Remove any previous metastore_db folder in the extracted part
4. Run the schema tool by
```
./bin/schematool -dbType derby -initSchema
```
5. Copy the hive-default.xml.template to hive-default.xml in the same conf folder
6. Create a hive-site.xml file with the following contents
```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
    <property>
        <name>hive.server2.enable.doAs</name>
        <value>false</value>
        <description>
            Setting this property to true will have HiveServer2 
            execute Hive operations as the user making the calls to it
        </description>
    </property>
    
</configuration>

```
7. Now you can run the hive server by the following command
```
./bin/hiveserver2
```
8. Wait till you see Hive Session Id 4 times in logs and then check https://localhost:10002 for web ui of hive to check if hive has started up