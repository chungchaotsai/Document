## 建立網路
多個container 間利用網路連結。目標用mssql server client container 連線至 mssql server container

## 建立MSSQL, 對network demo 新增 A record: sqlsvr, 對於container 而言, 各有各的ip,

`docker run -e "ACCEPT_EULA=Y" -e "MSSQL_SA_PASSWORD=Password" --network demo --network-alias mssqldev -p 1433:1433 --name mssqldev --hostname mssqldev -d mcr.microsoft.com/mssql/server:2022-latest`

## 建立client
(使用network demo, 客製化image方法:https://www.dataset.com/blog/create-docker-image/)
`docker run -d -t --network demo --name mssql-tools -d mcr.microsoft.com/mssql-tools:latest`

從client container 連線mssql, 方法目前想到三個, 
- 1. 連線本機ip,不能用localhost, container 無法解析localhost, 亦無法用127.0.0.1 應該是因為對於container 而言, 127.0.0.1 是container本身
- 2. sqlsvr container 的id, docker inspect mynginx | grep IPAddress
- 3. 利用network dns 查找, 如以上network:demo, 

`docker exec -it mssql-tools bash
 sqlcmd -S sqlsvr -U SA -P "Password";
`
 
## 從本地端連線sqlserver(使用mssqlserver management) 
127.0.0.1 SA/Password
