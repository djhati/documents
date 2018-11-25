# Installation of Cassandra using docker


## Run single node cassandra cluster in docker

`docker run --name cassandra-cluster-node1 -p 9042:9042 -d cassandra:latest`

Now logging into the bash of the container
`docker exec -it cassandra-cluster-node1 bash`

Then execute `cqlsh` for starting the sql command window

## Run a multinode cassandra cluster in the same vm/machine

* Execute the first command to start one single node cassandra cluster
 * Then execute the following commands for starting the 2nd node
 
 `docker run --name cassandra-cluster-node2 -d --link cassandra-cluster-node1:cassandra cassandra:latest`
  * And finally execute the following commands for starting the 3rd node
  
 `docker run --name cassandra-cluster-node3 -d --link cassandra-cluster-node2:cassandra cassandra:latest`
 
## Running nodetool

`docker exec -it cassandra-cluster-node1 nodetool status`

`docker exec -it cassandra-cluster-node1 nodetool ring`
