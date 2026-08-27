#### function: case
Syntax: (<object_parameter> case {<comparison_value> [then <equal_expression>]} [else <unequal_expression>])
Return value: simple data type.
Parameter **<object_parameter>**: original value/expression to be evaluated. Required parameter; simple data type; parameter name omitted.
Parameter **<comparison_value>**: value to compare with <object_parameter>. Required parameter; simple data type; parameter name omitted.
Parameter **[then <equal_expression>]**: When <object_parameter> equals <comparison_value>, return the result of this parameter. Required parameter; type is expression; parameter name cannot be omitted. Note that this parameter must be used together with <comparison_value>.
> Example: If year(order_test_date) = 2024, return "the year before last".
NLC snippet: (year(order_test_date) case 2024 then "the year before last").
Multiple pairs of <comparison_value> and [then <equal_expression>] are allowed, similar to the logic of a Java switch case.
> Example: If year(order_test_date) = 2024, return "the year before last"; = 2025, return "last year"; = 2026, return "this year".
NLC snippet: (year(order_test_date) case 2024 then "the year before last", 2025 then "last year", 2026 then "this year")

Parameter **else <unequal_expression>**: When <object_parameter> does not equal any of the <comparison_value>s, return this parameter as a fallback. Non-required parameter; type is expression; parameter name cannot be omitted.
> Example: If year(order_test_date) = 2024, return "the year before last"; = 2025, return "last year"; = 2026, return "this year"; else return "other year".
NLC snippet: (year(order_test_date) case 2024 then "the year before last", 2025 then "last year", 2026 then "this year"; else "other year")
