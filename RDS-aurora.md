+++
memflask = True
isdraft = False
+++

# RDS Aurora

## RDS Aurora Basics

Aurora is a cloud only database service designed to provide a distributed, fault-tolerant relational database with self-healing storage and auto-scaling up to 64TB per instance.  It currently comes in two versions, a MySQL compatible system, and a PostgreSQL compatible system.

## RDS Aurora MySQL Basics

-	Amazon’s proprietary fork of MySQL intended to scale up for high concurrency workloads. Generally speaking, individual query performance under Aurora is not expected to improve significantly relative to MySQL or MariaDB, but Aurora is intended to maintain performance while executing many more queries concurrently than an equivalent MySQL or MariaDB server could handle.
-	[Notable new features](http://www.slideshare.net/AmazonWebServices/amazon-aurora-amazons-new-relational-database-engine) include:
	-	Log-structured storage instead of B-trees to improve write performance.
	-	Out-of-process buffer pool so that databases instances can be restarted without clearing the buffer pool.
	-	The underlying physical storage is a specialized SSD array that automatically maintains 6 copies of your data across 3 AZs.
	-	Aurora read replicas share the storage layer with the write master which significantly reduces replica lag, eliminates the need for the master to write and distribute the binary log for replication, and allows for zero-data-loss failovers from the master to a replica. The master and all the read replicas that share storage are known collectively as an **Aurora cluster**. Read replicas can span up to [5 regions](https://aws.amazon.com/about-aws/whats-new/2018/09/amazon-aurora-databases-support-up-to-five-cross-region-read-replicas/).

## RDS Aurora MySQL Tips

-	In order to take advantage of Aurora’s higher concurrency, applications should be configured with large database connection pools and should execute as many queries concurrently as possible. For example, Aurora servers have been tested to produce increasing performance on some OLTP workloads with [up to 5,000 connections](http://www.slideshare.net/AmazonWebServices/amazon-aurora-amazons-new-relational-database-engine/31).
-	[Aurora scales well with multiple CPUs](https://www.percona.com/blog/2016/05/26/aws-aurora-benchmarking-part-2/) and may require a large instance class for optimal performance.
-	The easiest migration path to Aurora is restoring a database snapshot from MySQL 5.6 or 5.7. The next easiest method is restoring a dump from a MySQL-compatible database such as MariaDB. For [low-downtime migrations](https://aws.amazon.com/blogs/aws/amazon-aurora-update-spatial-indexing-and-zero-downtime-patching/) from other MySQL-compatible databases, you can set up an Aurora instance as a replica of your existing database. If none of those methods are options, Amazon offers a fee-based data migration service.
-	You can replicate [from an Aurora cluster to MySQL or to another Aurora cluster](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Aurora.Overview.Replication.MySQLReplication.html). This requires binary logging to be enabled and is not as performant as native Aurora replication.
-	Because Aurora read replicas are the [equivalent of a multi-AZ backup](http://stackoverflow.com/a/32428651/129052) and they can be configured as zero-data-loss failover targets, there are fewer scenarios in which the creation of a multi-AZ Aurora instance is required.

## RDS Aurora MySQL Gotchas and Limitations

- 🔸[Aurora 1.x is based on MySQL 5.6.x](https://news.ycombinator.com/item?id=12415693) with some cherry-picking of later MySQL features. It is missing most 5.7 features as well as some online DDL features introduced in 5.6.17.
- 🔸[Aurora 2.x is based on MySQL 5.7.x](https://aws.amazon.com/about-aws/whats-new/2018/02/amazon-aurora-is-compatible-with-mysql-5-7/)
- Aurora does not support GTID transactions in either the 5.6/Aurora 1.x or the 5.7/Aurora 2.x release lines.
- Aurora maximum cluster size is 64 TB


## RDS Aurora PostgreSQL Basics

- Amazon’s proprietary fork of PostgreSQL, intended to scale up for high concurrency workloads while maintaining ease of use. Currently based on PostgreSQL 9.6.
- Higher throughput (up to 3x with similar hardware).
- Automatic storage scale in 10GB increments up to 64TB.
- Low latency read replicas that share the storage layer with the master which significantly reduces replica lag.
- Point in time recovery.
- Fast database snapshots.

## RDS Aurora PostgreSQL Tips
- Aurora Postgres by default is supposed to utilize high connection rates and for this reason connection pooling must be configured accordingly.
- Because Aurora is based on PostgreSQL 9.6, it lacks features like declarative partitioning or logical replication.

## RDS Aurora PostgreSQL Gotchas and Limitations
- Aurora PostgreSQL falls behind normal RDS when it comes to available versions, so if you need features from the latest PostgreSQL version you might be better off with plain RDS.
- Patching and bug fixing is separate from open source PostgreSQL.

