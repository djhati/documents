# CQL tutorial

## Creating keyspace in sicgle datacenter
`create keyspace iot with replication = {'class': 'SimpleStrategy', 'replication_factor':3}`

Observe the keyspace in the cluster by using the nodetool command

`docker exec -it cnode1 nodetool describering iot`

Using nodetool status with keyspace

`docker exec -it cnode1 nodetool status iot`

## Creating keyspace in multi dc cluster

`create keyspace iot with replication = {'class': 'NetworkTopologyStrategy', 'DC1':2,'DC2':1}`


## Create Table

`create table devices(id varchar primary key`

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

## Inserting data

`insert into devices (id) values ('dhrub-mobile');` 
