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
