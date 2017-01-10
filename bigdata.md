#Bigdata prep-logs
Q | Info 
--- | ---
impedance match|describes the need to find the ideal solution for a given problem. Instead of using a “one-size-fits-all” approach, you should know what else is available
latency| that is, how fast the system can respond to read and write requests. This is often measured in harvest and yield.
MapReduce |was the missing piece to the GFS architecture, as it made use of the vast number of CPUs each commodity server in the GFS cluster provided. 
Drawback of GFS design|it was good with a few very, very large files, but not as good with millions of tiny files, because the data retained in memory for each file by the master node ultimately bounds the number of files under management. The more files, the higher the pressure on the memory of the master.
HBase |is implementing the Bigtable storage architecture very faithfully so that we can explain everything using HBase. 
