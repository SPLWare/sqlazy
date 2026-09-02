### Action: align
Syntax: <align_expression> [according <ordered_collection_or_other_table>] [with <benchmark_expression>] [take {<original_column_name> [as <new_column_name>]}] [sort_only] [rest_keep] [partition <partition_field>]
Parameter: **align_expression** **according**
These two parameters must be used together. The **align_expression** parameter is an expression related to the fields of the focus table, and its calculation result is used for alignment. It can be a single column (a kind of expression). Required parameter; type is an expression; the parameter name must be omitted.
The **according** parameter indicates the alignment benchmark, assuming its values are unique. It can be an ordered collection or another table. When it is another table, by default the column values of the primary key of that table are used as the benchmark; if there is no primary key, the column values of the first column are used as the benchmark. You can also use the **with** parameter to explicitly specify the benchmark expression. Required parameter; type is ordered collection, table, ordered collection identifier, or table identifier; the parameter name cannot be omitted.
Note: If the **according** parameter contains field values not present in the focus table, by default, missing records should be inserted at the corresponding positions, where such records only have that field value and other fields are null. Conversely, if the focus table contains field values not present in the **according** parameter, by default, those records should be deleted. This is an important characteristic of the align action.
> Example: Current data of the employee table is as follows
EId	Dept	Name	Salary
2	HR	Ashley	11000
3	Sales	Rachel	9000
4	HR	Emily	7000
5	Sales	Ashley	16000
6	Marketing	Matthew	11000
Goal: Align the employee table by the Dept field according to ["Sales","R&D","HR"].
NLC: align Dept; according ["Sales","R&D","HR"]
Result:
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
	R&D		
2	HR	Ashley	11000
4	HR	Emily	7000
Explanation: The focus table does not have "R&D" which is in the ordered collection, but has "Marketing" which is not in the ordered collection. Following the notes for the according parameter, a new record should be inserted between the records corresponding to Sales and HR, with only the Dept value "R&D" and other fields null; the record corresponding to "Marketing" should be deleted.

Parameter: **with <benchmark_expression>**
When the **according** parameter is another table, this parameter specifies the column to use as the alignment benchmark (benchmark expression). By default, the column values of the primary key of the **according** table are used as the benchmark; if there is no primary key, the column values of the first column are used. Optional parameter; type is (column) identifier/field; the parameter name cannot be omitted.
> Align the Dept field of the employee table (focus table) according to the DeptID column of the department table.
NLC: align Dept; according department_table; with DeptID
Note: Since DeptID is the primary key of the department table, "with DeptID" can also be omitted. The NLC would be: align Dept; according department_table
> The fields of the employee table (focus table) are: EId, Name, StateName, Salary. The state table has fields: StateID, StateName, where StateID is the primary key and also the first column. Align the employee table by the "StateName" field according to the state table. Since the primary key and first column of the state table are both "StateID" instead of "StateName", you must use the **with** parameter to specify "StateName" as the benchmark expression.
NLC: align StateName; according state_table; with StateName
Note: Here "StateName" after "align" is a field of the employee table (focus table), while "StateName" after "with" is a column of the state table. Since the state table's primary key (and first column) is "StateID", the default would use "StateID" as the benchmark, not "StateName", so "with StateName" cannot be omitted.

Parameter: **take**
When the **according** parameter is another table, this parameter can be used to attach other columns (except the base column) of that table to the back of the focus table. Optional parameter; compound parameter; the parameter name cannot be omitted. This parameter is a compound parameter consisting of one or more pairs of sub-parameters: sub-parameter **original_column_name** and sub-parameter **as**, each pair representing a column in the other table.
Where sub-parameter **original_column_name** is a column name in the other table (including the row number column #) to be attached to the focus table. Required parameter; type is a column identifier; the parameter name must be omitted.
Where sub-parameter **as** is the new name for the **original_column_name** after being attached to the focus table. Optional parameter, default is to keep the original name; type is a column identifier; the parameter name cannot be omitted.
> Align the Dept field of the employee table (focus table) according to the primary key column (DeptID) of the department table, and attach the department_name and manager fields from the department table, where manager is renamed to manager_name.
NLC: align Dept; according department_table; take department_name, manager as manager_name.
 
Parameter: **sort_only**
If the **according** parameter contains field values not present in the focus table, by default, missing records should be inserted at the corresponding positions. This parameter is a modification of that default rule, i.e., when this parameter is used, no missing records are inserted. Optional parameter; boolean type; the parameter name cannot be omitted, and the parameter value must be omitted.
> Align the employee table by the Dept field according to ["Sales","R&D","HR"], only sort, do not supplement missing records.
NLC: align Dept; according ["Sales","R&D","HR"]; sort_only
Result:
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
2	HR	Ashley	11000
4	HR	Emily	7000
Parameter: **rest_keep**
If the focus table contains field values not present in the **according** parameter, by default, those records should be deleted. This parameter is a modification of that default rule, i.e., when this parameter is used, these records are no longer deleted, but placed at the end of the focus table. Optional parameter; type is boolean; the parameter name cannot be omitted, and the parameter value must be omitted.
> Align the employee table by the Dept field according to ["Sales","R&D","HR"], and keep the records in the employee table that cannot be aligned.
EId	Dept	Name	Salary
3	Sales	Rachel	9000
5	Sales	Ashley	16000
R&D		
2	HR	Ashley	11000
4	HR	Emily	7000
6	Marketing	Matthew	11000

Parameter: **partition**
Align by partition, where partitions do not affect each other, i.e., records of each partition are aligned to the same according parameter, conceptually similar to SQL's PARTITION BY. Optional parameter; identifier type; the parameter name cannot be omitted.
> Example: For each customer's records in the order example table, align by the product_type field according to the collection "Small Appliances","Textiles","Food".
NLC: align product_type; according ["Small Appliances","Textiles","Food"]; partition customer

