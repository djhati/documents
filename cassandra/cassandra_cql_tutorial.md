# CQL tutorial

## Creating keyspace in single datacenter

```sql
create keyspace iot with replication = {'class': 'SimpleStrategy', 'replication_factor':3}
```

Observe the keyspace in the cluster by using the nodetool command

`docker exec -it cnode1 nodetool describering iot`

Using nodetool status with keyspace

`docker exec -it cnode1 nodetool status iot`

## Creating keyspace in multi dc cluster
```sql
create keyspace iot with replication = {'class': 'NetworkTopologyStrategy', 'DC1':2,'DC2':1};
```
## Alter Keyspace

```sql
alter keyspace iot with replication = {'class: 'NetworkTopologyStrategy', 'DC2': 2};
```

## Drop Keyspace

```sql
drop keyspace iot;
```

## Use Keyspace

```sql
use devices;
```

## Create Table

```sql
create table devices(id varchar primary key);`\
```
 or

e.g.
```sql
create table moviedb.movies(
name text,
release_date timestamp,
rating int,
lead_actor text,
runtime_in_minutes int,
description text,
director text,
genre text,
primary key ((name, release_date), rating, runtime_in_minutes, genre)
)with clustering order by (rating DESC, runtime_in_minutes DESC);
```

### create table with properties
```sql
create table iot.devices(id varchar primary key) with comment='Table for IoT Devices';
```

Other table properties
* comment
* caching(keys, rows_per_partition)
* read_repair_chance
* dclocal_read_repair_chance
* default_time_to_live
* gc_grace_seconds
 
## Alter Table

### Adding columns
```sql
alter table iot.devices add type varchar;
```

### Removing columns
```sql
alter table iot.devices drop type;
```

## Remove Data 

```sql
truncate iot.devices;
```

## Drop Table

```sql
drop iot.devices;
```


## Consistency Level
To check the consistency level use the following command

`consistency`

To change the consistency level to QUORUM

`consistency QUORUM;`

To change the consistency level to ALL;

`consistency ALL;`

## Enabling Tracing

`tracing on`

E.g.

```
Tracing session: 493b9120-f148-11e8-9912-6d2c86545d91

 activity                                                                                      | timestamp                  | source     | source_elapsed | client
