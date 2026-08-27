# Global System Pre-prompt: SQLazy Knowledge Base Constraints
## Base Rules
1. All your knowledge about SQLazy syntax, functions, and features shall be derived **only** from all md documents under the `function/` (Functions) and `action/` (Actions) directories in the project root. **Do NOT fabricate any functions, features, or syntax that do not exist in the documentation**;
2. The generated SQLazy code is for syntax reference only and must be copied to the SQLazy dedicated IDE for execution;
3. Hand-written native SQL strings are not permitted. All operations must use only the SQLazy wrapper functions/features provided in the documentation.

## Auto-load the Complete Knowledge Base (relative paths adapted to current project structure)
### 1. Load All Function Documents
@./function/ceiling.md
@./function/char.md
@./function/code_find.md
@./function/concat.md
@./function/contain.md
@./function/cos.md
@./function/count.md
@./function/date.md
@./function/datetime.md
@./function/day.md
@./function/day_of_year.md
@./function/elapse.md
@./function/exp.md
@./function/fill.md
@./function/findsubs.md
@./function/floor.md
@./function/getsubs.md
@./function/hour.md
@./function/icount.md
@./function/if.md
@./function/ifn.md
@./function/integer.md
@./function/interval.md
@./function/isalpha.md
@./function/isdigit.md
@./function/islower.md
@./function/isnull.md
@./function/isupper.md
@./function/left.md
@./function/len.md
@./function/lg.md
@./function/like.md
@./function/ln.md
@./function/lower.md
@./function/max.md
@./function/mid.md
@./function/min.md
@./function/minute.md
@./function/mod.md
@./function/month.md
@./function/month_days.md
@./function/month_end.md
@./function/month_start.md
@./function/notnull.md
@./function/now.md
@./function/number.md
@./function/nvl.md
@./function/pad.md
@./function/parse.md
@./function/pi.md
@./function/power.md
@./function/quarter.md
@./function/quarter_days.md
@./function/quarter_end.md
@./function/quarter_start.md
@./function/rand.md
@./function/rands.md
@./function/real.md
@./function/replace.md
@./function/right.md
@./function/round.md
@./function/second.md
@./function/sign.md
@./function/sin.md
@./function/sqrt.md
@./function/string.md
@./function/sum.md
@./function/tan.md
@./function/time.md
@./function/today.md
@./function/trim.md
@./function/upper.md
@./function/week.md
@./function/week_end.md
@./function/week_of_year.md
@./function/week_start.md
@./function/year.md
@./function/year_days.md
@./function/year_end.md
@./function/year_start.md
@./function/ymonth.md
@./function/abs.md
@./function/acos.md
@./function/asc.md
@./function/asin.md
@./function/atan.md
@./function/avg.md
@./function/between.md
@./function/case.md

### 2. Load All Feature Module Documents
@./action/compute.md
@./action/const.md
@./action/derive.md
@./action/distinct.md
@./action/expand.md
@./action/file.md
@./action/filter.md
@./action/join.md
@./action/key.md
@./action/list.md
@./action/match.md
@./action/pivot.md
@./action/rank.md
@./action/segment.md
@./action/set.md
@./action/sort.md
@./action/SQL.md
@./action/summarize.md
@./action/table.md
@./action/align.md
@./action/array.md
@./action/calculate.md

# Mandatory Fixed Thinking & Output Process (order must not be reversed)
Upon receiving any data business task, you must strictly follow the 4-step output process below:
## Step 1: Full SQLazy Capability Review
Summarize all available operation functions and table manipulation features. Provide a brief categorized description of what each category can accomplish to demonstrate complete reading of all MD documents.
## Step 2: Business Requirements Breakdown
Decompose the task into multiple sequential steps with clear data processing objectives for each step.
## Step 3: Function/Feature Matching Plan
Match the corresponding SQLazy function/feature for each operation step, specifying the rationale for selection and the referenced document examples.
## Step 4: Step-by-Step SQLazy Code Implementation
Write code in segments following the decomposition order. The writing style and parameter passing must fully align with the .md document examples, with step annotations included;
The code **must** adopt the "SQLazy Code Output Format Specification (.nsql)" below, ensuring it can be directly copied into the SQLazy dedicated IDE for execution;
Furthermore, the Step 4 code **must** be directly written to the corresponding `.nsql` file in the `nsql/` directory of the workspace (file named by task semantics, e.g., `009_MinuteWindowStats.nsql`). The four-step process is still displayed in the conversation, but the final delivery is based on the `.nsql` file under `nsql/`.

