# Oracle prep-logs
Q | Info 
--- | --- 
**Soft parses are better**|database skips the optimization and row source generation steps, proceeding straight to execution.
Softer soft parses|configuring the session shared SQL area can sometimes reduce the amount of latching in the soft parses, making them "softer."
inevitable HARD parses|The database always perform a hard parse of DDL
recursive SQL | would issue a COMMIT before executing
adaptive plan| changes plan in runtime
adaptive reoptimization|changes plan after the initian execution
Reoptimization|Statistics feedback, Performance feedback (PARALLEL_DEGREE_POLICY =  ADAPTIVE)
Populate inmemory store for a table| ALTER TABLE mysales INMEMORY;
SQL baselines| ``` SELECT plan_name,sql_handle,sql_text,enabled, accepted  FROM   dba_sql_plan_baselines WHERE  sql_text LIKE '%SPM%';```<br>ACC- accepted plan<br>SELECT PLAN_TABLE_OUTPUT<br>FROM   V$SQL s, DBA_SQL_PLAN_BASELINES b,<br>&nbsp;&nbsp;&nbsp;&nbsp;TABLE(<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;DBMS_XPLAN.DISPLAY_SQL_PLAN_BASELINE(b.sql_handle,b.plan_name,'basic')<br>&nbsp;&nbsp;&nbsp;&nbsp;) t<br>WHERE s.EXACT_MATCHING_SIGNATURE=b.SIGNATURE<br>AND    b.PLAN_NAME='SQL_PLAN_bk42daz2f53zwc69cec1f';<br>
Accept new plan|cVal := dbms_spm.evolve_sql_plan_baseline(sql_handle=>' SQL_b9104d57c4e28ffc',verify=>'NO');
High Water Mark (HWM) Loading|ALTER SESSION DISABLE PARALLEL DML;<br>INSERT /*+ APPEND */ INTO sales_copy t1 SELECT ...;<br><ul><li> - The new data is inserted into extents allocated above the table (or partition) high water mark</li><li> - Rows above the high water mark will not be scanned by queries, so the new data remains invisible until the transaction is committed</li></ul>
 equi-partition load|will map parallel execution servers to particular partitions in the target table
Temp Segment Merge (TSM) Loading|Parallel Create Table As Select (PCTAS) for a non-partitioned table:<br>CREATE TABLE sales_copy PARALLEL 8 AS SELECT * FROM sales;<br>Parallel Insert Direct Load (PIDL) into a non-partitioned table or a single partition: <br>INSERT /*+ APPEND PARALLEL(t1,8) */ INTO sales_copy_nonpart t1 SELECT ...;<br>INSERT /*+ APPEND PARALLEL(t1,8) */ INTO sales_copy partition (part_p1) t1 SELECT ...;<br>When a parallel data load begins, each parallel execution server producer is allocated to a single temporary segment. The temporary segments will reside in the same tablespace as the table or partition being loaded, and at commit time the temporary segments will be merged with the base segment by manipulating the extent map. In other words, the temporary segments will be incorporated into the table or partition without moving the data a second time.
 external fragmentation|Extent trimming can sometimes leave gaps between extents that may be too small for new extents to be created and used. This is commonly referred to as external fragmentation.
 UNIFORM|If you are using UNIFORM extent allocation then extents will not be trimmed.
 internal fragmentation | If data is only loaded into a table using direct path load, then unfilled space in segments will remain unused. 
 High Water Mark Brokering (HWMB)| new row data is added directly to base segments above the high water mark.<br>default for single-segment loads in Oracle Database 11g if Auto DOP is used <br>multiple PX servers may need to insert row data into the same database segment. In this situation, it becomes necessary for the new position of the high water mark to be coordinated or “brokered” between multiple processes and even multiple database servers. Brokering is implemented using database HV enqueues. Each segment has its own HV enqueue, which is used to record the position that the high water mark must be moved to once the transaction is committed. The enqueue ensures that multiple processes can’t update the high water mark position value at the same time.
