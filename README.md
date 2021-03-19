## Docker MySQL master-slave replication 

MySQL master-slave replication with using Docker. 

## Run

To run this examples you will need to start containers with "docker-compose" 
and after starting setup replication. See commands inside ./build.sh. 

#### Create 2 MySQL containers with master-slave row-based replication 

```
./build.sh
```

#### Make changes to master

```

connect 
	user: root
	pass: 111
	db: mydb

create table:

create table user (
  id int primary key,
  name varchar(255)
);


insert into user (id, name) values (1, 'Levik');
```

#### Read changes from slave

```
select * from user;
```

### Add new column 

```

ALTER TABLE user ADD COLUMN age int;
insert into user (id, name, age) values (2, 'Levik1', 12);
```


#### Read changes from slave

```
select * from user;
```

### Drop new column 

```
ALTER TABLE user DROP age;
```

#### Read changes from slave

```
select * from user;
```

## Troubleshooting

#### Check Logs

```
docker-compose logs
```

#### Start containers in "normal" mode

> Go through "build.sh" and run command step-by-step.

#### Check running containers

```
docker-compose ps
```

#### Clean data dir

```
rm -rf ./master/data/*
rm -rf ./slave1/data/*
rm -rf ./slave2/data/*
```

#### Run command inside "mysql_master"

```
docker exec mysql_master sh -c 'mysql -u root -p111 -e "SHOW MASTER STATUS \G"'
```

#### Run command inside "mysql_slave"

```
docker exec mysql_slave sh -c 'mysql -u root -p111 -e "SHOW SLAVE STATUS \G"'
```

#### Enter into "mysql_master"

```
docker exec -it mysql_master bash
```

#### Enter into "mysql_slave"

```
docker exec -it mysql_slave bash
```
