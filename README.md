# URL shortner

A scalable python URL shortner with load balancer, distributed database, data replication & partitioning, CAP theorem, caching, disaster recovery, monitoring system, and health checks.


### Caching

Both the load balancer and the URLShortner will have a cache of recent requests. If hits return from cache. This is a performance improvement since requests can be answered from cache without needing to forward to URLShornter or database.

### Process disaster recovery

Monitoring system will monitor and restart dead processes, which includes the load balancer, central DB, and URLShortner.

### Data disaster recovery

If local database is destroyed, URLShortner will restore part/all of it from memory, additionally the central DB will have a replica of all data, and can be used to restore databases.

### Orchestration

Monitor system have a built in orchestration feature, where it can start the cluster including load balancer, central DB, and URLShortners. It can also shutdown the cluster easily. Adding and deleting hosts can be done via a bash script.

### Healthcheck

Monitoring system will periodically ping hosts to check if alive. Additionally the central DB will udp broadcast its heartbeat in a port that the monitor system is listening. System health is displayed in a simple UI of the monitoring system.

### Data replication

Central DB will have a replica of the collective data set, and will be updated periodically if a host received new data.

### Load balancing

Requests will be MD5 hashed and redirected based on hash result. This is a performance improvement as work load from requests is balanced among alive hosts.

### Consistency

Request is hashed to specific host for GET and PUT requests, ensuring requests for the same short resource will always return the same answer of the most recent write.

### Availability

When a hosts fails, requests will be re-hashed and redirected to remaining hosts. This ensures every request will receive a non-error response.

### Partition tolerance

Data will be re-partitioned and sync'ed between hosts via central DB periodically. If cannot partition(due to any reason), system will continue to run without interrpution but consistency may not be guarenteed.

### Data partitioning

Data will be MD5 hashed and the first 2 bytes will be converted into integer. Distribution among alive hosts will be determined by modding the result integer with number of host alive.