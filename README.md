TextInputformat in hadoop has a default record delimter as \n , which does not help when you receive JSON records all wrapped up in record in one line.

Example:
[{"m":"","lc":"","tu":""},{"m":"","lc":"","tu":""}]

Default TextInputFormat cannot let you define a hive table on this data because it will show the entire line as one record. Expected input to use this data as a valid table should be as follows :

{"m":"","lc":"","tu":""}
{"m":"","lc":"","tu":""}

This CustomInputFormat handles these oneliner input sets and breaks the records where JSON record finishes.

Usage:

git clone https://github.com/ashishkdubey/hadoop.git
mvn package


In target location you will have a jar - CustomJsonInputFormat-0.0.1-SNAPSHOT.jar ;

The jar can be used in the following manner:

ADD jar s3://bucket/jars/CustomJsonInputFormat.jar ;

CREATE external TABLE testcustomif(
raw string)
ROW FORMAT SERDE 
  'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe' 
STORED AS INPUTFORMAT 
  'com.qubole.custom.inputformat.CustomTextInputFormat' 
OUTPUTFORMAT 
  'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat'
LOCATION
  's3n://bucket//input';


To Query the data:
ADD jar s3://bucket/jars/CustomJsonInputFormat-0.0.1-SNAPSHOT.jar ;

select  * From testcustomif;