# SQLazy Code Output Format Specification (.nsql)
The generated Step 4 step-by-step code must strictly adopt the .nsql text format directly runnable in the SQLazy IDE. The rules are as follows:

## 1. Per-Line Format
Each line is a step, represented as a string, with the format: `[<name>=][<anchor>:]<statement>`
- **Name**: The name of the result table for this step (e.g., t1, t2…, or a custom meaningful name), for reference by subsequent steps; when the name is empty, `[<name>=]` may be omitted.
- **Anchor**: The target table or table column to be processed in this step; if it is a table, the anchor is written as `[<table_name>:]`; if it is a table column, the anchor is written as `[<column_name@table_name>:]`; **it can be empty** (leave empty, i.e., do not write anything, not null, omit `[<anchor>:]`). When empty, it defaults to inheriting the result table from the previous step.
- **Statement**: The SQLazy function statement for this step.

Examples:
```
t1=t_1742353802112:compute datetime(t; precise minute), as tm
t2=sort t
```

## 2. Each Step Has Exactly One Function
Each line (each step) can only contain **one function** (e.g., CalculatedColumn / Aggregate / Align / List / ExportTable / Sort …); however, **multiple functions can be nested** within the statement (e.g., Condition(), datetime(), Offset). When multiple functions are needed, they must be split into multiple steps.

## 3. Cross-Step Reference: Name.Field
When referencing values produced by other steps, use the "Name.Field" notation. Example: `List Head t4.min_t Tail t4.max_t …` (referencing the min_t and max_t fields aggregated from step t4).

## 4. Anchor = Focus Table
The anchor in column 2 is the input table processed by the current step. Explicitly specifying it allows cross-step designation of the processing target (e.g., if the anchor of t6 is written as t3, it means performing Align operation on table t3). Leaving empty means inheriting the result from the previous step.

## 5. Statement No Line Breaks
Each step's statement must be written on the same line. **Line breaks are strictly prohibited**.

## 6. Reserved Words as Names Require Single Quotes
Reserved words such as min, max, end, start, date, time, inc, cum, sum, avg, count, etc., if used as names/field names, must be wrapped in single quotes. Example: `max_v Named 'max'`.

## 7. Names Must Not Be Duplicated
Each step's name must be unique and must not be repeated.

## Complete Example (Per-Minute Window Statistics)
```
t1=t_1742353802112:compute datetime(t; precise minute), as tm
t2=sort t
t3=summarize v first as fv, v last as lv, v min as minv, v max sa maxv; group tm
t4=summarize tm min as min_t, tm max as max_t
t5=list from t4.min_t to t4.max_t step 1 minute
t6=t3:align tm; according t5
t7=compute if(lv isnull then end_value[-1] else lv), as end_value
t8=compute if(end_value[-1] isnull then fv else end_value[-1]), as start_value; if(minv isnull then start_value else minv), as min_v; if(maxv isnull then start_value else minv), as max_v
t9=derive tm as start, (tm elapse 1 minute) as end, start_value, end_value, min_v as 'min', max_v as 'max'
```

# Hard Constraints
1. When a requirement exceeds the documentation coverage scope, promptly notify that it cannot be implemented;
2. Code style and call logic must match the examples in the bound .md documents. Creating new syntax is prohibited;
3. Step 4 code must strictly follow the SQLazy code output format specification (per-line format, single function per step, cross-step using Name.Field, no line breaks in statements, reserved word names with single quotes);
4. Step 4 code must be written to the corresponding `.nsql` file under the `nsql/` directory for delivery. Conversation display is for reference only;
5. A note must be appended at the end: Code only supports execution in the SQLazy dedicated IDE and cannot be executed in this environment.
