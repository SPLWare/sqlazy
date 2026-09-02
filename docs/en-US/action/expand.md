### Action: expand
Syntax: <list> [use <column_expression>] [as <new_column_name>] [take {<original_column_name> [as <new_column_name>]}]

Parameter: **as**
The column to fill with expanded values, can be a new column or an existing column in the focus table. If the **list** parameter is another table in the context, omitting this parameter means the expanded column name will use the column name of the column that serves as the expanded set in that table. Optional parameter; (field) identifier type; parameter name cannot be omitted. This parameter is generally used together with the **list** parameter.

Parameter: **list** 
One of several forms used to generate a set, including: constant set (including identifiers), natural number N, expressions that can generate a set, other tables in the context (identifiers). Required parameter; type is multiple forms; parameter name must be omitted.
First form: When the list parameter is a constant set, assuming the number of set members is M, each record of the focus table is expanded into M records, with the set members filled into the specified column in sequence.
> Student_table (focus table) originally has 2 records, data as follows:
StudentName	Class
Zhang San	5-2		
Li Si	5-2
Requirement: Now expand each record into 3 records using the ordered set ["Physics","Math","English"], named as Subject.
Expected result:
StudentName	Class	Subject
Zhang San	5-2		Physics
Zhang San	5-2		Math
Zhang San	5-2		English
Li Si	5-2		Physics
Li Si	5-2		Math
Li Si	5-2		English
NLC: expand ["Physics","Math","English"]	
Second form: When the list parameter is an integer N, the set is the natural numbers 1 to N. Each record of the focus table is expanded into N records, with the set members filled into the Semester column of the focus table.
> Student_table originally has 2 records, the Semester column is null, data as follows:
StudentName	Class	Semester
Zhang San	5-2		
Li Si	5-2
Requirement: Now expand each record into 2 records using the natural number 2, fill into the "Semester" column.
Expected result:
StudentName	Class	Semester
Zhang San	5-2		1
Zhang San	5-2		2
Li Si	5-2		1
Li Si	5-2		2
NLC: expand 2 as Semester

Third form: When the parameter list is an expression that can generate a set, assuming the set has M members, each record of the focus table expands into M rows, and the set members are sequentially filled into the specified column. Note that this expression is restricted; the generated set is relatively fixed and cannot vary with the current record of the focus table.  
> Student_table (focus table) originally has 2 records, data as follows:  
Student Name	Class  
Zhang San	Class 5 Grade 2	  
Li Si	Class 5 Grade 2  
Requirement: Now use the expression (if(parameter1>2 then ["Chemistry","Language"]; else ["Physics","Math","English"])) to expand each record into multiple rows, assuming variable parameter1=3.  
Expected result:  
Student Name	Class	Subject  
Zhang San	Class 5 Grade 2	Chemistry  
Zhang San	Class 5 Grade 2	Language  
Li Si	Class 5 Grade 2	Chemistry  
Li Si	Class 5 Grade 2	Language  
NLC: expand (if(parameter1>2 then ["Chemistry","Language"]; else ["Physics","Math","English"])) as Subject  

Fourth form: When the list parameter is another table in the context, if that table has a primary key, by default the set is all values of the primary key column; if it has no primary key, by default the set is all values of the first column of that table. You can also use the **use** parameter to specify which column to take. Assuming the set length is M, each record of the focus table is expanded into M records, with the set members filled into the specified column.
> In the context there is a Gift_table, with structure [GiftName, Brand, Grade], no primary key, with N records. The focus table is Customer_table. Now require to sequentially write the GiftName of Gift_table into the PlannedGift column of Customer_table, expanding each record into N records.
NLC: expand Gift_table as PlannedGift
Note: Gift_table has no primary key, so the default uses the first column (GiftName) as the expanded set; therefore the "use" parameter is omitted here.

Parameter: **use <column_expression>**
When the **list** parameter is another table, this parameter specifies which column of that table to take values from as the expanded set. By default, the column values of the primary key of the **list** table are used as the expanded set; if there is no primary key, the column values of the first column are used. Optional parameter; type is (column) identifier/field; the parameter name cannot be omitted.
> In the context there is a Course_table with structure [CourseID, CourseName, Teacher], where CourseID is the primary key. The focus table is Student_table. Now require to use the CourseName of Course_table as the expanded set, sequentially writing it into the ElectiveCourse column of Student_table, expanding each record into N records.
NLC: expand Course_table; use CourseName; as ElectiveCourse
Note: Since the primary key of Course_table is "CourseID" (also the first column), the default would use CourseID as the expanded set, not CourseName, so "use CourseName" must be specified.

Parameter: **take**
When the **list** parameter is another table, this parameter can be used to join other columns of that table (except the column that generates the set) to the back of the focus table. Optional parameter; composite parameter; parameter name cannot be omitted. This parameter is a composite parameter consisting of one or more pairs of sub-parameters, i.e., sub-parameter **original_column_name** and sub-parameter **as**, each pair representing one column in the other table.
Where, sub-parameter **original_column_name** is a column name in the other table (including the sequence column #) to be joined to the focus table. Required parameter; type is column identifier; parameter name must be omitted.
Where, sub-parameter **as** is the new name after joining the **original_column_name** to the focus table. Optional parameter, default keeps the original name; type is column identifier; parameter name cannot be omitted.
> Write the GiftName of Gift_table sequentially into the PlannedGift column of Customer_table (focus table), and join the Brand and Grade fields to Customer_table, expanding each record into N records, where the field Grade is renamed to Level.
NLC: expand Gift_table as PlannedGift; take Brand, Grade as Level
