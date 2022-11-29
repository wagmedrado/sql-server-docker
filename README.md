# sql-server-docker
Sql Server development environment with Docker

## Steps
1 - Create ```docker-compose.yaml``` file

```
version: '3.6'
services:
  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      SA_PASSWORD: "secret!"
      ACCEPT_EULA: "Y"
      MSSQL_PID: "Developer"
    ports:
      - "1433:1433"
```
2 - Execute command in bash:

```sh
docker-compose up -d
```

## Resore Backup

Replace ```[container id]``` and ```MYDB```

```
docker exec -it [container id] mkdir /var/opt/mssql/backup

docker cp MYDB.bak [container id]:/var/opt/mssql/backup

docker exec -it [container id] /opt/mssql-tools/bin/sqlcmd -S localhost \
   -S localhost -U SA -P 'secret!' \
   -Q 'use [master]'

docker exec -it [container id] /opt/mssql-tools/bin/sqlcmd -S localhost \
   -S localhost -U SA -P 'secret!' \
   -Q 'alter database [MYDB] set restricted_user with rollback immediate'

docker exec -it [container id] /opt/mssql-tools/bin/sqlcmd -S localhost \
   -S localhost -U SA -P 'secret!' \
   -Q 'RESTORE DATABASE [MYDB] FROM DISK = "/var/opt/mssql/backup/MYDB.bak" WITH FILE = 1, MOVE "mydb" TO "/var/opt/mssql/data/MYDB.mdf", MOVE "mydb_log" TO "/var/opt/mssql/data/MYDB_log.ldf", NOUNLOAD, REPLACE, STATS = 5'

docker exec -it [container id] /opt/mssql-tools/bin/sqlcmd -S localhost \
   -S localhost -U SA -P 'secret!' \
   -Q 'ALTER DATABASE [MYDB] SET MULTI_USER'

docker exec -it [container id] rm /var/opt/mssql/backup/MYDB.bak
```
