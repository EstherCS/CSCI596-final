# CSCI596-final
## Goals
When you want to apply for the job on the websites, sometimes you can't understand which abilities are most important in this position. So our team wants to solve this problem by scanning the job description, then use the MapReduce concept to count the words, finally output the three most frequent words in that job description.

## Structure

**String** ItemId <br>
**String** name <br>
**String** address <br>
**Set&lt;String>** keywords <br>
**String** url <br>
**String** imageUrl <br>

## TF-IDF algorithm

When calculating keywords, there are some words which has a high frequency in all documents but are not so meaningful. Such as 'a', 'the', 'is', 'are' etc. To avoid these words been selected as keywords, we apply TF-IDF algorithm</br>
To put it simply, **TF** is the frequency of the words in a document and **IDF** is the importance of the word in the document. If some words appear in a lot of different documents, we could expect a low importance of the words and have a low value for IDF.

- **Final score = TF * IDF**
1. *TF-Term frequency*
2. *IDF-Inverse document frequency*
- **TF (w) = number of w in document / number of all words in document**
- **IDF (w) = log (number of all documents / number of documents have w + 1)**</br>
 *The reason why we add 1 is to avoid the case when w never exists in any doc.*

## First approach (MapReduce)
**Steps**

 1. Use GitHub job API ([https://jobs.github.com/api](https://jobs.github.com/api)) to fetch job information as our data source which matches some specific keywords. 
ex: If keyword is "developer", then we could get a list of "developer" jobs.
 2. Dropped unnecessary attributes from the response by Step 1.<br/>
 *Raw data from the API:*<br/>
 ![enter image description here](https://i.imgur.com/jVskXMq.png)
 3. Extract keywords of the job by using MapReduce.<br/>
 *We count the frequency of each words and the top 3 frequent words are considered as "keywords" in our case.*<br/>
  ![enter image description here](https://s3.amazonaws.com/files.dezyre.com/images/Tutorials/MapReduce_Example.jpg)
**Steps of MapReduce**
- **Splitting step** - We first split sentences into words
- **Mapping step** - Each words is identified and mapped with number 1
- **Shuffling step** - A partitioner takes actions to do "shuffling" so that tuples with the same key are sent to the same node
- **Reducing step** - The Reducer node processes all the tuples to count each specific keys and set its frequency to the value of the tuple.
4. Run the server and use a get request to retrieve the data. Then we could go through above steps and produce keywords and also print some other important job attributes as the final output.

## Second approach (PySpark)
**What is Spark?**

-   It’s based on Hadoop MapReduce, but what makes it different from Hadoop is that Hadoop reads and writes files to HDFS (Hadoop Distributed File System), Spark processes data in RAM using a concept called RDD (Resilient Distributed Dataset). Therefore, the processing is faster than Hadoop. Spark also supports more types of computations such as interactive queries or stream processing.
    
-   RDD is the fundamental data structure of Spark. Each dataset in RDD is divided into partitions and will be computed on the cluster of nodes. RDD is a read-only, partitioned collection of records. There are two ways to create a RDD:
    

  1.  Parallelizing an existing collection in your driver program which is what we do in this project.
    
  2.  Referencing a dataset in an external storage system, such as a shared file system, HDFS, or any data source that has a Hadoop Input Format.

![enter image description here](https://miro.medium.com/max/1050/1*l2MUHFvWfcdiUbh7Y-fM5Q.png)
In this approach, we replace MapReduce with the use of RDD.
Compared to the first approach, we change the following intermediate steps of MapReduce to RDD: 
-  Using parallelize in SparkContext to convert the list of words into RDD
    
-  Mapping the RDD to set up the parallel computing over clusters
    
-  Reduce the RDD data using the word key
    
- Collect the RDD: our computation is not performed until our result is collected

## Results
![enter image description here](https://i.imgur.com/3WRAa0W.png)</br>
![enter image description here](https://i.imgur.com/jbmQEYc.jpg)</br>
![enter image description here](https://i.imgur.com/W5Alx69.jpg)</br>

## Contribution:
Shih-Yu Lai:
USC-ID: 7183984563
Discuss project topic with others and implement first approach.

Shiuan-Chin Huang:
USC-ID: 9815633745
Discuss project topic with others and implement second approach.

Yu-Chieh Cheng:
USC-ID: 5261294599
Discuss project topic with others and write the report.
