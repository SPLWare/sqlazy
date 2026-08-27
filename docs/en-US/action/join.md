### Action: join
Description: The right table is joined to the left table (the current/focused table) based on the specified base column, and the specified columns are attached to the left table to form a wider table. Supports left join, full join, and positional (by row number) join. Note: after this action executes, the record order usually does not remain consistent with the focus table (main table); calculations related to order should be re-sorted.
Syntax: [<base_column_name>] [with <associated_table_name>] [<join_column_name>] [take {[<original_join_column_name>] [as <new_join_column_name>]}] [option] [condition <condition_expression>]
Parameter: **base_column_name** 
One or more fields in the left table (current table) used for the join, including # (the positional field/row number field). Optional parameter; if absent, it means that a fully non-equi join must be established via the **condition <condition_expression>** parameter; if present, an equi-join is established using this parameter, and a non-equi join condition can be further added using **condition <condition_expression>**. Type is a field identifier or a set of field identifiers; the parameter name must be omitted.
> Join the employee table and department table on the department number field.
Partial NLC code: join department_number…  // the ellipsis indicates the code is incomplete and not yet runnable

Parameter: **with <associated_table_name>**
The right table to be joined to the left table. Required parameter; type is a table identifier; the parameter name cannot be omitted.
> Partial NLC code: join department_number; with department_table…

Parameter: **join_column_name**
One or more fields in the right table used for the join, including # (the positional field/row number field). Required parameter; omitting the parameter value means the value is the primary key of the right table or a column in the right table with the same name as the base column; type is a field identifier or a set of field identifiers; the parameter name must be omitted.
> Partial NLC code: join department_number; with department_table; department_number…  // Here the join column name is the same as the base column name, so it can be omitted.

Parameter: **take**
After the join relationship is established, all columns from the right table that need to be attached to the left table. Required parameter; contains 2 sub-parameters: **original_join_column_name**, **as <new_join_column_name>**. Using these two sub-parameters once attaches one column from the right table; to attach multiple columns, use them multiple times. The parameter name "take" cannot be omitted.
Sub-parameter: **original_join_column_name**	A single column in the right table to be attached to the left table. Required parameter; type is a single field identifier; the parameter name must be omitted. Note: if the user instruction requires attaching all columns, all field names of the right table from the referenced tables in the context should be taken as values for this parameter.
Sub-parameter: **as <new_join_column_name>** The new name for the original join column after renaming. Optional parameter, meaning it can be left unchanged; type is a single field identifier; the parameter name cannot be omitted.
> Join the employee table and department table on the department number field, and attach the department_name and office_address columns from the department table to the left table. The original field office_address is renamed to address.
NLC: join department_number; with department_table; department_number; take department_name, office_address as address	// This is a complete code, no ellipsis.

Parameter: **option**
The join type for the association. Required parameter; if absent, the parameter value defaults to left join, as in the previous example; enumerated type; the parameter name must be omitted. There are 4 enumeration values: left, inner, full, align.
left, inner, full: These three enumeration values are understood in the usual sense, similar to the three join types in SQL.
> Instead of the default left join, use a full join to join the employee table and department table.
NLC: join department_number; with department_table; department_number; take department_name, office_address as address; full

align: Indicates joining by position or row number #, i.e., records with the same row number are joined together. When using align, the base column name and join column name are both # and can be omitted.
Parameter: **condition <condition_expression>**
This parameter is used to filter the result after joining. It usually contains fields from the base table (left table) and the right table, where base table fields must have an @ suffix and non-base table fields must have no @ suffix, e.g., in the NLC code snippet: order_date>registration_date@. Here, order_date is a right table field and registration_date is a left table field. The role of this parameter is similar to the "where" condition in "join on… where…" in SQL, e.g., in SQL code: "select left_table.* from left_table inner join right_table on left_table.id=right_table.id where right_table.order_date>left_table.registration_date", this parameter is analogous to "right_table.order_date>left_table.registration_date" in SQL.
Optional parameter; if absent, the **base_column_name** parameter must be present; if present, the **base_column_name** parameter may be absent. Type is a condition expression; the parameter name cannot be omitted. Note: when this parameter is present, only inner join and left join are supported, full join is not supported.
> Example: Join the employee table and department table on the department number field, attach the department_manager and department_address columns from the department table, and filter with the condition (employee_salary@>=5000 and (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)).
NLC: join department_number; with department_table; department_number; take department_manager department_address; inner; condition (employee_salary@>=5000 and (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name))
> Example: Join the employee table and department table, attach the department_manager and department_address columns from the department table, and filter with the condition (employee_salary@>=5000 and (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)).
NLC: join with department_table; take department_manager department_address; inner; condition (employee_salary@>=5000 and (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name))

Special rule: You must convert interval joins to the **condition <condition_expression>** parameter. This rule means that user instructions may contain descriptions where a left table field falls within an interval of two right table fields, such as "the price@ field of the left table is between budget_1 and budget_2 of the right table". In this case, translate this description into an equivalent interval condition expression "price@>=budget_1 and price@<=budget_2", and add it to the **condition <condition_expression>** parameter. When there is no explicit open/closed interval description, use left-open right-open interval.
> Example: Join the employee table and department table on the department number field, attach the department_manager and department_address columns from the department table, employee_salary@ is between salary_lower and salary_upper, and filter with the condition (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)).
NLC: join department_number; with department_table; department_number; take department_manager department_address; inner; condition ((employee_salary@>=salary_lower and employee_salary<=salary_upper) and (["Sales Dept 1","Sales Dept 2","Product Dept"] contain department_name)) 


