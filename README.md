# Chaos engineering with ToxiProxy
The idea is to have a cluster, and try to simulate the possible problems that can happen in a cluster deployment: Latencies, network partition etc...

## Deploy the cluster and ToxiProxy

### Prerequisites
Make sure to have: 

- [x] Docker installed
- [x] Docker compose installed
- [x] Internet access (at least to download the needed images)

### Project structure
The project is structured as following:
```
.
│___README.md
│___docker-compose.yml
│       
│
└───core1
│   │___ data
│   │___ backups
│   │___ conf
│   │___ import
│   │___ logs
│   │___ metrics
│   
└───core2
│   │___ data
│   │___ backups
│   │___ conf
│   │___ import
│   │___ logs
│   │___ metrics
│   
└───core3
│   │___ data
│   │___ backups
│   │___ conf
│   │___ import
│   │___ logs
│   │___ metrics
|
└───toxiproxy
│   │___ conf
|   |   |____ toxiproxy.json
│   │___ toxics
|   |   |____ 
│   │___ Dockerfile
```

`docker-compose.yml`: Will contain all the containers configuration
`core{1,2,3}`: Each directory stands for a Neo4j instance.
`core{1,2,3}/data`: All data-related content, such as databases, transactions, cluster-state (if applicable), dumps, and cypher script files (from the neo4j-admin restore command).
`core{1,2,3}/backups`: Directory where the backups will be stored using `neo4j-admin`.
`core{1,2,3}/conf`: The Neo4j configuration settings and the JMX access credentials.
`core{1,2,3}/import`: All the files that the command LOAD uses as sources to import data in Neo4j.
`core{1,2,3}/logs`: The Neo4j log files.
`core{1,2,3}/metrics`: The Neo4j built-in metrics for monitoring the Neo4j DBMS and each individual database.
`toxiproxy`: Directory for toxiproxy Dockerfile and configuration file.

### Deploy the cluster

A `docker-compose` file is ready with a deployment of a 3 cores cluster and a toxiproxy container.
#### Neo4j
`Neo4j` configuration files are located in `./core{1,2,3}/conf/neo4j.conf`. You can fine tune them at your own needs (if you want to add a core, a replica or change the ports).

#### ToxiProxy
For `ToxiProxy`, there is a `Dockerfile` which adds a configuration file to the image when building it, and then start the toxiproxy server with this given configuration.
If you want to change the configuration, you can modify `toxiproxy/conf/toxiproxy.json`.
`ToxiProxy` will start on port `6666` to not conflict with the cluster configuration. Remeber to specify the correct port later for the CLI commands.

#### Run
Before starting `docker-compose`, make sure that these directories are created and that the volumes points on them (the actual configuration will work if you don't change anything):


To start the cluster, simply run:

```
$> docker-compose -f docker-compose.yml up --build
```

### Connect to ToxiProxy container

To use the ToxiProxy cli, you need to be in the container:

```
$> docker exec -i -t toxiproxy sh
```

Once you are connected, go to: 

```
$> cd /go/bin
```

You can execute different commands to activate/deactivate proxies, add/remove toxics, etc.

Please refer to toxiproxy/toxics folder to have all the commands to manage toxiproxy:

- [x] [info](toxiproxy/toxics/info.md)
- [x] [bolt](toxiproxy/toxics/bolt.md)
- [x] [discovery](toxiproxy/toxics/discovery.md)
- [x] [raft](toxiproxy/toxics/raft.md)
- [x] [transaction](toxiproxy/toxics/transaction.md)
