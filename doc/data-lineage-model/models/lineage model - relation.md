## relation

Relation represents the column-level lineage. It includes **one target column, one or more source columns**.

struct definition

```json

    "elementName" : "relation",
    "attributeDefs": [
        {
            "name": "id",
            "typeName": "int",
            "isOptional": false,
            "isUnique": true
        },
        {
            "name": "type",
            "typeName": "string",
            "isOptional": false
        },
        {
            "name": "effectType",
            "typeName": "string",
            "isOptional": true
        },  
        {
            "name": "processId",
            "typeName": "string",
            "isOptional": true
        },  
        {
            "name": "dependency",
            "typeName": "string",
            "isOptional": true
        },  
        {
            "name": "coordinate",
            "typeName": "array<integer>",
            "isOptional": true
        },  
        {
            "name": "target",
            "typeName": "targetElement",
            "isOptional": false
        },
        {
            "name": "source",
            "typeName": "array<sourceElement>",
            "isOptional": false
        } 
    ]
}
```

### id

unique id in the output.

### type

type of the column-lineage, available value: `fdd`, `fdr`, `join`.

Please check [dbobjects_relationship](dbobjects_relationship.md) for the detailed information.

### effectType

This is the SQL statement that generate this relation.
Available values: `select, insert, update, merge_update, merge_insert, create_view, create_table, merge, delete, function, rename_table, swap_table, like_table, cursor,` `trigger,` `create_view`

### dependency

How this reltion convert source column to target column. One of those values: simple, function, expression.

### coordinate

The coordindate of SQL code the build this relation.

### processId

This is the SQL query that build this relation.

### queryHashId

Use `processId` instead.

~~This is the hash code of the SQL query text from which this relation is generated. The `queryHashId` combined with target and source columns can be used to determine a unique relation in the lineage model. It's useful when export the lineage into the data catalog such as the Apache Atals to avoid the duplicated relation been inserted.~~

~~The SQL query with the same queryHashId is treated as the same query. This is usually happened when a SQL query been executed multi times.~~

## target,source element

```json
{
    "elementName" : "target",
    "attributeDefs": [
        {
            "name": "id",
            "typeName": "int",
            "isOptional": false,
            "isUnique": true
        },
        {
            "name": "column",
            "typeName": "string",
            "isOptional": false
        },
        {
            "name": "parent_id",
            "typeName": "int",
            "isOptional": false
        },  
        {
            "name": "parent_name",
            "typeName": "string",
            "isOptional": false
        },
        {
            "name": "source",
            "typeName": "string",
            "isOptional": true
        },
        {
            "name": "clauseType",
            "typeName": "string",
            "isOptional": true
        },
        {
            "name": "coordinate",
            "typeName": "string",
            "isOptional": true
        }  
    ]
}
```

### id

the unique id in the output.

### column

The name of the column.

There is a specific column name: `PseudoRows`, which represents the number of rows in the table/view/resultset. [Check here](dbobjects_relationship.md) for more information.

### parent_id

This is usually the id of a table that this columns belongs.

### parent_name

This is usually the name of a table that this columns belongs.

### source

If the value of source is `system`, this means the column doesn't comes from the SQL query. It's generated by SQLFlow.

### clauseType

Where this column comes from, such as where clause.
