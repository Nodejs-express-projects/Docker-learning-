# Dockerize nodejs app with Postgres

## Create a NodeJS application with database connections to postgres (npm ps)

**1) Install docker in local System**

## Create a file called Dockerfile in the application root folder

The contents of the Dockerfile are as follows

**Sample docker file**

```
FROM node:12.0-slim
COPY . .
RUN npm install
CMD [ "node" , "server.js" ]

```

FROM indicates the additional docker container to download, in this case is the node VM
COPY indicates the workfiles to be copied from particular path
RUN While running the container what are the commands that can be called
CMD indicates the command for running the application in array notation


After creating the application run the command 

```
docker build -t cvwithexpress .

```

Syntax 
```
docker build -t (nodejsAppname) .

```

After the build is complete 

## Create a docker network to communicate between containers

command
```
docker network create cvnetwork

```

## Run the dependencies in the network( In this case it is Postgres )

```
docker run -e "POSTGRES_HOST_AUTH_METHOD=trust"   --name=my-postgres-image --rm --network=cvdata -p 3001:5432 postgres

```

-rm - removes the old image and adds new one during creation
-p - package port expose externalport:internalport 

This command creates and runs the postgres instance as a container in the network cvnetwork

## Run the application container


```
docker run  -e DATABASENAME=POSTGRES -e PGUSER=postgres -e PGHOST=postgres  -e PGPASSWORD=balaji -e PGDATABASE=postgres -e PGPORT=5432 --name=cvwithexpress --rm --network=cvnetwork  -p 3000:5000  cvwithexpress

```

You can check the containers in a network by the command 

```
docker network inspect cvresume

```

```
[
    {
        "Name": "cvresume",
        "Id": "37b5672f3ec254dba411f46af7528ac2d85ff151a37cec95beecb38bcbb1bfbd",
        "Created": "2021-06-11T18:51:58.2948681Z",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "6a1da7612a6392fe5e83f75ad14263b95d8881ea394870d7f131a47c92188693": {
                "Name": "my-postgres-image",
                "EndpointID": "b11ea73a3d8d311cf32996329742c058de8b1ba2bda3e096b2bbac198c735fee",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "d1c59456e336fbadb5634174da2f2821c513f2cc1c07494dac9e81d0bbf8d9a2": {
                "Name": "cvwithexpress",
                "EndpointID": "d7458caa628e557077c4ee625b26c9fa1e41d29288f3ab9ef88d4e725bd08ef8",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]
```

This is run the application and the application will connect to database  and pull data if the configurations are correct







