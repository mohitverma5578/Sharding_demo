# Sharding_demo
**Step 1: Configure config server replica set.**

sudo docker exec -it mongocfg1 bash -c "echo 'rs.initiate({_id: \"mongors1conf\",configsvr: true, members: [{ _id : 0, host : \"mongocfg1\" },{ _id : 1, host : \"mongocfg2\" }, { _id : 2, host : \"mongocfg3\" }]})' | mongo"

**Step 2: Check config server status.**

docker exec -it mongocfg1 bash -c "echo 'rs.status()' | mongo"

**Step 3: Building first Shard ReplicaSet.**

docker exec -it mongo1 bash -c "echo 'rs.initiate({_id : \"mongors1\", members: [{ _id : 0, host : \"mongo1\" },{ _id : 1, host : \"mongo2\" },{ _id : 2, host : \"mongo3\" }]})' | mongo"

**Step 4: Building Second Shard ReplicaSet.**

docker exec -it mongo4 bash -c "echo 'rs.initiate({_id : \"mongors2\", members: [{ _id : 0, host : \"mongo4\" },{ _id : 1, host : \"mongo5\" },{ _id : 2, host : \"mongo6\" }]})' | mongo"

**Step 5: Check Shards status.**

docker exec -it mongo1 bash -c "echo 'rs.status()' | mongo"

**Step 6: Introducing first shards to router.**

docker exec -it mongos1 bash -c "echo 'sh.addShard(\"mongors1/mongo1\")' | mongo "

**Step 7: Introducing second shards to router.**

docker exec -it mongos2 bash -c "echo 'sh.addShard(\"mongors2/mongo2\")' | mongo "

**Step 8: To check Shard status.**

docker exec -it mongos1 bash -c "echo 'sh.status()' | mongo "

**Step 9: Now creating a new database.**

docker exec -it mongo1 bash -c "echo 'use someDb' | mongo"

**Step 10: Enabling Sharding in new database.**

docker exec -it mongos1 bash -c "echo 'sh.enableSharding(\"someDb\")' | mongo "

**Step 11: Creating a collection in sharding database.**

docker exec -it mongo1 bash -c "echo 'db.createCollection(\"someDb.someCollection\")' | mongo "

**Step 12: Sharding the new collection.**

docker exec -it mongos1 bash -c "echo 'sh.shardCollection(\"someDb.someCollection\", {\"someField\" : \"hashed\"})' | mongo "
