## Introduction

Apache Hadoop is a collection of open-source software utilities that
facilitates using a network of many computers to solve problems
involving massive amounts of data and computation. It provides a
software framework for distributed storage and processing of big data
using the MapReduce programming model.[^1]

## Commands

### Hadoop File System (HDFS) {#hadoop_file_system_hdfs}

``` bash
# List user directory
hdfs dfs -ls /user

# Make a directory
hdfs dfs -mkdir /user/peter

# Output content in shell
hdfs dfs -cat /user/filename

# Insert/Inject data into the HDFS
hdfs dfs -put /source/of/your/data/ /directory/destination
## hdfs dfs -put /home/peter/hadoop_files/data/ /user/peter
```

### Use Hadoop {#use_hadoop}

#### Map Reduce with .jar files {#map_reduce_with_.jar_files}

``` bash
# Use function jar to run Map Reduce code
hadoop jar /source/of/jar/file -mapper /source/of/mapper - reducer /source/of/reducer -input /source/in/HDFS -output /destination/in/HDFS

# Analyze the output
## List Files in HDFS
hdfs dfs -ls /destination/in/HDFS
## Output content in shell
hdfs dfs -cat /destination/in/HDFS/filename
```

``` bash
## Examples
hadoop jar /home/peter/hadoop_files/hadoop-streaming-3.3.0.jar -mapper /bin/cat -reducer /usr/bin/wc -input /user/peter/data/* -output /user/peter/dataOut

hadoop jar /home/peter/hadoop_files/hadoop-streaming-3.3.0.jar -mapper /tmp/hadoop_scripte/mapper.py -reducer /tmp/hadoop_scripte/reducer.py -input /user/peter/data/* -output /user/peter/dataOut2

## -output option: file mustn't exist otherwise the result is not correct!
## -mapper & -reducer option must always be in a source that the application can reach (/tmp, /srv, /opt, etc.)
## Example for /tmp/ location
mkdir /tmp/hadoop_scripte && chmod -R 755 /tmp/hadoop_scripte/
```

<references />

[^1]: Wikipedia Hadoop <https://en.wikipedia.org/wiki/Apache_Hadoop>
