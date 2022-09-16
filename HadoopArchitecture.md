# Hadoop Architecture

Hadoop is a framework written in Java to maintain and store big data. Today, lots of big organizations, such as Facebook, Yahoo, Netflix, and eBay, are using Hadoop in their organizations to deal with big data.

The Hadoop architecture mainly consists of four components: MapReduce, Hadoop Distributed File System (HDFS), Yet Another Resource Framework (YARN), and Common Utilities (or Hadoop Common).

## Hadoop Distributed File System (HDFS)

The Hadoop Distributed File System (HDFS) is utilized for storage permission in a Hadoop cluster. It is mainly designed for working on inexpensive hardware devices. The HDFS is designed to store bigger chunks of data more efficiently than smaller ones.

There are two types of data storage nodes in the HDFS: NameNode and DataNode. The NameNode works as a master node in a Hadoop cluster by guiding the DataNodes, which are also defined as slave nodes. The main functionality of a NameNode is to store information about the data. For example, a NameNode could host the transaction log about a user’s activity in a Hadoop cluster.

DataNodes are mainly utilized for storing the data in a Hadoop cluster. The higher the number of DataNodes is, the more data the Hadoop cluster will be able to store. Therefore, it is advised that the DataNode should have a high storage capacity to store a large number of file blocks.

![High-level architecture of the HDFS]()

As mentioned earlier, data in the HDFS is always broken and stored in 128 MB chunks. Here is an example to help you to better understand the concept of breaking down a file into blocks. Suppose you have uploaded a file of 500 MB to your HDFS. This file gets divided into three blocks of 128 MB and one smaller block with the remaining 116 MB. Because Hadoop runs using inexpensive hardware, the HDFS automatically makes a copy of the file blocks stored in it for backup purposes. By default, the replication factor (i.e., the number of times each file is duplicated in Hadoop) is set to three.

## Yet Another Resource Negotiator (YARN)

YARN is a framework that MapReduce works with. YARN performs two operations: job scheduling and resource management. The purpose of the Job Scheduler is to divide a big task into small jobs so that each job can be assigned to various slave nodes in a Hadoop cluster to maximize processing times. The Job Scheduler also keeps track of job prioritization, which job has more dependencies, and job timing. The Resource Manager is used to manage all of the resources that are made available for running a Hadoop cluster.

The image below represents a simplified version of the YARN architecture:

![High-level architecture of YARN]()

The main component of YARN architecture is the Resource Manager. The Resource Manager is responsible for resource allocation, distributing parts of requests to corresponding node managers accordingly. It also manages the cluster resources and decides the allocation of the available resources for competing applications.

The second component, the Node Manager, monitors individual nodes in a Hadoop cluster and manages user jobs and workflow on the given node. It also works with the Resource Manager to ensure that the node is working correctly. Its primary goal is to manage application containers assigned to it by the Resource Manager.

The Application Master works on a single job submitted to the framework. Each Application Master coordinates an application’s execution in the cluster and also manages faults. Its task is to negotiate resources from the Resource Manager and work with the Node Manager to execute and monitor the component tasks.
Finally, a container is a collection of physical resources, such as RAM, CPU cores, and disks on a single node. It grants rights to an application to use a specific amount of resources (memory, CPU, etc.) on a specific host.

## Hadoop Common (or Common Utilities)

Hadoop Common (or Common Utilities) includes the Java library, Java files, and Java scripts that you need for all of the other components present in a Hadoop cluster to work together. These utilities are used by the HDFS, YARN, and MapReduce for running the cluster. In particular, it’s responsible for how the data is stored in the HDFS and how MapReduce should handle the data stored by the HDFS in chunks. It is also responsible for the communications between the MapReduce and YARN frameworks for job scheduling and resource management.