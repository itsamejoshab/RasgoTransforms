

# join

Join a source table with another join table based on certain join keys between table, as well as satisfying a range condition where a date in the main table falls in between two dates in the join_table. **NOTE**: For columns in the Join table with the same name in the source table, only the columns from the join table will be included in the output. 

## Parameters

|   Argument   |   Type    |                                                  Description                                                   |
| ------------ | --------- | -------------------------------------------------------------------------------------------------------------- |
| join_table   | table     | Dataset object to join with the source dataset.                                                                |
| join_type    | join_type | 'LEFT','RIGHT', 'INNER', or None                                                                               |
| join_columns | join_dict | Columns to use for the join. Keys are columns in the source_table and values are on columns in the join_table. |
| anchor_date_col | column | Column in main table that is used as an achor date. |
| start_date_col | column | Column in join_table that indicates the beginning of the join range. |
| end_date_col | column | Column in the join_table that indicates the end of the join range. |

## Example

The range join is used when you only want to pull 1 record from the adjoining table based on whether an anchor date falls between two other dates. For example, consider the following:

**source_table**

| CUSTOMERID | REGIONID | SOURCE_OBS_DATE |
|------------|----------|-----------------|
|          1 |        1 | 2022-02-15      |
|          2 |        1 | 2022-02-15      |
|          3 |        1 | 2022-02-15      |
|          4 |        1 | 2022-02-15      |
|          5 |        1 | 2022-02-15      |

**join_table**

| CUSTOMERID | REGIONID | ACCOUNT_STATUS | EFFECTIVE_START_DATE | EFFECTIVE_END_DATE |
|------------|----------|----------------|----------------------|--------------------|
|          1 |        1 | ACTIVE         | 2000-01-01           | 2015-05-14         |
|          1 |        1 | INACTIVE       | 2015-05-14           | 2021-03-04         |
|          1 |        1 | ACTIVE         | 2021-03-04           | 2999-12-31         |
|          2 |        1 | ACTIVE         | 2000-01-01           | 2999-12-31         |
|          3 |        1 | ACTIVE         | 2000-01-01           | 2020-06-07         |
|          3 |        1 | CANCELLED      | 2020-06-07           | 2999-12-31         |
|          4 |        1 | ACTIVE         | 2011-04-04           | 2016-09-22         |
|          4 |        1 | ON HOLD        | 2016-09-22           | 2019-07-08         |
|          4 |        1 | INACTIVE       | 2019-07-08           | 2999-12-31         |
|          5 |        1 | ACTIVE         | 2022-02-01           | 2022-02-03         |
|          5 |        1 | ON HOLD        | 2022-02-03           | 2022-02-11         |
|          5 |        1 | ACTIVE         | 2022-02-11           | 2022-02-12         |
|          5 |        1 | ON HOLD        | 2022-02-12           | 2022-02-14         |
|          5 |        1 | INACTIVE       | 2022-02-14           | 2999-12-31         |

**Desired result**

| CUSTOMERID | REGIONID | SOURCE_OBS_DATE | ACCOUNT_STATUS |
|------------|----------|-----------------|----------------|
|          1 |        1 | 2022-02-15      | ACTIVE         |
|          2 |        1 | 2022-02-15      | ACTIVE         |
|          3 |        1 | 2022-02-15      | CANCELLED      |
|          4 |        1 | 2022-02-15      | INACTIVE       |
|          5 |        1 | 2022-02-15      | INACTIVE       |

```python
  source_ds = rasgo.get.dataset()
  join_ds = rasgo.get.dataset()

  source_ds.transform(
    transform_name='range_join',
    join_table=join_ds,
    join_type='LEFT',
    join_columns={
      'CUSTOMERID':'CUSTOMERID',
      'REGIONID':'REGIONID'
    },
    anchor_date_col='SOURCE_OBS_DATE',
    start_date_col='EFFECTIVE_START_DATE',
    end_date_col='EFFECTIVE_END_DATE'
  ).preview()
```

## Source Code

{% embed url="https://github.com/rasgointelligence/RasgoUDTs/blob/main/table_transforms/range_join/range_join.sql" %}