Hybrid TSM/HWMB|this is a hybrid solution that combines the beneficial characteristics of temp segment merge and high water mark brokering. Oracle Database 12c we combine high water mark brokering with temp segment merge to eliminate cross-instance contention for HV enqueues while keeping down the number of new extents that need to be created per load operation. 
In-Memory Aggregation|The basic approach of in-memory aggregation is to aggregate while scanning. To optimize query blocks involving aggregation and joins from a single large table to multiple small tables, such as in a typical star query, the transformation uses KEY VECTOR and VECTOR GROUP BY operations. 
VECTOR_TRANSFORM |enables the vector transformation on the specified query block, regardless of costing. NO_VECTOR_TRANSFORM disables the vector transformation from engaging on the specified query block.
VECTOR_TRANSFORM_FACT|includes the specified FROM expressions in the fact table generated by the vector transformation.
VECTOR_TRANSFORM_DIMS|includes the specified FROM expressions in enabled dimensions generated by the vector transformation
Table Expansion|You can configure a table so that an index is only created on the read-mostly portion of the data, and active portion of the table is not indexed. This way DMLs are not impeding loads.<br>Works only on partitioned tables with local index.<br>Optimizer transforms the query into UNION ALL statement<br> EXPAND_TABLE hint, The hint overrides the cost-based decision
Detect partitions, accessed by given query|SET LINESIZE 150<br>SET PAGESIZE 0<br>SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY_CURSOR(format => **'BASIC,PARTITION'**));
Join Factorization|oin factorization transformation can share common computations across the UNION ALL branches. Avoiding an extra scan of a large base table can lead to a huge performance improvement.
adaptive plan| The STATISTICS_LEVEL initialization parameter is set to ALL.<br>query **DBMS_XPLAN.DISPLAY_CURSOR** to see the final plan and adaptive plan<br>SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY_CURSOR(FORMAT=>'+ALLSTATS'));<br>Performance of 2 statements:<br>SELECT CHILD_NUMBER, CPU_TIME, ELAPSED_TIME, BUFFER_GETS<br>FROM   V$SQL<br>WHERE  SQL_ID = 'gm2npz344xqn8';<br>Adaptive plan:<br>SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY(FORMAT=>'+ADAPTIVE'));
PQ_DISTRIBUTE|explicitly forces a partial partition-wise join
UNIQUE key|index does not contain ROWIDs in leaf blocks
NULLs in B-tree indexes| Single column indexes never store NULLs.
ndex Skip Scan|The number of distinct values in the leading columns of the index determines the number of logical subindexes
Bitmap index|The database stores a bitmap index in a B-tree structure with corresponding rowid range and bitmap.
nested loops join | work best on small tables with indexes on the join conditions<br>The optimizer always tries to put the smallest row source first, making it the driving table<br> if outer table is smal (threshold is not passed) the NLJ is used, otherwise the rest of the table is loaded into memory, hashed and  HJ is used to join with inner table.
HASH JOIN| used on equijoin(=)<br>When smaller dataset fits in memory. IN this case cost is single pass over 2 data sets.<br>Hash Table is in PGS, rows are accessed without latching, reduces logical I/O<br>If data does not fit memory - row sources are partitioned.
 index clustering factor |measures the physical grouping of rows in relation to an index value, such as last name
 Bulk Load stats|if you roll back the transaction, then the database automatically deletes statistics gathered during the bulk load.<br>No index stats or histograms.<br> Run EXEC DBMS_STATS.GATHER_TABLE_STATS( user, 'SH_CTAS', options => 'GATHER AUTO' ); to gather stats missing after bulk load
 Determine if stats were gathered|execute DBMS_STATS.FLUSH_DATABASE_MONITORING_INFO, and then query USER_TAB_MODIFICATIONS.INSERTS. If the query returns a row indicating the number of rows loaded, then the statistics were **not** gathered automatically during the most recent bulk load
