- [SQLFlow frontend guide](#sqlflow-frontend-guide)
  * [video](#video)
    + [1. editor](#1-editor)
      - [1.1 dbvendor](#11-dbvendor)
      - [1.2 sample sql](#12-sample-sql)
      - [1.3 upload](#13-upload)
      - [1.4 visualize](#14-visualize)
      - [1.5 visualize join](#15-visualize-join)
      - [1.6 login](#16-login)
    + [2. schema](#2-schema)
      - [seach schema](#seach-schema)
      - [seach schema path](#seach-schema-path)
      - [collapse all](#collapse-all)
    + [3. setting](#3-setting)
    + [4. job](#4-job)
    + [5. download](#5-download)
    + [6. sqlflow diagram panel](#6-sqlflow-diagram-panel)
    + [How it works](#how-it-works)
    + [Restful API](#restful-api)



# SQLFlow frontend guide

![1](https://user-images.githubusercontent.com/6293752/95873864-e2734400-0da2-11eb-85a9-e46ea43ff5c3.png)

## video
- [SQLFlow get started](images/sqlflow_tutorial_101.gif)
- [SQLFlow visual diagram](images/sqlflow_tutorial_diagram.gif)
- [SQLFlow settings](images/sqlflow_tutorial_settings.gif)
- [SQLFlow search and visualize](images/sqlflow-tutorial-search-table-visualize.gif)

### 1. editor

Enter the sql into the input box, and then choose the database accordingly from the dbvendor menu,
then click visualize or visualize join button to get a clear nice data lineage diagram.


#### 1.1 dbvendor
Select the corresponding database.

#### 1.2 sample sql

Click sample sql button will load the sample SQL for the current selected database 
to the input box.

#### 1.3 upload

upload a single SQL file, or a zip file includes multiple SQL files for processing.
A job will be created for the uploaded file.

#### 1.4 visualize

click visualize button will call [graph interface](#graph), and pass the following parameters：
API used when click the  button.

| name             | value                                     |
| ---------------- | ----------------------------------------- |
| sqltext          | SQL query in the input box                |
| dbvendor         | database from the dbvendor menu           |
| showRelationType | fdd                                       |
| ignoreFunction   | true                                      |

#### 1.5 visualize join

click visualize join button will call [graph interface](#graph), and pass the following parameters:
API used when click the visualize join button.

| name             | value                                     |
| ---------------- | ----------------------------------------- |
| sqltext          | SQL query in the input box                |
| dbvendor         | database from the dbvendor menu           |
| showRelationType | join                                      |
| ignoreFunction   | true                                      |

#### 1.6 login

You have to login in order to upload a SQL or zip file for processing.

### 2. schema

show the schema objects returned by the backend.
you can select a schema/database/table and click the right mouse button to visualize the data lineage of the selected schema object.

![3](https://user-images.githubusercontent.com/6293752/95968181-b8bc2a80-0e3f-11eb-8fc4-1501778fdc74.gif)


- global, green color, means all data lineage information is returned for this object.
- summary, black color, means summary information returned for this object.
- ignore record, orange color, means data lineage is returned without intermediate result.

![image](https://user-images.githubusercontent.com/6293752/95972556-2a4aa780-0e45-11eb-8b61-2126ae9f3e0d.png)

the color of `DATAMART、DBO` is orange, means the returned data linege doesn't include the intermediate result.
the color of `LOAN` is greeen, means all data lineage informaiton is returned.
node in gray means it's not visualized yet.

#### seach schema

your can search `database`,`schema`,`table`,`schema` in the schema tree.

![动画](https://user-images.githubusercontent.com/6293752/116866667-62c59d80-ac3e-11eb-9f5f-c1e35df2f173.gif)

#### seach schema path

you can search `database.schema.table.column ` ,`schema.table`,`table.column` or someting else like it. When you type `dot`, the schema tree will expand the next level.

![2](https://user-images.githubusercontent.com/6293752/116867914-69551480-ac40-11eb-9434-a581de844911.gif)

#### collapse all

![3](https://user-images.githubusercontent.com/6293752/116868278-14fe6480-ac41-11eb-9c9e-c7539a5b4b62.gif)

### 3. setting

![image](https://user-images.githubusercontent.com/6293752/95977385-6da81480-0e4b-11eb-8ec0-cc0de5466701.png)

set the [graph interface](#graph)：

| name             | value                                                           |
| ---------------- | ------------------------------------------------------------ |
| hideColumn       | false/true，hide all columns                      |
| showRelationType | dataflow=true, impact=false, then value is fdd.<br /> dataflow=true, impact=true, the value is fdd,ddi,fdr,frd；<br /> if dataflow=false, impact=true, the value is: fddi,fdr,frd； |
| ignoreRecordSet  | false/true，show intermediate recordset           |
| ignoreFunction   | false/true，show function                         |

### 4. job

![image](https://user-images.githubusercontent.com/6293752/95977128-0b4f1400-0e4b-11eb-8c68-62657380e853.png)

click upload button to create a job by submit SQL text file or zip file including multiple SQL files or connect to a database.

### 5. download

export result to json or png file for download.

### 6. sqlflow diagram panel

select a column to highlight the data flow, click cancel button to cancel the highlight.

![3](https://user-images.githubusercontent.com/6293752/95986233-3ee46b00-0e58-11eb-8ee4-85a7ca5ee0f4.gif)

right mouse click the menu item table lineage or column lineage to show the table or column relation, click cancel to back to the previous state.

![3](https://user-images.githubusercontent.com/6293752/95986541-c336ee00-0e58-11eb-8a45-ad2d904d89ca.gif)

### How it works

SQLFlow frontend communicates with the backend using the RESTFul API [**/sqlflow/generation/sqlflow/graph**](https://github.com/sqlparser/sqlflow_public/blob/master/api/sqlflow_api.md)。
Once it fetchs data from the backend, the frontend will analyze the graph and sqlflow and draw an interactive diagram accordingly.

Click the button or change the value in the setting panel will result in calling different API or call the same API with the different parameters,
and get the different data from the backend consequence.


Reference：[SQLFlow api doc](https://github.com/sqlparser/sqlflow_public/blob/master/api/sqlflow_api.md)


### Restful API

Reference：[SQLFlow api doc](https://github.com/sqlparser/sqlflow_public/blob/master/api/sqlflow_api.md)

<span id="graph">graph interfact：</span> post /sqlflow/generation/sqlflow/graph

