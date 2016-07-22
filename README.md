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
