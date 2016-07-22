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
