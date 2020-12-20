---
title: Haystack - Facebook's object storage for photos
date: 2020-12-21 01:30:00 +05:30
---

The following article is the paper summary done by me for Facebook's object storage system used for storing photos known as Haystack[1].

# Introduction:
Haystack is an object storage system optimized for Facebook’s photo application.  It is designed to serve long-tail requests of sharing photos in a large social network. It provides a fault-tolerant and simple storage solution for photos that is incrementally scalable to serve large users and requests.

# Need for Haystack:
* Need for write once, read often, never modified, and seldom deleted storage system.

* Facebook handles one million images at the peak, need to better scale to handle further loads in the future.

* Traditional file systems need to read metadata from disk, which counts for billions of photos results in throughput bottleneck.

* Overall, the previous approach of NFS + NAS + CDN required many disk operations (at least three) to read a single photo. 

# Goals of Haystack:
* High throughput and low latency: Keep all metadata in main memory, thereby requiring at most 1 disk read operation per photo.

* Fault-tolerant: Replicates each photo in geographically distinct regions.

* Cost-effective: Usable storage and read rate normalized is better than NFS using per terabyte storage as a metric.

•	Simple: Quick to build, deploy, and manage in the production environment.

# Design:
Haystack architecture consists of 3 core components: The Haystack Store, Haystack Directory, Haystack Cache. The above figure (taken from the paper) illustrates how the three components fit into the interaction between a client, web server, CDN, and storage system. A typical URL that directs the browser to CDN looks like: http://{CDN}/{Cache}/{Machine id}/{Logical volume, Photo}

## Haystack Directory:
•	Provides mapping from logical volume to physical volume, used by webservers while uploading photos and constructing the image URL for a page request.

•	Load balances writes across logical volumes and reads across physical volumes.

•	Determines whether a photo request has to be handled by the CDN or the Cache.

•	Identifies read only logical volumes either due to operational reasons or they have reached their storage capacity.

## Haystack Cache:
•	Organized as a distributed hash table and uses photo id as key to locating cached data.

•	In case of cache miss fetches the photo from store machine identified in the URL and replies.

•	Caches photo only when

Request comes directly from the user and not CDN, as post CDN caching is usually ineffective as fewer chances of cache hit when it misses CDN.

o	Photo is fetched from write enabled store machine. Used to shelter right enabled machine from reads using Cache as found due to high workload of reads after write.

## Haystack Store:
•	For read the request contains photo id, logical volume & store machine id. Returns photo in case of success else returns an error.

•	Each Store machine manages multiple physical volumes which are stored as a large file consisting of a superblock followed by a sequence of needles, and each volume holds millions of photos.

•	Store machine can access the photo using logical volume id and its file offset.

•	In-memory data structure is maintained for each volume mapping pairs of keys (key, alternate key) to needle’s flags, size, and volume offset. Mapping can be reconstructed after a crash from the volume file before processing new requests.

# Fault tolerance:
•	A background task “pitchfork” periodically checks the health of each store machine. If failure is detected, it marks all logical volumes on the store machine as read only.

•	Another technique “bulk sync” is used to reset data of store machine using the volume files supplied by one of its replicas.

# Optimizations:
•	Compaction: Online operation used to reclaim space used by duplicated and deleted needles, done by copying needles into a new file while skipping duplicate and deleted entries. Post this atomic swapping takes place for the file and in-memory structures.

•	For deleted photos offset value is set to 0, instead of storing it in memory. The machine does not keep track of cookie values in memory and check after reading from the disk. These two techniques help in reducing the memory footprint.

•	Batch upload of photos results in large sequential writes which are better in disks.

# Evaluation:
•	Haystack directory balances reads and writes across store machines using hashing policy.

•	Haystack cache has ~80% hit rate, reducing read request rate for write enabled machines.
•	Haystack store targets the long tail of photo requests while maintaining high throughput and low latency despite random reads.

•	Haystack machines use NVRAM-backed RAID controller for buffering the writes, thus allowing async writes and flushing post that using single fsync reducing write latency.

•	CPU utilization of store machines is low and idle time ranges in 92-96%.

# References:
1. Finding a needle in Haystack: Facebook’s photo storage
Doug Beaver, Sanjeev Kumar, Harry C. Li, Jason Sobel, Peter Vajgel,
Facebook Inc.
