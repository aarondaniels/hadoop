# Writing a Java Program to Access the Hadoop Database

This exercise will establish data storage in Hadoop using data that contains information about product sales transactions. The first part of the exercise will be to build a Hadoop Docker container and ingest data into Hadoop data storage. The second part of the exercise will be to explain what each java program does and then apply the MapReduce rramework to an input file to calculate aggregate sales by country. 

## Part 1: Ingesting data into the HDFS
1. The [test program file]() has been copied to this repository. It must be copied to the home directory of the Hadoop `namenode`. This is accomplished by navigating to the directory of the testprogram file and executing the following command:
``` 
docker cp testprogram/ namenode:/home
```
2. Open the namenode CLI in Docker (or execute `docker exec -it namenode /bin/bash` to access). Inside the `home` directory, use the following HDFS command to create an input folder called `inputMapReduce`
```
hdfs dfs -mkdir /inputMapReduce
```
Navigate to the home directory of the `testprogram` folder. Use the following command to perform an HDFS copy of the SalesData.csv file into the Hadoop data storage
```
hdfs dfs -copyFromLocal SalesData.csv /inputMapReduce
```
3. Review the copied file using the HDFS cat command:
```
hdfs dfs -cat /inputMapReduce/SalesData.csv
```
## Part 2: Performing MapReduce: Aggregation Sales by Country
This part of the exercise will use the code files under the `testprogram` folder on the `namenode` using the Docker CLI
1. Navigate and review the source code in the `testprogram` folder and describe the functions within each of the following files:
- `SalesCountryDriver.java` - This file is the Job Creator which controls the flow of data processing
- `SalesMapper.java` - This file is used for splitting records for distributed calculation. The input data is split into smaller blocks and each of them is assigned to a mapper for processing
- `SalesCountryReducer.java` - This file is used for processing chunks and combining output. The Reduce task takes the output from the Map as an input
2. In the `namenode` CLI, run the commands below to set up an executable path in the system envrionment:
```
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre/
export CLASSPATH="$HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-client-core-3.2.1.jar:$HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-client-common-3.2.1.jar:$HADOOP_HOME/share/hadoop/common/hadoop-common-3.2.1.jar:~/testprogram/SalesCountry/*:$HADOOP_HOME/lib/*"
export HDFS_NAMENODE_USER="root"
export HDFS_DATANODE_USER="root"
export HDFS_SECONDARYNAMENODE_USER="root"
export YARN_RESOURCEMANAGER_USER="root"
export YARN_NODEMANAGER_USER="root"
```
Once complete, run the following command to confirm that the environment variables are correctly defined:
```
$echo JAVA_HOME
```
3. Navigate to the /home/testprogram folder inthe namenode CLI and compile a Java program using the following command. This will create a SalesCountry folder with class file within it
```
javac -d . SalesMapper.java SalesCountryReducer.java SalesCountryDriver.java
```
4. From the testprogram folder, create a jar file from the Java code compiled in the previous step. To acheive this, run the following command: 
```
jar cfm ProductSalePerCountry.jar Manifest.txt SalesCountry/*.class
```
5. Perform a MapReduce operation using the following command: 
```
$HADOOP_HOME/bin/hadoop jar ProductSalePerCountry.jar /inputMapReduce /mapreduce_output_sales
```
6. Review the `SalesCountry` data output in the part-00000 file by running the following command: 
```
$HADOOP_HOME/bin/hdfs dfs -cat /mapreduce_output_sales/part-00000
```
