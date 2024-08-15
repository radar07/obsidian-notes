## Designing system that scale

### Horizontal vs Vertical scaling

- **Vertical scaling** - Easy to set up with. Adding a big server with more CPU and more memory (throw more hardware get away with it for a while). Not ideal for production. Prone to Single Point of Failure.
- **Horizontal scaling** - Multiple servers handling the requests through a load-balancer. No single point of failure. If any server goes down the load-balancer will handle traffic by using the other active servers. Easier is servers are *stateless* (subsequent requests should not depend on something stored in the previous request). 

### Fail over strategies

- **Cold standby** - a server with some backup (periodic) remains inactive until it is needed. When something goes wrong in the primary server, the cold standby system is activated.
- **Warm standby** - a server that is partially active by replicating the data fro the primary server constantly.

- **Hot standby** - an active machine will be up always with all the data mirrored (both systems are identical). When the primary machine goes down the traffic can be redirected to the hot standby server.
### Horizontal scaling of Databases

Sharding - multiple shards of databases with each shard having one or more backups
Gotchas:
	- difficult to do joins
	- resharding
	- hotspots (celebrity problems)
	- works best with key/value pairs

### Data lakes
Data lake is a centralized repository that allows us to store all your structured and unstructured data at any scale (AWS S3)
Store data as-is, without having to structure the data, and run different types of analytics.
[Read more](https://aws.amazon.com/what-is/data-lake/)

### ACID compliance
1. Atomicity - Either the entire transactions succeeds or the entire thing fails.
2. Consistency - All database rules are enforced (ex. value can never be null), or the entire transaction is rolled back.
3. Isolation - No transaction is affected by any other transaction that is still in progress.
4. Durability - Once a transaction is committed, it stays, even if the system crashes immediately

### Caching
#### Eviction policies
1. LRU - Least Recently Used
2. LFU - Least Frequently Used
3. FIFO - First In First Out
#### Caching technologies
1. Memcached - In-memory key/value store.
2. Redis - snapshots, replication, transactions, pub/sub, advanced data structures
3. Ncache - made for .NET, Java
4. Ethcache - distributed map for Java
5. ElastiCache - AWS, fully-managed Redis or Memcached

### Content Delivery Network (CDN)

- Geographically distributed network of proxy servers and data centers to provide HA (high-availability)
- Mostly used for static content, such as images or static web pages.
Ex: AWS Cloudfront, Google Cloud CDN, Microsoft Azure CDN, Akamai, Cloudfare.