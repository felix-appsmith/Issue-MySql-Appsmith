
# Self-hosted MariaDB Too many [connections](https://discord.com/channels/725602949748752515/1022081904087941130)
"Plugin failed to connect to data source with error: io.r2dbc.spi.R2dbcNonTransientResourceException: [1040] Too many connections"

With this our instance is not working, and all queries fails.  Before there has been no connection issue whatsoever.

--------------------------
### Steps to follow to reproduce the issues.
1. Start the appsmith [self-host.](https://docs.appsmith.com/getting-started/setup/installation-guides/docker)
2. Start a [docker-compose](mysql/docker-compose.yaml) with MySQL.
```yaml
version: '3.3'
services:
  db:
    image: mysql:5.7
    command:
        --max_connections=2
    restart: always
    environment:
      MYSQL_DATABASE: 'db'
      MYSQL_USER: 'user'
      MYSQL_PASSWORD: 'password'
      MYSQL_ROOT_PASSWORD: 'password'
    ports:
      - '3306:3306'
    expose:
      - '3306'
    volumes:
      - my-db:/var/lib/mysql
volumes:
  my-db:
```
3. Up the docker container: `sudo docker-compose up `

` Note: To make the connections,use ngrok.`

4. install [ngrok](https://ngrok.com/download)

5. Run this command: ` ngrok tcp 3306`

6. Example how to get Ngrok host and port to make connections.
```console
appsmith@ngrok:~$ ngrok tcp 3306
Session Status   
online                                                                                   
Account                       Appsmith-svg (Plan: Free)                                                                  
Version                       3.1.0                                                                                     
Region                        Europe (eu)                                                                               
Latency                       164ms                                                                                    
Web Interface                 http://127.0.0.1:4040                                                                     
Forwarding                    tcp://0.tcp.eu.ngrok.io:16696 -> localhost:3306 
Connections                  
ttl     opn     rt1     rt5     p50     p90                                                                            
0       0       0.00    0.00    0.00    0.00                                                                                 
```

  - For example, the host and the port to make that connection would be.

`host: 0.tcp.eu.ngrok.io`
`port: 16696`

7. Connect to the database with DBeaver and Execute a query.
8. Follow our guide to create a [MySQL Datasource](https://docs.appsmith.com/reference/datasources/querying-mysql).

` Note: disable ssl in appsmith.`

9. Connect to appsmith and run a mysql query and see the error.


```conosle
Credentials
Database Name: mysql
Username: root
password: password
```

[Back-end logs.](https://www.udrop.com/7lr5/backend-2b407002a5c7.log)
