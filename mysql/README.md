### Start
Start MySQL, Adminer and phpMyAdmin, each in one container:
```console
// Either ...
michael@lenovo:~$ echo "SHARED_DIRECTORY_PATH=$HOME" > .env
// ... or copy the template and edit your new .env manually
michael@lenovo:~$ cp .env.template .env
// Then you can start the containers
michael@lenovo:~$ sudo docker-compose up -d
```

### Access MySQL in terminal
```console
// With MySQL client installed
michael@lenovo:~$ mysql -h127.0.0.1 -P3308 -uroot -proot

// With Docker in Bash and CMD
michael@lenovo:~$ sudo docker run \
    -it \
    --network network_mysql_and_adminer \
    --rm \
    mysql \
    mysql -hdb -P3306 -uroot -proot

// With Docker in Git Bash
michael@lenovo:~$ winpty docker run \
    -it \
    --network network_mysql_and_adminer \
    --rm \
    mysql \
    mysql -hdb -P3306 -uroot -proot

// Or login to your Docker instance via docker exec -it [YOUR CONTAINER NAME] bin/bash ...
michael@lenovo:~$ sudo docker exec -it docker-mysql-local_db_1 bin/bash
// ... and login to MySQL via mysql -u USERNAME -p.
root@c0a33c5aed72:/# mysql -u root -proot
```

### Access in Adminer
Open in a browser http://localhost:8182/ and use this credentials:
- Server: `db`
- User: `root`
- Password: `root` (Adminer 4.6.3 and newer does not support accessing a database without a password)

### Access in phpMyAdmin
Open in a browser http://localhost:8183/ and use this credentials:
- Server: `db`
- User: `root`
- Password: `root`

### Usage in Symfony's .env.local
```
DATABASE_URL=mysql://root:root@127.0.0.1:3306/my_database_name
```

### Import a database dump
`docker-compose.yaml` defines a shared folder shared between the host machine 
and the MySQL container. So you can:
```console
// Move the exported sql file in the shared folder defined in docker-compose.yaml
michael@lenovo:~$ mv my-dump.sql /your-configured-path/.docker-mysql-local/

// Login to MySQL
michael@lenovo:~$ mysql -h127.0.0.1 -P3306 -uroot -proot

// While in MySQL CLI, import the sql file via source /path/to/file.sql.
mysql> use my-database-name
mysql> source /var/docker-mysql-local/my-dump.sql

// Windows
> mysql -uroot -proot -P 3306 my_db_name < C:\Users\John\my_name.sql
```