-----------------------------------------------------------------------------------------------+----------------------------+------------+----------------+-----------
                                                                            Execute CQL3 query | 2018-11-26 06:55:40.342000 | 172.17.0.2 |              0 | 127.0.0.1
     Parsing insert into devices (id) values ('anon-device-02'); [Native-Transport-Requests-1] | 2018-11-26 06:55:40.345000 | 172.17.0.2 |           4723 | 127.0.0.1
                                             Preparing statement [Native-Transport-Requests-1] | 2018-11-26 06:55:40.346000 | 172.17.0.2 |           5164 | 127.0.0.1
                               Determining replicas for mutation [Native-Transport-Requests-1] | 2018-11-26 06:55:40.346000 | 172.17.0.2 |           5514 | 127.0.0.1
                                                      Appending to commitlog [MutationStage-2] | 2018-11-26 06:55:40.346000 | 172.17.0.2 |           5858 | 127.0.0.1
                                                  Adding to devices memtable [MutationStage-2] | 2018-11-26 06:55:40.347000 | 172.17.0.2 |           6041 | 127.0.0.1
         Sending MUTATION message to /172.17.0.4 [MessagingService-Outgoing-/172.17.0.4-Small] | 2018-11-26 06:55:40.347000 | 172.17.0.2 |           6313 | 127.0.0.1
         Sending MUTATION message to /172.17.0.3 [MessagingService-Outgoing-/172.17.0.3-Small] | 2018-11-26 06:55:40.347000 | 172.17.0.2 |           6644 | 127.0.0.1
            MUTATION message received from /172.17.0.2 [MessagingService-Incoming-/172.17.0.2] | 2018-11-26 06:55:40.350000 | 172.17.0.3 |            641 | 127.0.0.1
            MUTATION message received from /172.17.0.2 [MessagingService-Incoming-/172.17.0.2] | 2018-11-26 06:55:40.351000 | 172.17.0.4 |            693 | 127.0.0.1
                                                      Appending to commitlog [MutationStage-1] | 2018-11-26 06:55:40.352000 | 172.17.0.4 |           2236 | 127.0.0.1
                                                  Adding to devices memtable [MutationStage-1] | 2018-11-26 06:55:40.352000 | 172.17.0.4 |           2607 | 127.0.0.1
                                           Enqueuing response to /172.17.0.2 [MutationStage-1] | 2018-11-26 06:55:40.353000 | 172.17.0.4 |           2832 | 127.0.0.1
 Sending REQUEST_RESPONSE message to /172.17.0.2 [MessagingService-Outgoing-/172.17.0.2-Small] | 2018-11-26 06:55:40.353000 | 172.17.0.4 |           3216 | 127.0.0.1
    REQUEST_RESPONSE message received from /172.17.0.4 [MessagingService-Incoming-/172.17.0.4] | 2018-11-26 06:55:40.354000 | 172.17.0.2 |          13820 | 127.0.0.1
                                                      Appending to commitlog [MutationStage-1] | 2018-11-26 06:55:40.355000 | 172.17.0.3 |           5525 | 127.0.0.1
                                                  Adding to devices memtable [MutationStage-1] | 2018-11-26 06:55:40.355000 | 172.17.0.3 |           5845 | 127.0.0.1
                                           Enqueuing response to /172.17.0.2 [MutationStage-1] | 2018-11-26 06:55:40.355000 | 172.17.0.3 |           6027 | 127.0.0.1
 Sending REQUEST_RESPONSE message to /172.17.0.2 [MessagingService-Outgoing-/172.17.0.2-Small] | 2018-11-26 06:55:40.357000 | 172.17.0.3 |           7710 | 127.0.0.1
    REQUEST_RESPONSE message received from /172.17.0.3 [MessagingService-Incoming-/172.17.0.3] | 2018-11-26 06:55:40.358000 | 172.17.0.2 |          17184 | 127.0.0.1
                                 Processing response from /172.17.0.4 [RequestResponseStage-2] | 2018-11-26 06:55:40.365000 | 172.17.0.2 |          24649 | 127.0.0.1
                                 Processing response from /172.17.0.3 [RequestResponseStage-2] | 2018-11-26 06:55:40.365000 | 172.17.0.2 |          24857 | 127.0.0.1
                                                                              Request complete | 2018-11-26 06:55:40.367130 | 172.17.0.2 |          25130 | 127.0.0.1

```

## Cassandra DataTypes


### Basic DataTypes

#### Numeric (Integer or Float)
bigint, decimal, double, float, int, smallint, tinyint, varint

#### String
ascii, text, varchar

#### Date or Time Related
date, duration, time, timestamp, timeuuid

#### UUID
timeuuid, uuid

#### Counter
counter

#### Large Objects
blob

#### IP Address
inet

### Collection DataTypes

Maps, Sets and Lists

### Tuples

### User Defined Types

## Partition Keys and Clustering Columns

In cassandra the partition key is responsible for storing the data in nodes. There are three ways to get the parition key from the primary key.

* The most simple way is when the primary key will have one single column.
 e.g.
 ```sql
create table iot.devices(id varchar primary key)';
```
In such case the column in the primary key is the partition key

* The second type is when the primary key will contain several columns in a single bracket. e.g.
`PRIMARYKEY (id, song_order,...)`

 Here the first key is the partition key and the rest are the clustering columns. Clustering is a storage engine process that sorts data within the partition. This type of keys are called/was called compound keys.

* The third and the mostly used case is `PRIMARY KEY ((block_id, breed), color, short_hair)`

  In this case there are two sets of brackets, one inside the another. The inner brackets containing columns are the combined partition keys and the columns outside that are the clustering columns. This type of partition keys are called composite partition key.
  
  Please note in the where clause you can use only either partition keys or clustering columns. It will throw you an error if you try to filter using any other column.


## Inserting data

e.g.
```sql
insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('The Shawshank Redemption', '1995-02-17', 9, 'Tim Robbins', 142, 'Frank Darabont', 'Drama');

insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('The Godfather', '1972-08-24', 9, 'Marlon Brando', 175, 'Francis Ford Coppola', 'Drama');

insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('The Godfather', '1975-05-15', 9, 'Al Pacino', 202, 'Francis Ford Coppola', 'Drama');

insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('The Dark Knight', '2008-07-24', 9, 'Christian Bale', 152, 'Chris Nolan', 'Action');

insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('12 Angry Men', '1957-04-01', 8, 'Henry Fonda', 96, 'Sidney Lumet', 'Crime');
```

## Selecting Data

```sql
select * from moviedb.movies where name='The Godfather';

