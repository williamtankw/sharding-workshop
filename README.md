# MongoDB Atlas Sharding Workshop

## Introduction
This objective of this workshop is to demostrate the following:
1.  Create a MongoDB sharded cluster in MongoDB Atlas (from a M10 replica set to a 2-shard M30 sharded cluster)
2.  Create a database and collection via MongoDB Compass or Atlas UI
3.  Import documents into the collection via MongoDB Compass
4.  Enable sharding for the databases that we want to shard
5.  Create an index on the shard key (if the sharded collection has data or is not empty)
6.  Shard the collection that we want to shard
7.  Show sharding status via sh.status()
8.  Show shard distribution via db.collection.getShardDistribution()

## Pre-requisites
1.  Install MongoDB Compass and mongosh (MongoDB shell).  Minimally install MongoDB Compass.
    - [Download and install MongoDB Compass](https://www.mongodb.com/try/download/compass)
    - [Download and install mongosh](https://www.mongodb.com/try/download/shell)


## Steps

### 1 - Create a MongoDB sharded cluster in MongoDB Atlas (from a M10 replica set to a 2-shard M30 sharded cluster)
1.  Look and follow my steps on the screen.

### 2 - Create a database and collection via MongoDB Compass or Atlas UI
1.  Look and follow my steps on the screen.
2.  database name = LXDB
3.  collection name = products

### 3 - Import documents into the collection via MongoDB Compass
1.  Look and follow my steps on the screen.
2.  Please use the json file that I have prepare for you and import the file into the collection.

### 4 - Enable sharding for the databases that we want to shard
1.  Look and follow my steps on the screen.
2.  Use the following commands:
```
use LXDB
sh.enableSharding("products")
```

### 5 - Create an index on the shard key (if the sharded collection has data or is not empty)
1.  Look and follow my steps on the screen.
2.  Use the following commands:
```
db.products.createIndex({ “sku”:1 })
```

### 6 - Shard the collection that we want to shard





