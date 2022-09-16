# Hadoop

Hadoop is an open source software platform for distributed stoarge and distributed processing for big data. For better context of the application for Hadoop, it is worthwhile to consider what [Big Data]() is. Big Data can succintly be defined as data that has high velocity, volume, and variety. In short, Hadoop was designed with this in mind as precedent system's were not equiped to deal with these challenges. 
Hadoop democratized computing power and made possible to analyze and query big data, open-source software and inexpensive, off-the-shelf hardware. It emerged to store and process huge data; increase computing power, fault tolerance, and flexibility in data management; and lower costs compared to data warehouses. 

# How to Set up Hadoop

## Create Hadoop Docker Image
This docker image is provided by the [Big Data Europe](https://github.com/big-data-europe/docker-hadoop) project. For the sake of this project, I have cloned a copy of the linked repo within this repo. It is recommended that a refreshed clone be used if this project is being repeated. 

## Create Hadoop Docker container
With the Docker image downloaded, move into the directory where it is stored and enter `docker-compose up` from the terminal. If successful, the Docker will appear in the Docker dashboard. It should also show 7 nodes:
- 1 HDFS namenode (or primary node to manage the secondary)
- 3 datanodes (secondary nodes)
- 1 YARN resourcemanager
- 1 historyserver
- 1 nodemanager

## Check Hadoop status using a web browser
With the containers running, navigate to `http://localhost:9870/`. You should see a summary of the system and number of nodes. The ribbon on the top 

## Simple Application
For a rudimentary application of HDFS, these steps outline the execution of a wordcount program against data (text) within the HDFS:

Create an input file for Hadoop to perform the word frequency count
1. access the `namenode` container by executing `docker exec -it namenode /bin/bash` in terminal
2. Within the home directory, create directory for input, `mkdir input`
3. From the bash command prompt, run the following commands to create two text files in the input folder that was just created, `echo "it was the best of times it was the worst of times" > ./input/f1.txt` and `echo "it was the age of wisdom it was the age of foolishness" > ./input/f2.txt`. Confirm the files in the input folder, `ls -l ./input`
4. In order to allow Hadoop to access the text files to perform the word count, copy the files from teh containers file system to the HDFS. From the bash command prompt, run the following command to create a folder called `input` in the HDFS, `hadoop fs -mkdir -p input`
5. Copy the two text files create to the HDFS. From the bash command prompt, run the following command to copy the files from the input folder to the HDFS, `hdfs dfs -put ./input/* input`
6. Download word count program inside the Hadoop container. In this case, a program (in jar format) will be downloaded via CURL command. Access the namenode container within the bash terminal and run the following command to download the jar file, `curl -L https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/hadoop-mapreduce-examples-2.7.1-sources.jar --output hadoop-mapreduce-examples-2.7.1-sources.jar`. Confirm the .jar file is in the container. 
7. Run word count program on the Hadoop platform (within the namenode container) with the following command, `hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input output`
8. Confirm output, `hdfs dfs -cat output/part-r-00000`