select * from moviedb.movies where name='The Godfather' allow filtering;;

select * from moviedb.movies where name='The Godfather' and release_date = '1975-05-15';

select * from moviedb.movies where rating<9 allow filtering;
```

## Delete Data

Deleting the wrongly updated column
```sql
delete from moviedb.movies
where name='The Godfather' and release_date = '1975-05-15';
```
Inserting again the correct column
```sql
insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('The Godfather: Part II', '1975-05-15', 9, 'Al Pacino', 202, 'Francis Ford Coppola', 'Drama');
```

## Inserting data with TTL
```sql
insert into moviedb.movies (name, release_date, rating, lead_actor, runtime_in_minutes, director, genre) values
('12 Angry Men', '1957-04-01', 8, 'Henry Fonda', 96, 'Sidney Lumet', 'Crime') using ttl 120;
```

Selecting the data with the ttl value in cassandra
```sql
select lead_actor, ttl(lead_actor) from moviedb.movies;
```
## Updating the data with TTL
```sql
update moviedb.movies using ttl 300 
set director = 'Christopher Nolan'
where name='The Dark Knight' 
and release_date='2008-07-24'
and rating = 9
and runtime_in_minutes = 152
and genre='Action';
```
## Writetime in Cassandra
In cassandra it is possible to check the last update time of the table

```sql
select name, writetime(director) from moviedb.movies;
```

## Tombstones

Cassandra's processes for deleting data are designed to improve performance, and to work with Cassandra's built-in properties for data distribution and fault-tolerance.

Cassandra treats a delete as an insert or upsert. The data being added to the partition in the DELETE command is a deletion marker called a tombstone. The tombstones go through Cassandra's write path, and are written to SSTables on one or more nodes. The key difference feature of a tombstone: it has a built-in expiration date/time. At the end of its expiration period (for details see below) the tombstone is deleted as part of Cassandra's normal compaction process.

## Create and query Tables with Counter

```sql
create table moviedb.ratings(
name text,
year_of_release int,
rating_count counter,
primary key(name, year_of_release));
```

You can only use update statements in tables with Counters

```sql
update moviedb.ratings 
set rating_count= rating_count + 1
where name ='The Shawshank Redemption' 
and year_of_release=1995 ;
```

## Handling timeseries data

```sql
create table moviedb.website_visits(
page_id text,
movie_name text,
view_id timeuuid,
primary key(page_id, view_id, movie_name)
);
```
This case the first one is the only primary key rest are clustering columns.

When you don't need to know the exact value but just want to make sure its unique and associated with current date and time
```sql  
insert into moviedb.website_visits(page_id, movie_name, view_id) values ('a1dbc23r', 'The Shawshank Redemption', now());
```
for querying data from columns with type timeuuid cql provide some inbuilt functions
e.g
```sql
select page_id, movie_name, dateOf(view_id) from moviedb.website_visits;

select page_id, movie_name, unixTimestampOf(view_id) from moviedb.website_visits;

select dateOf(view_id)
from moviedb.website_visits
where page_id='a1dbc23r'
and view_id>=maxTimeuuid('2018-11-01 00:00+0000')
and view_id <minTimeuuid('2018-11-27 00:00+0000');
```

Using a timeuuid as a clustering key is very effective method to handle timeseries data. 
In multiple node with multiple write scenario this will handle perfectly the data arriving in order and the same while storing the data even if it is delayed by failure of nodes.


## Bucketing timeseries data

In cassandra you can store a maximum of 2 billion cells(rows X columns) inside a single partition key.
All data which belongs to a single partition must fit in a single node in a cluster.
In Iot scenario where data of billions of devices are stored with a bad partition key we might reach that limit very quickly.

```sql
create table iot.moving_device_data(
year int,
week_in_year int,
reading_id timeuuid,
device_type text,
device_id text,
data text,
primary key((year, week_in_year),reading_id)
)with clustering order by (reading_id desc);

insert into iot.moving_device_data(year, week_in_year, reading_id, device_type, device_id, data) values
(2018, 47, now(), 'Commercial Car', 'abc12e6re', $${"AccelerationX":1.1523132,"AccelerationY":8.338104,"Altitude":819,"AccelerationZ":3.69458,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:24.099Z","OrientationZ":-0.13475741,"OrientationY":-0.30233166,"Longitude":77.615204,"OrientationX":-1.13624},{"AccelerationX":1.3079376,"AccelerationY":8.217194,"Altitude":819,"AccelerationZ":3.6526947,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:25.019Z","OrientationZ":-0.1795138,"OrientationY":-0.34385014,"Longitude":77.615204,"OrientationX":-1.1296704}$$);

