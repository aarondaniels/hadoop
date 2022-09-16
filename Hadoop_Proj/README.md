# Project Details
This project will use Hadoop to compute word frequency in a text. 

Create an input file for Hadoop to perform the word frequency count
1. access the `namenode` container by executing `docker exec -it namenode /bin/bash` in terminal
2. Within the home directory, create directory for input, `mkdir input`
3. From the bash command prompt, run the following commands to create two text files in the input folder that was just created, `echo "it was the best of times it was the worst of times" > ./input/f1.txt` and `echo "it was the age of wisdom it was the age of foolishness" > ./input/f2.txt`. Confirm the files in the input folder, `ls -l ./input`
4. In order to allow Hadoop to access the text files to perform the word count, copy the files from teh containers file system to the HDFS. From the bash command prompt, run the following command to create a folder called `input` in the HDFS, `hadoop fs -mkdir -p input`
5. Copy the two text files create to the HDFS. From the bash command prompt, run the following command to copy the files from the input folder to the HDFS, `hdfs dfs -put ./input/* input`
6. Download word count program inside the Hadoop container. In this case, a program (in jar format) will be downloaded via CURL command. Access the namenode container within the bash terminal and run the following command to download the jar file, `curl -L https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-examples/2.7.1/hadoop-mapreduce-examples-2.7.1-sources.jar --output hadoop-mapreduce-examples-2.7.1-sources.jar`. Confirm the .jar file is in the container. 
7. Run word count program on the Hadoop platform (within the namenode container) with the following command, `hadoop jar hadoop-mapreduce-examples-2.7.1-sources.jar org.apache.hadoop.examples.WordCount input output`
8. Confirm output, `hdfs dfs -cat output/part-r-00000`

