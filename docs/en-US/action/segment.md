### Action: segment
Syntax: [<expression_for_fixed_condition>] [fixed_condition_or_record_count] [<count>] [condition <condition_expression>] [partition <partition_column>] [as <new_column_name>]
The purpose of this action is to segment records in order, i.e., assign a calculated segment number or group number (initial value 1) to a new column according to the record order, incrementing the group number only when a condition is met (equivalently, keeping the group number unchanged when the condition is not met). There are three types of conditions: the first is fixed conditions, including whether a certain field changes, remains unchanged, increases, does not increase, decreases, does not decrease. The second is grouping by record count, including: group every N records (the last group may have fewer than N), divide into M groups in total. The third is any conditional expression.
Where, the first type of fixed condition corresponds to the enum values of the parameter **fixed_condition_or_record_count**: change, unchanged, up, not_up, down, not_down, these 6 enum values belong to "fixed conditions".
The second type of grouping by record count corresponds to the enum values of the parameter **fixed_condition_or_record_count**: groups, step, these 2 enum values belong to "fixed record count".
The third type, any conditional expression, corresponds to the parameter **condition <condition_expression>**.
Parameter: **expression_for_fixed_condition**
The expression targeted by the first type of fixed condition; the simplest expression is a single column. Optional parameter; type is expression; parameter name must be omitted, parameter value cannot be omitted. This parameter does not support cross-row calculation and aggregation calculation, i.e., the expression cannot contain relative position calculations like F[i], F[a:b], nor aggregate calculations like sum, average of a set.
> A stock price table (focus table) already sorted by Date.
Date	ClosePrice
2021-01-01	10.2	
2021-01-02	11	
2021-01-03	12.1	
2021-01-04	11.3	
2021-01-05	9.2	
2021-01-07	11	
2021-01-08	9.1
Goal: fill the new field UpFlag with values; when the same UpFlag value appears for 2 or more consecutive records, it indicates a continuous rise; when it is 1, it indicates a drop or flat. Equivalent to using the segment action to fill the group number in the UpFlag field, targeting the ClosePrice column, with the fixed condition "down"
NLC: segment ClosePrice; down; as UpFlag
Result:
Date	ClosePrice	UpFlag
2021-01-01	10.2	1
2021-01-02	11	1
2021-01-03	12.1	1
2021-01-04	11.3	2
2021-01-05	9.2	3
2021-01-07	11	3
2021-01-08	9.1	4
Analysis: The records from 2021-01-01 to 2021-01-03 have the same UpFlag (group number), this group contains 3 records, indicating a continuous rise for 3 days.
Parameter: **fixed_condition_or_record_count**
This parameter corresponds to the first type of fixed conditions and the second type of grouping by record count. Optional parameter; type is enum; parameter name must be omitted, parameter value cannot be omitted.
The first type, fixed conditions, has 6 enum values as follows:
change: increment the group number when the parameter **expression_for_fixed_condition** changes.
unchanged: increment the group number when the parameter **expression_for_fixed_condition** does not change.
up: increment the group number when the parameter **expression_for_fixed_condition** increases.
not_up: increment the group number when the parameter **expression_for_fixed_condition** does not increase (unchanged or decreased).
down: increment the group number when the parameter **expression_for_fixed_condition** decreases.
not_down: increment the group number when the parameter **expression_for_fixed_condition** does not decrease (unchanged or increased).
> For the new column "Flag" of the focus table, fill in group numbers sequentially, incrementing the group number when the field Month does not change, and keeping the group number unchanged when Month changes.
NLC: segment Month; unchanged; as Flag

The second type, grouping by record count, note that the parameter **expression_for_fixed_condition** is not needed, but the parameter **count** is required. The parameter **fixed_condition_or_record_count** has 2 enum values as follows:
groups: roughly divide the total number of records into M groups evenly; each group has the same group number. Because perfect division is not guaranteed, it is roughly even. For example, 10 records divided into 4 groups, the group numbers are [1,1,1,2,2,2,3,3,3,4]
step: group every N records; each group has the same group number, the last group may have fewer than N.
> A stock price table (focus table) already sorted by Date.
Date	ClosePrice
2021-01-01	10.2	1
2021-01-02	11	1
2021-01-03	12.1	1
2021-01-04	11.3	2
2021-01-05	9.2	3
2021-01-07	11	3
2021-01-08	9.1	4
Goal: every 3 working days as a group, fill an incrementing group number into the new field "GroupNumber".
NLC: segment step; 3; as GroupNumber
Result:
Date	ClosePrice	GroupNumber
2021-01-01	10.2	1	1
2021-01-02	11	1	1
2021-01-03	12.1	1	1
2021-01-04	11.3	2	2
2021-01-05	9.2	3	2
2021-01-07	11	3	2
2021-01-08	9.1	4	3
Parameter: **count**
When the enum values of the parameter **fixed_condition_or_record_count** are groups and step, this parameter is needed to specify the total number of groups or the number of members per group corresponding to these two enum values. Optional parameter; type is integer; parameter name must be omitted.
Parameter: **condition <condition_expression>**
When the first type of fixed conditions and the second type of grouping by record count are insufficient, the third type, any conditional expression, is needed to calculate segments. In this case, the first three parameters are not needed. Optional parameter; type is conditional expression; parameter name cannot be omitted. This parameter supports cross-row calculation and aggregation calculation, i.e., the expression can contain relative position calculations like F[i], F[a:b], and aggregate calculations like sum, average of a set.
> A stock price table (focus table) already sorted by Date.
Goal: create a new field "UpFlag" on the focus table and fill group numbers, starting from 1, increment the group number when the condition "ClosePrice<ClosePrice[-1]" is true, keep the group number unchanged when the condition is false.
NLC: segment condition ClosePrice<ClosePrice[-1]; as UpFlag
Analysis: The condition in this example is simple and can be replaced by fixed conditions, so it is equivalent to: segment ClosePrice; down; as UpFlag
In particular, this parameter can be used to express "when a certain expression equals or does not equal a certain value, increment the group number".
> Assign the new column "Number" of the focus table starting from 1, increment the number when the condition is "Month <> 3", otherwise keep the number unchanged.
NLC: segment condition (Month <> 3); as Number
Parameter: **partition**
Calculate segment numbers by partition, partitions do not affect each other, conceptually similar to SQL's PARTITION BY. Optional parameter; type is (field) identifier; parameter name cannot be omitted.
Parameter: **as <new_column_name>**
New column name, the group number will be assigned to this new column. Required parameter; type is (field) identifier; parameter name cannot be omitted.
Goal: use the segment action to fill the group number into the new column "NewGroupNumber" of the focus table, with the condition "ClosePrice<ClosePrice[-1]"
NLC: segment condition ClosePrice<ClosePrice[-1]; as NewGroupNumber