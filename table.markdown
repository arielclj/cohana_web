## Description on Table.yaml

This section describes the schema of the dataset and is used in data compacting
and query processing. [Example File](/example-data/example-table.yaml)

### For data compacting 
When compacting the data, "table.yaml" defines the exact schema of the
dataset in the sense that:

> For column i of the dataset, the i-th 
entry of the schema file describing the meta-data of this column 

Each entry has three attributes, i.e., name, fieldType and dataType. 

1. The name attribute is a unique string representing the name of the respective column. 
2. For fieldType attribute, there are six possible values: "AppKey", "UserKey",
"Action", "ActionTime", "Segment" and "Metric". 

	* "AppKey" indicates that the
respective column contains the unique id of a certain application. As of now,
we only support a single application in a dataset, and so this field is not
used in the query.
	* "UserKey", "Action" and "ActionTime" indicate that the
columns respectively ontain the user id, event and event time. These three
columns compose the primary key of the dataset, and thus they MUST be present
in the dataset. 
	* "Segment" indicates the respective column contains String
values.
	* "Metric" indicates the respective column contains Int32 values.

3. The dataType attribute only has two possible values: String and Int32. 
Note we also treat the ActionTime as Int32, though it can be of the timestamp format.

### For query precessing
Users can add more entries 
(used as cohort selection attributes in the query processing) 
to "table.yaml". 

Each such entry defines an aggregate function, and hence has two additional attributes, "baseField" and "aggregator",
compared with the fields defined in the original schema file. 

* "name" is specified by user and should be different from the name of other entries. 
* "fieldType" and "dataType" of such entries are fixed and
take the value of Metric and Aggregate, respectively.
* "baseField" indicates which column of the original schema will be aggregated and takes the name attribute of the that column as its value. 
* "aggregator" indicates the aggregate function to apply. 
For now, it can be **COUNT**, **SUM**, **RETENTION**. More
aggregate functions are now under development.


## Description on Cube.yaml
For cube.yaml, there are two parts: dimensions and measures. 
For now,dimensions is not used and can be omitted. 
For each entry in the measures part, it contains three attributes: "aggregator", "name" and "tableFieldName". 

* "aggregator" and "name" have the same meaning as the schema file
table.yaml
* "tableFieldName" is the same as the baseField attribute
of the schema file. The entries of the measures part provides the metrics that
can be specified in cohort queris. 

[Example File](/example-data/example-cube.yaml)