Bulk load stats are not gathered if|It's a SYS schema<br>It's nested table <br>It's IOT<br>It's external table<br>It's GTT ON COMMIT<br>It is loaded using a multitable insert statement.<br>It has  virtual columns<br>It has PUBLISH=False<br>It's stats are locked<br>It's partitioned+INCREMENTAL=true+extended syntax is not used.<br>Example:DBMS_STATS.SET_TABLE_PREFS(null, 'sales', incremental', 'true'). In this case, the database does not gather statistics for INSERT INTO sales SELECT, even when sales is empty. However, the database does gather statistics automatically for INSERT INTO sales PARTITION (sales_q4_2000) SELECT.
Bulk load stats HINTS|By default, the database gathers statistics during bulk loads.<br>Disable with NO_GATHER_OPTIMIZER_STATISTICS hint
SQL Plan Directives example|SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY_CURSOR(FORMAT=>'**ALLSTATS LAST**'));<br>Check V$SQL.IS_REOPTIMIZABLE value<br>Monitor directives using the views DBA_SQL_PLAN_DIRECTIVES and DBA_SQL_PLAN_DIR_OBJECTS<br>EXEC DBMS_SPD.FLUSH_SQL_PLAN_DIRECTIVE;
Dynamic Stats|ALTER SESSION SET OPTIMIZER_DYNAMIC_SAMPLING =11;
Automatic Histograms|If you gather statistics for a table and do not query the table, then the database does not create histograms for columns in this table. For the database to create the histograms automatically, you must run one or more queries to populate the column usage information in SYS.COL_USAGE$.<br>run DBMS_STATS for a table with the METHOD_OPT parameter set to the default SIZE AUTO.<br> Use USER_TAB_COL_STATISTICS to check if Hms exist.<br>before and after selects: EXEC DBMS_STATS.GATHER_TABLE_STATS(USER,'SALES2',OPTIONS=>'GATHER AUTO');
A popular value |occurs as an endpoint value of multiple buckets.
Create histograms|BEGIN<br>  DBMS_STATS.GATHER_TABLE_STATS ( <br>    ownname    => 'SH'<br>,   tabname    => 'COUNTRIES'<br>,  <br>method_opt => 'FOR COLUMNS COUNTRY_SUBREGION_ID'<br>);<br>END;
Check histogram information|SELECT TABLE_NAME, COLUMN_NAME, NUM_DISTINCT, HISTOGRAM<br>FROM   USER_TAB_COL_STATISTICS<br>WHERE <br>TABLE_NAME='COUNTRIES'<br>AND    COLUMN_NAME='COUNTRY_SUBREGION_ID';
Check endpoint number and endpoint value|SELECT ENDPOINT_NUMBER, ENDPOINT_VALUE<br>FROM   USER_HISTOGRAMS<br>WHERE  TABLE_NAME='COUNTRIES'<br>AND   <br>COLUMN_NAME='COUNTRY_SUBREGION_ID';
Create top-frequency historgams|BEGIN<br>  DBMS_STATS.GATHER_TABLE_STATS (<br>    ownname    => 'SH'<br>,   tabname    => 'COUNTRIES'<br>,  <br>method_opt => 'FOR COLUMNS COUNTRY_SUBREGION_ID **SIZE 7**'<br>);<br>END;
synopsis| is a sample of distinct values<br>DB is merging partition-level synopises to come up with global NDV
Incremental Statistics Maintenance|Use DBMS_STATS.SET_TABLE_PREFS to set the INCREMENTAL value<br>AUTO_SAMPLE_SIZE for ESTIMATE_PERCENT and AUTO for GRANULARITY <br>atabase gathers global index statistics by performing a full index scan
starts for exchange partition|DBMS_STATS can create a synopsis on a nonpartitioned table. The synopsis enables the database to maintain incremental statistics as part of a partition exchange operation without having to explicitly gather statistics on the partition after the exchange
Dynamic sampling|ALTER SESSION SET OPTIMIZER_DYNAMIC_SAMPLING=4;
pending statistics |save the statistics and not publish them immediately after the collection.
Gather stats on column group |BEGIN<br>  DBMS_STATS.GATHER_TABLE_STATS( 'sh','customers',<br>METHOD_OPT => 'FOR ALL COLUMNS SIZE<br>SKEWONLY ' ||<br>'FOR COLUMNS SIZE SKEWONLY (cust_state_province,country_id)' );<br>END;<br>/
