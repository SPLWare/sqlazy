#### function: datetime
Syntax: datetime(<date_or_datetime_string> <time> [format <format_pattern>] [language <language_code>] [precise])
Return: datetime type. This function has two overloads: if <date_or_datetime_string> is a date type, generate a datetime using that date and <time>; if it is a datetime string, generate a datetime from the string.
Parameter **<date_or_datetime_string>**: date type or datetime string. Required parameter; type is date or string; parameter name omitted.
> Example: Generate datetime from string "2026-04-01 08:30:50".
NLC snippet: datetime("2026-04-01 08:30:50") // result is datetime 2026-04-01 08:30:50
Parameter **<time>**: time type, used to compose datetime. Non-required parameter; type is time; parameter name omitted.
> Example: Generate datetime from date 2026-04-01 and time 08:30:50.
NLC snippet: datetime(date("2026-04-01"),time(" 08:30:50")) // result is datetime 2026-04-01 08:30:50
Parameter **[format <format_pattern>]**: When <date_or_datetime_string> is a datetime string, parse it using this pattern. Non-required parameter; type is string; parameter name cannot be omitted.
> Example: Parse "12/28/1972 10:23:43" using pattern "MM/dd/yyyy HH:mm:ss".
NLC snippet: datetime("12/28/1972 10:23:43"; format "MM/dd/yyyy HH:mm:ss") // result is datetime 1972-12-28 10:23:43
Parameter **[language <language_code>]**: When <date_or_datetime_string> is a datetime string, parse it using this language and [format <format_pattern>]. Non-required parameter; type is string; parameter name cannot be omitted. Note: common languages include en and zh.
> Example: Parse "4 五月 2001 3:08 下午" using Chinese pattern "d MMM yyyy h:mm a".
NLC snippet: datetime("4 五月 2001 3:08 下午"; format "d MMM yyyy h:mm a"; language "zh") // result is datetime 2001-05-04 15:08:00
Parameter **[precise]**: When converting the string <date_or_datetime_string> to datetime, restrict time precision to hour, minute, or second. Non-required parameter; type is enumeration; enumeration values: hour, minute, second; parameter name cannot be omitted.
> Example: Convert "2026-04-01 08:30:50:640" to datetime with minute precision.
NLC snippet: datetime(datetime("2026-04-01 08:30:50:640"); precise minute) // result is datetime 2026-04-01 08:30:00