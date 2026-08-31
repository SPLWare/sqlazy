#### function: like
Syntax: (<object_parameter> like <pattern> [case_insensitive] [spl_match])
Return: boolean. Returns true if <object_parameter> matches the <pattern> containing wildcards, otherwise false.
Parameter **<object_parameter>**: Required parameter; string type; parameter name omitted.
Parameter **<pattern>**: string containing wildcards. Default wildcards: % (zero or more characters), _ (one character). Required parameter; string type; parameter name omitted.
> Example: Does "abc123" match the pattern "abc%"?
NLC snippet: ("abc123" like "abc%") // returns true
> Example: Does "abc123" match the pattern "abc_"?
NLC snippet: ("abc123" like "abc_") // returns false
Parameter **[case_insensitive]**: Default is case-sensitive. With this parameter the matching becomes case-insensitive. Optional parameter; boolean type; parameter name must not be omitted, parameter value must be omitted.
> Example: Does "abc123" match the pattern "ABC%", case-insensitive?
NLC snippet: ("abc123" like "ABC%"; case_insensitive) // returns true
Parameter **[spl_match]**: Default wildcards are %, _. With this parameter, switch to SPL wildcards: * (zero or more characters), ? (one character). Optional parameter; boolean type; parameter name must not be omitted, parameter value must be omitted.
> Example: Does "abc123" match the pattern "abc*", using SPL wildcards?
NLC snippet: ("abc123" like "abc*"; spl_match) // returns true