insert into iot.moving_device_data(year, week_in_year, reading_id, device_type, device_id, data) values
(2018, 47, now(), 'Commercial Car','abc12e6re', $${"AccelerationX":1.2205505,"AccelerationY":8.268677,"Altitude":819,"AccelerationZ":3.8214874,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:25.039Z","OrientationZ":-0.13874406,"OrientationY":-0.30915084,"Longitude":77.615204,"OrientationX":-1.1190871},{"AccelerationX":1.190628,"AccelerationY":8.3506775,"Altitude":819,"AccelerationZ":3.4515839,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:25.059Z","OrientationZ":-0.14570934,"OrientationY":-0.33216998,"Longitude":77.615204,"OrientationX":-1.1586124}$$);

insert into iot.moving_device_data(year, week_in_year, reading_id, device_type, device_id, data) values
(2018, 47, now(), 'Commercial Car','abc12e6re', $${"AccelerationX":1.2444916,"AccelerationY":8.343506,"Altitude":819,"AccelerationZ":3.63414,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:25.079Z","OrientationZ":-0.12858334,"OrientationY":-0.32992816,"Longitude":77.615204,"OrientationX":-1.1393306},{"AccelerationX":1.1840363,"AccelerationY":8.483551,"Altitude":819,"AccelerationZ":3.349823,"Latitude":12.915812,"VehicleSpeed":1.98,"_time":"2018-09-02T07:04:25.099Z","OrientationZ":-0.12846963,"OrientationY":-0.33975595,"Longitude":77.615204,"OrientationX":-1.1741878}$$);
```


## Working with Complex Data Types

### Set
```sql
create table iot.things(
id text,
fields set<text>,
device_id text,
primary key(id, device_id));

insert into iot.things(id,device_id, fields) 
values ('demo-thing-id-01', 'demo-device-id-01', {'Latitude', 'Longitude', 'AccelerationX', 'AccelerationY'});
```

Adding elements in the existing set
```sql
update iot.things
set fields = fields + {'Altitude'}
where id = 'demo-thing-id-01' 
and device_id='demo-device-id-01';
```
Remove items from set
```sql
update iot.things
set fields = fields - {'AccelerationX', 'AccelerationY'}
where id = 'demo-thing-id-01' 
and device_id='demo-device-id-01';
```
Delete all items from set
```sql
update iot.things
set fields = {}
where id = 'demo-thing-id-01' 
and device_id='demo-device-id-01';
```

### Map

Now we have stored only the the sensors present in the thing, but we don't know which datatype they belong. To add the information we will use a map of sensor name and its data type.

```sql
create table iot.things(
id text,
fields map<text, text>,
device_id text,
primary key(id, device_id));

insert into iot.things(id,device_id, fields) 
values ('demo-thing-id-01', 'demo-device-id-01', {'Latitude':'Decimal', 'Longitude':'Decimal', 'AccelerationX':'Decimal', 'AccelerationY':'Decimal', 'Engine RPM':'Integer', 'OrientationX':'Radian'});
```

Adding and deleting elements from the map is same as set. Please check the official cassandra website for more details on this subject.

### Tuples
We also need to store precision of decimal values in fields. So it contains more information which cannot be set achieved only through the map data type. We need a datatype which can contain more values. Though here in the example we have used tuple with two values it can be used for more than 2 values. This is also example opf nesting of complex data types.

Here you might have noticed we have used the frozen keyword in this case as cassandra stored the nested types as blob internally so they cannot be updated partially. frozen explicitly says the same. If frozen keyword is used against a complex data type then the data type is updated completely even if there is a partial modification.

```sql
create table iot.things(
id text,
fields map<text, frozen<tuple<text,text>>>,
device_id text,
primary key(id, device_id));


insert into iot.things(id,device_id, fields) 
values ('demo-thing-id-01', 'demo-device-id-01', {'Latitude':('Decimal','4'), 'Longitude':('Decimal','4'), 'AccelerationX':('Decimal','4'), 'AccelerationY':('Decimal','4'), 'Engine RPM':('Integer', 'NA'), 'OrientationX':('Radian', 'NA')});
```
