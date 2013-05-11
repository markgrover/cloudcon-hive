cloudcon-hive
=============

Hive presentation at CloudCon 2013 in San Francisco

Files in the repository
=======================
* [Grover Hive presentation.pdf](https://github.com/markgrover/cloudcon-hive/blob/master/Grover%20Hive%20presentation.pdf): Slides used in the presentation
* [2008.tar.gz](https://github.com/markgrover/cloudcon-hive/blob/master/2008.tar.gz): Flight delay dataset from 2008.
* [airports.csv](https://github.com/markgrover/cloudcon-hive/blob/master/airports.csv): Dataset linking airport codes to their full names. More details in [Introduction](https://github.com/markgrover/bdtc-hive/blob/master/1-Introduction.md) section.
* [README.md](https://github.com/markgrover/cloudcon-hive/blob/master/README.md): This file.

Datasets
========
There are 2 datasets in the repo.

a) The first dataset contains on-time flight performance data from 2008, originally released by [Research and Innovative Technology Administration (RITA)](http://www.transtats.bts.gov/Fields.asp?Table_ID=236). The source of this dataset is http://stat-computing.org/dataexpo/2009/the-data.html. The dataset 

b) The second dataset contains listing of various airport codes in continental US, Puerto Rico and US Virgin Islands. The source of this dataset is http://www.world-airport-codes.com/ The data was scraped from this website and then cleansed to be in its present CSV form.

Hive commands
=============

* On hive shell: Create hive table, *flight_data*:

<pre>
<code>
CREATE EXTERNAL TABLE flight_data(
   year INT,
   month INT,
   day INT,
   day_of_week INT,
   dep_time INT,
   crs_dep_time INT,
   arr_time INT,
   crs_arr_time INT,
   unique_carrier STRING,
   flight_num INT,
   tail_num STRING,
   actual_elapsed_time INT,
   crs_elapsed_time INT,
   air_time INT,
   arr_delay INT,
   dep_delay INT,
   origin STRING,
   dest STRING,
   distance INT,
   taxi_in INT,
   taxi_out INT,
   cancelled INT,
   cancellation_code STRING,
   diverted INT,
   carrier_delay STRING,
   weather_delay STRING,
   nas_delay STRING,
   security_delay STRING,
   late_aircraft_delay STRING
)
ROW FORMAT DELIMITED
FIELDS TERMINATED BY ','
LOCATION '/user/hive/warehouse/flight_data';
</code>
</pre>

* Load the data into the table:

<pre>
<code>
LOAD DATA LOCAL INPATH '2008.csv' OVERWRITE INTO TABLE flight_data;
</code>
</pre>

* Ensure the table got created and loaded fine:

<pre>
<code>
SHOW TABLES;
SELECT
   *
FROM
   flight_data
LIMIT 10; 
</code>
</pre>

* Query the table. Find average arrival delay for all flights departing SFO in January:

<pre>
<code>
SELECT
   avg(arr_delay)
FROM
   flight_data
WHERE
   month=1
   AND origin='SFO';
</code>
</pre>
