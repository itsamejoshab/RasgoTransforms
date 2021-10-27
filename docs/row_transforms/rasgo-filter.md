

# rasgo_filter

Apply one or more column filter to the source

## Parameters

|  Argument  |    Type     |                                             Description                                              |
| ---------- | ----------- | ---------------------------------------------------------------------------------------------------- |
| dimensions | string_list | List of where statements filter the table by. Ex. ["<col_name> = 'string'", "<col_name> IS NOT NULL" |


## Example

```python
source = rasgo.get.data_source(id=363)

source.transform(
  transform_id=transform.id,
  filter_statements=[
      'MONTH = 4',
      'FIPS < COVID_NEW_CASES',
      "YEAR = '2020'"
      'COVID_DEATHS IS NOT NULL'
  ]
).preview()
```

## Source Code

{% embed url="https://github.com/rasgointelligence/RasgoUDTs/tree/main/row_transforms/rasgo-filter/rasgo-filter.sql" %}
