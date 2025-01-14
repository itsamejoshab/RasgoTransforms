

# datespine_groups

Generates a date spine across custom groupings. Each grouping will be returned with a 
continuous set of dates.
Spines can be calculated globally using the table min/max or locally using each group min/max.

All intervals are considered to be start-inclusive and end-exclusive, or `[start, end]`. 
The join with the date spine will be an outer join such that all intervals are present 
and all data that does not fall into one of those intervals is excluded. 


## Parameters

|    Argument     |    Type     |                                                                                                                                                 Description                                                                                                                                                 | Is Optional |
| --------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------- |
| group_by        | column_list | The column(s) used to partition you data into groups. A datespine will be calculated for each group.                                                                                                                                                                                                        |             |
| date_col        | column      | The column used to create intervals. This must be a datetime column.                                                                                                                                                                                                                                        |             |
| start_timestamp | timestamp   | The timestamp to start calculating from. If not provided, the min of the date_col will be used                                                                                                                                                                                                              | True        |
| end_timestamp   | timestamp   | The timestamp to calculate to If not provided, the max of the date_col will be used                                                                                                                                                                                                                         | True        |
| interval_type   | date_part   | A valid SQL datepart to slice the date_col. For interval types, see [this Snowflake doc.](https://docs.snowflake.com/en/sql-reference/data-types-datetime.html#interval-constants)                                                                                                                          |             |
| group_bounds    | value       | Pass 'global' to construct each datespine using the same start & end date (i.e. the min/max of the entire table)  Pass 'local' to constuct each datespine using the min/max of the group as the start & end dates Pass 'mixed' to use a local start, while retaining a global end date 'global' is default  | True        |


## Example

```python
ds = rasgo.get.dataset(74)

# Usage 1: Local datespines - will return a datespine per group with 
# min/max values within the group
ds2 = ds.datespine_groups(
    group_by=['PRODUCTKEY'],
    date_col='ORDERDATE',
    group_bounds='local',
    interval_type='day',
    start_timestamp='2010-12-29',
    end_timestamp='2014-01-28' 
)
ds2.preview()

# Usage 2: Global datespines - will return a datespine per group with 
# min/max values set to the global min/max
ds2 = ds.datespine_groups(
    group_by=['PRODUCTKEY'],
    date_col='ORDERDATE',
    group_bounds='global',
    interval_type='day',
    start_timestamp='2010-12-29',
    end_timestamp='2014-01-28' 
)
ds2.preview()

```

## Source Code

{% embed url="https://github.com/rasgointelligence/RasgoTransforms/blob/main/table_transforms/datespine_groups/datespine_groups.sql" %}

