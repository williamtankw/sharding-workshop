# MongoDB Atlas Sharding Workshop

## Introduction
This objective of this workshop is to demostrate the following:
1.  Create a MongoDB sharded cluster in MongoDB Atlas (from a M10 replica set to a 2-shard M30 sharded cluster)
2.  Create a database and collection via MongoDB Compass or Atlas UI
3.  Import documents into the collection via MongoDB Compass
4.  Enable sharding for the databases that we want to shard
5.  Create an index on the shard key via MongoDB Compass, Atlas UI or mongosh (if the sharded collection has data or is not empty)
6.  Shard the collection that we want to shard
7.  Show sharding status via `sh.status()`
8.  Show shard distribution via `db.collection.getShardDistribution()`
9.  **(OPTIONAL)** Live re-sharding to a new shard key (in this case the hashed index of the same field as the new shard key)
10.  **(OPTIONAL)** Monitoring the re-sharding operation using the `$currentOp` pipeline stage
11.  **(OPTIONAL)** Show sharding status via `sh.status()` and compare the difference to the one before
12.  **(OPTIONAL)** Show shard distribution via `db.collection.getShardDistribution()` and compare the difference to the one before
13.  **(OPTIONAL)** Finishing the re-sharding operation

## Pre-requisites and Notes
1.  Install MongoDB Compass and mongosh (MongoDB shell).  Minimally install MongoDB Compass.
    - [Download and install MongoDB Compass](https://www.mongodb.com/try/download/compass)
    - [Download and install mongosh](https://www.mongodb.com/try/download/shell)
2.  At the time of compiling this lab, MongoDB Atlas version 6.0.6 is used.

## Steps

### 1 - Create a MongoDB sharded cluster in MongoDB Atlas (from a M10 replica set to a 2-shard M30 sharded cluster)
1.  Look and follow my steps on the screen.

### 2 - Create a database and collection via MongoDB Compass or Atlas UI
1.  database name = LXDB
2.  collection name = products

### 3 - Import documents into the collection via MongoDB Compass
1.  Please use the json file that I have prepared for you and import the file into the collection.

### 4 - Enable sharding for the databases that we want to shard
1.  Use the following commands:
```
sh.enableSharding("LXDB")
```

### 5 - Create an index on the shard key via MongoDB Compass, Atlas UI or mongosh (if the sharded collection has data or is not empty)
1.  This is trying to simulate range sharding.  Based on the collection, you might get an unbalanced shard distribution.
2.  Use the following commands:
```
use LXDB
db.products.createIndex({ “sku”:1 })
```

### 6 - Shard the collection that we want to shard
1.  Use the following commands:
```
sh.shardCollection( "LXDB.products", { "sku":1 })
```

### 7 -  Show sharding status via sh.status()
1.  Use the following commands:
```
sh.status()
```
2.  Under `collections` section, you could see the shard key used, the number of chunks in each shard, the range of the min and max of each chunk for the sharded collection.  Please see the following pic as an example:

![pic](pics/sh-status-1.png)

### 8 - Show shard distribution via db.collection.getShardDistribution()
1.  Use the following commands:
```
db.products.getShardDistribution()
```
2.  Using this command, you could see further into the shards on top of what `sh.status()` could provide.  You could see the estimated data per chunk, the estimated docs per chunk under each shard.  You could also see the percentage(%) of data and docs, and average object size on the shards.  Please see the following pic as an example:

![pic](pics/getShardDistribution-1.png)

### 9 - **(OPTIONAL)** Live re-sharding to a new shard key (in this case the hashed index of the same field as the new shard key)
1.  Use the following commands:
```
db.adminCommand({
  reshardCollection: "LXDB.products",
  key: { "sku" : "hashed" }
})
```

### 10 - **(OPTIONAL)** Monitoring the re-sharding operation using the `$currentOp` pipeline stage
1.  Launch another command shell, login to Atlas sharded cluster via mongosh and then use the following commands:
```
db.getSiblingDB("admin").aggregate([
  { $currentOp: { allUsers: true, localOps: false } },
  {
    $match: {
      type: "op",
      "originatingCommand.reshardCollection": "LXDB.products"
    }
  }
])
```
2.  Using this command, you would be able to see the details of the re-sharding process of the sharded collection, more importantly, the `totalOperationTimeElapsedSecs` and `remainingOperationTimeEstimatedSecs`.  Please see the following pic as an example:

![pic](pics/monitoring-resharding-1.png)

### 11 - **(OPTIONAL)** Show sharding status via `sh.status()` and compare the difference to the one before
1.  Use the following commands:
```
sh.status()
```
2.  You should see the difference in the number of chunks in each shard, the min and max of each chunk etc.  Please see the following pic as an example:

![pic](pics/sh-status-2.png)

### 12 - **(OPTIONAL)** Show shard distribution via `db.collection.getShardDistribution()` and compare the difference to the one before
1.  Use the following commands:
```
db.products.getShardDistribution()
```
2.  Using this command, you could see further into the shards on top of what `sh.status()` could provide.  You could see the estimated data per chunk, the estimated docs per chunk under each shard.  You could also see the percentage(%) of data and docs, and average object size on the shards.  Please see the following pic as an example:

![pic](pics/getShardDistribution-2.png)

### 13 - **(OPTIONAL)** Finishing the re-sharding operation







