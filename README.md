# Riak-CRDT-Experiment

This tutorial introduces how to reproduce our experiments on Riak CRDTs, and generate histories. 

## Setup Riak

### Install Riak

You should install Riak on at least 3 machines(recommended 5). 

Our experiment is conducted on Riak KV 2.2.3. 

You can follow this official tutorial https://docs.riak.com/riak/kv/2.2.3/setup/index.html. 

### Configure Riak

After installing Riak, you have to configure a Riak cluster involves instructing each node to listen on a non-local interface. 

You can follow this tutorial https://docs.riak.com/riak/kv/2.2.3/using/running-a-cluster/index.html. 

At last, you need to create [bucket types](https://docs.riak.com/riak/kv/2.2.3/using/cluster-operations/bucket-types.1.html) that set the `datatype` bucket parameter. 

You can do this in command line on any machine that runs Riak. 

```
$ riak-admin bucket-type create map311 '{"props":{"datatype":"map","n_val":3,"r":1,"w":1}}}'
$ riak-admin bucket-type activate map311
$ riak-admin bucket-type create map321 '{"props":{"datatype":"map","n_val":3,"r":2,"w":1}}}'
$ riak-admin bucket-type activate map321
$ riak-admin bucket-type create set311 '{"props":{"datatype":"set","n_val":3,"r":1,"w":1}}}'
$ riak-admin bucket-type activate set311
$ riak-admin bucket-type create set321 '{"props":{"datatype":"set","n_val":3,"r":2,"w":1}}}'
$ riak-admin bucket-type activate set321
```



## Configure Bench

Before experiment, you need to configure the IPs and ports for each machine running Riak, in src/main/java/experiment/RiakEnvironment.java. 

```java
static String availableIPs[] = {"172.24.81.1", "172.24.81.2", "172.24.81.3", "172.24.234.4", "172.24.234.5"};
static int port = 8087
```

## Build & Run Bench

```
$ mvn compile && mvn package
$ ./run.sh [map|set] [1|2] [n]
```

For example, 

```shell
$ ./run.sh set 1 1000		#generate 1000 histories for set-r1
$ ./run.sh map 2 10			#generate 10 histories for map-r2
```

