---
title: "Run MSSQL inside docker"
date: 2023-05-19T00:35:05+08:00
draft: false
description: ""
---

My machine: Macbook Pro with M1 chip

## Steps

1. Prepare your local docker app. I manuallu installed Docker Desktop.
2. Pull your docker image from [Docker Hub](https://hub.docker.com/_/microsoft-mssql-server) with your terminal.
    ```
    docker pull mcr.microsoft.com/mssql/server:2022-latest
    ```
3. Before you run your docker image, be sure to adjust settings.  
    Settings > Features in development > Beta features  
    Check this option: "Use Rosetta for x86/amd64 emulation on Apple Silicon"
4. Run the command below to run your docker image.
    ```
    docker run 
    -e "ACCEPT_EULA=Y" 
    -e "MSSQL_SA_PASSWORD=SuperStrongPassword" 
    -e "MSSQL_COLLATION=Chinese_Taiwan_Stroke_CI_AS" 
    -p 1433:1433  
    --name YourSqlServer 
    -d  mcr.microsoft.com/mssql/server:2022-latest
    ```
    The option "-p 1433:1433" means to map the 1433 port of the container to your machine. With this option set you can connect the database inside through port 1433.
5. Your container should be running right now. You can use command or database management app to access your database.

## Others

If you want to change `sa` account's password, can use the command below:
```
docker exec -it sql1 /opt/mssql-tools/bin/sqlcmd -S localhost -U SA -P "<YourStrong!Passw0rd>" -Q 'ALTER LOGIN SA WITH PASSWORD="<YourNewStrong!Passw0rd>"'
```


---

Reference:
1. [Microsoft official document](https://learn.microsoft.com/zh-tw/sql/linux/quickstart-install-connect-docker?view=sql-server-ver16&pivots=cs1-bash)
2. [GitHub issue: (Docker apple M1) Invalid mapping of address](https://github.com/microsoft/mssql-docker/issues/668#issuecomment-1412206521)