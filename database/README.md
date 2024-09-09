##   Mongo Sharding:

To shard the sanfrancisco.company_name collection using the _id field, follow these steps::



### 1. Enable Sharding on the Database:
Once MongoDB is upgraded, connect to your instance and enable sharding on the sanfrancisco database:
bash
```
mongos> use admin
mongos> sh.enableSharding("sanfrancisco")
```

###  2.Shard the company_name Collection:
Initiate sharding on the sanfrancisco.company_name collection using the _id field as the shard key:

```
mongos> sh.shardCollection("sanfrancisco.company_name", { "_id": 1 })

```

 ### 3.Add replicaset_2 as a Shard:
If replicaset_2 is already initialized, add it as a shard:
```
mongos> sh.addShard("replicaset_2/mongo-replicaset2-host:port")
```


### 4.Verify Sharding:
Check the sharding status to ensure the collection is being properly sharded and balanced across replicaset_1 and replicaset_2:
```
mongos> sh.status()

```
### 5. Monitor the Balancing Process:
MongoDB’s balancer should automatically begin distributing data across the shards. You can manually start the balancer if needed:
```
sh.startBalancer()

```

Monitor the balancer’s progress using:

```
mongos> use config
mongos> db.locks.find({ _id: "balancer" })

```
