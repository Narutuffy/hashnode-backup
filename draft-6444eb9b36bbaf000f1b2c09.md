---
title: "Resolving High Disk Space Utilization in MongoDB"
slug: resolving-high-disk-space-utilization-in-mongodb
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684820133633/36e7954d-7fea-4ac5-8687-cf76018ed340.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1684820148806/194294d9-8b59-4f14-8ceb-14edd8c4e4d3.png

---

### Problem

According to docs, [Disk utilization % on Data Partition](https://www.mongodb.com/docs/atlas/reference/alert-resolutions/disk-io-utilization/) occurs if the percentage of time during which requests are being issued to any partition that contains the MongoDB collection data meets or exceeds the threshold. The downside of the Disk Utilization being high is the DB cannot process other queries, or requests if it maxes out. This can lead to data loss or inconsistencies.

We have recently been receiving high number of these alerts about high disk usage on MongoDB Atlas for the past two weeks. These are how the metrics looked like in one of the last alerts. From the graph, Disk Util % at 4:30UTC was around 99.86%.

![a set of three screens showing different graphs](https://cdn.hashnode.com/res/hashnode/image/upload/v1682313012185/a0922408-4a74-4813-9457-d44ff5b246c4.png align="center")

Checking the profiler, we can notice a query that's running during the same time, and it takes around `17s` to resolve!

![a screenshot of a computer screen with a number of numbers on it](https://cdn.hashnode.com/res/hashnode/image/upload/v1682316589744/77e990f3-5a5e-4285-9e72-def7d2465829.png align="center")

This query is related to a `KinesisAnalyticsLogs` collection. The collection internally is used to record views of different posts. The above query already makes use of an index and still takes that much time to resolve because of the sheer size of the collection.

![a screen shot of a web page with a number of items on it](https://cdn.hashnode.com/res/hashnode/image/upload/v1682312447723/bcf12123-229c-4247-bdf3-7bab64c757fa.png align="center")

The total number of documents is close to 70 million records, and the index size itself is close to a GB! That seems to be the reason why it takes that much time to resolve. Since this collections was recording analytical data, it was bound to reach this volume at some point. Along with that, from the profiler image, we can see that the query has yielded ~1300 times. According to an answer on [StackOverflow](https://dba.stackexchange.com/questions/82969/mongodb-how-to-reduce-numyields), if a query yields a lot then it was hitting a disk a lot. If we want that query to be faster then the data needs to be set in memory (index).

Upon digging further this query is scheduled to run every 30mins to sync the views. So, we can correlate high query times on the profiler and Disk IOPS peaking at almost the same time.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682317743687/01f4c579-2d4a-4fec-bec3-9c3fec72bc76.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1682317793584/30aa75ff-0132-4b1f-9547-e217f158b5f1.png align="center")

### Solutions

Based on the investigation we came up with two solutions:

**Short Term Solution**

* We are not using older data, deleting records older than a month will reduce the collection size drastically.
    

* We can also add `TTL` to the records in `kinesisAnalyticsLogs` collection ([https://hashnode.com/rix/share/sg4lYYf\_M](https://hashnode.com/rix/share/sg4lYYf_M)). It'll automatically delete the records older than a month, going forward. This will make the index smaller and lead to smaller query time.
    

**Long Term Solution**

Data like views/analytics should not be stored in the same place as the core business data. This collection will keep growing by the minute since it records the views. Some other DB should be used that's more appropriate for it.

### Implementation

We decided to go with the short term solution for now. For starters, we add TTL indexes immediately. With this, all the future records that will be created will be automatically deleted after expiry time. This index can only be set on a field type `date`

```javascript
kinesisAnalyticsLogSchema.index({ dateAdded: 1 }, { expireAfterSeconds: 2592000 });
```

Now, to delete the past records we need to run a script that can delete all the records older than a month.

### TLDR

High disk usage alerts have been received for MongoDB Atlas for the past two weeks. The article investigates the issue and finds that a query related to a metadata collection containing 70 million records is the cause of the problem. The long-term solution suggested is to store metadata in a more appropriate database, while the short-term solution is to delete records older than a month and add TTL records to the collection.