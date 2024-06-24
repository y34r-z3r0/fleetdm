### Deploy

First:
```
docker-compose -f docker-compose-slave.yml up -d
```

Second:
```
docker-compose -f docker-compose-master.yml up -d
```

### MySQL Сlusterisation

Connect to the mysql-master container: (Requires password from environment variable "MYSQL_ROOT_PASSWORD")
```
docker exec -it mysql-master mysqlsh
```
Check if mysql-slave can become part of the cluster:
```
dba.checkInstanceConfiguration('root@localhost:3307')
```
Set a password ("MYSQL_ROOT_PASSWORD")
```
var dbPass = 'password'
```
Initialize cluster:
```
var cluster = dba.createCluster("fleetdm-cluster")
```
Add mysql-slave as a slave node:
```
cluster.addInstance({user: "root", host: "localhost", port: 3307, password: dbPass})
```
Check cluster status:
```
cluster.status()
```
### Secondary commands

Change primary instance:
```
cluster.setPrimaryInstance({host:"localhost", port: 3306"})
```
Get cluster to use `cluster.*` commands:
```
var cluster = dba.getCluster()
```