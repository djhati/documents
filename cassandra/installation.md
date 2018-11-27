# Installation of Cassandra using docker

## Docker

Please install docker and assign 1GB RAM (default in image) per cassandra node you want to create. So if you need to create 4 nodes please start docker with 4GB of RAM or else containers will keep on failing when you try to add new nodes in the cassandra cluster.

## Run single node cassandra cluster in docker

`docker run --name cnode1 -p 9042:9042 -d cassandra:latest`

Now logging into the bash of the container
`docker exec -it cnode1 bash`

Then execute `cqlsh` for starting the sql command window

## Run a multinode cassandra cluster in the same vm/machine

* Execute the first command to start one single node cassandra cluster
 * Then execute the following commands for starting the 2nd node
 
 `docker run --name cnode2 -d -e CASSANDRA_SEEDS="$(docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' cnode1)" cassandra:latest`
  * And finally execute the following commands for starting the 3rd node
  
 `docker run --name cnode3 -d -e CASSANDRA_SEEDS="$(docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' cnode1)" cassandra:latest`
 
 To check if the containers are up please use the  `docker ps` command.
 
## Running nodetool

To check if the cluster is up and running cassandra have an utility called nodetool. Please execute the following command to check if the cluster is up and running.

`docker exec -it cassandra-cluster-node1 nodetool status`

`docker exec -it cassandra-cluster-node1 nodetool ring`

## Creating Multi Datacenter Cassandra Cluster

If you want to create a multi DC Cassandra cluster you need to execute the next series of commands in your machine.

```bash
docker run --name dcnode1 \
-e CASSANDRA_CLUSTER_NAME=DHATI_CS_CLUSTER \
-e CASSANDRA_DC=DC01 \
-e CASSANDRA_RACK=RACK01 \
-e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
-d cassandra:latest
```

```bash
docker run --name dcnode2 \
-e CASSANDRA_CLUSTER_NAME=DHATI_CS_CLUSTER \
-e CASSANDRA_DC=DC01 \
-e CASSANDRA_RACK=RACK02 \
-e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
-e CASSANDRA_SEEDS="$(docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' dcnode1)" \
-d cassandra:latest
```

```bash
docker run --name dcnode3 \
-e CASSANDRA_CLUSTER_NAME=DHATI_CS_CLUSTER \
-e CASSANDRA_DC=DC02 \
-e CASSANDRA_RACK=RACK01 \
-e CASSANDRA_ENDPOINT_SNITCH=GossipingPropertyFileSnitch \
-e CASSANDRA_SEEDS="$(docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' dcnode1)" \
-d cassandra:latest
```

