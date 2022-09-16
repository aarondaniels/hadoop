# What Is MapReduce?

MapReduce is a core component of the Hadoop framework. The main functionality of MapReduce is to split large amounts of data into smaller chunks for efficient manipulation. In more technical terms, MapReduce can split a petabyte (1,000 TB) of data into smaller chunks and distribute the smaller chunks onto multiple servers for parallel processing. Later, the results are collected in one place and integrated to form the final dataset.

For example, suppose you have 10,000 servers and that each one of them can process 256 MB of data at a time. If you are working with 5 TB of data, you could use MapReduce to distribute your data among your servers and process all of the data simultaneously in a fraction of the time.

# How Does MapReduce Work?

The MapReduce algorithm contains two main functions: Map and Reduce.

- The Map function takes a dataset and converts it into another dataset, where individual elements are broken down into key-value pairs.
During this phase, the input data is split into smaller blocks, and each of them is assigned to a mapper for processing. For example, if you have 500 records to be processed, 500 mappers can run at the same time to process one of them at a time; a mapper may also be able to process two records at the same time. The number of mappers is decided by Hadoop depending on data and the memory capability of the server.

- The Reduce task takes the output from the Map as an input. It shuffles and sorts the data key-value pairs into a smaller set of tuples.
Naturally, the Reduce task is always performed after the mapping is done.

Between the ***Map*** and the ***Reduce*** functions, there are two intermediate ones: the Combine and Partition functions:

- The ***Combine*** function reduces the data on each mapper to a more simplified form. This step is optional, as it depends on the complexity of the data.
- The ***Partition*** function, translates the key-value pairs from the mapper to a different set of key-value pairs before feeding it to the reducer. This step is important, as it decides how the data will be represented.

The diagram below gives you an idea of how the MapReduce pipeline works:

![MapReduce pipeline]()

# MapReduce Use Case

As a simple example of how MapReduce works, suppose your Hadoop framework is using three mappers: ***Mapper1***, ***Mapper2***, and ***Mapper3***. You will also use the data in the diagram above for this example.

The input value of each mapper is a record. The mapper processes this record to produce key-value pairs so the output of each mapper will be:

Mapper1 -> <Student A, 1>,<Student B, 1>,<Student C,1>

Mapper2 -> <Student B, 1>,<Student B, 1>,<Student C, 1>

Mapper3 -> <Student A, 1>,<Student C, 1>,<Student B, 1>

You will further assume that you have one combiner assigned to each mapper: Combiner1, Combiner2, and Combiner3. Each of these combiners will count each student in each mapper. So, the output of the combiners will be:

Combiner1 -> <Student A, 1>,<Student B, 1>,<Student C,1>

Combiner2 -> <Student B, 2>,<Student C, 1>

Combiner3 -> <Student A, 1>,<Student C, 1>,<Student B, 1>

After this phase, the partitioner allocates and sorts the data for the reducers. The output of each partitioner will be:

Partitioner1 -> <Student A>{1,1}

Partitioner2 -> <Student B>{1,2,1}

Partitioner3 -> <Student C>{1,1,1}

If you chose not to use combiners, the output would instead have been:

Partitioner1 -> <Student A>{1,1}

Partitioner2 -> <Student B>{1,1,1,1}

Partitioner3 -> <Student C>{1,1,1}

Finally, the reducer can calculate the count for each student, giving the following output:

Reducer1 -> <Student A>{2}

Reducer2 -> <Student B>{4}

Reducer3 -> <Student C>{3}
