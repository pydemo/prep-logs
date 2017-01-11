#Bigdata prep-logs
Q | Info 
--- | ---
impedance match|describes the need to find the ideal solution for a given problem. Instead of using a “one-size-fits-all” approach, you should know what else is available
latency| that is, how fast the system can respond to read and write requests. This is often measured in harvest and yield.
MapReduce |was the missing piece to the GFS architecture, as it made use of the vast number of CPUs each commodity server in the GFS cluster provided. 
Drawback of GFS design|it was good with a few very, very large files, but not as good with millions of tiny files, because the data retained in memory for each file by the master node ultimately bounds the number of files under management. The more files, the higher the pressure on the memory of the master.
HBase |is implementing the Bigtable storage architecture very faithfully so that we can explain everything using HBase. 
The HBase regions |are equivalent to range partitions as used in database sharding.
 |Initially there is only one region for a table, and as you start adding data to it, the system is monitoring it to ensure that you do not exceed a configured maximum size. If you exceed the limit, the region is split into two at the middle key--the row key in the middle of the region—creating two roughly equal halves
  |Splitting and serving regions can be thought of as **autosharding**
 |system is integrated with the MapReduce framework by supplying wrappers that convert tables into input source and output targets for MapReduce jobs
  |There is also the option to run client-supplied code in the address space of the server. The server-side framework to support this is called **coprocessors**. The code has access to the server local data and can be used to implement lightweight batch jobs, or use expressions to analyze or summarize data based on a variety of operators.
HFiles| are persistent and ordered immutable maps from keys to values. Internally, the files are sequences of blocks with a block index stored at the end. The index is loaded and kept in memory when the HFile is opened. The default block size is 64 KB but can be configured differently if required. The store files internally provide an API to access specific values as well as to scan ranges of values given a start and end key.
delete marker| (also known as a tombstone marker) is written to indicate the fact that the given key has been deleted
LSM-trees |are storing data in multipage blocks that are arranged in a B-tree-like structure on disk
three major components to HBase:| the client library, at least one master server, and many region servers. 
**HBase**| is a distributed, persistent, strictly consistent storage system with near-optimal write—in terms of I/O channel saturation—and excellent read performance, and it makes efficient use of disk space by supporting pluggable compression algorithms that can be selected based on the nature of the data in specific column families.
