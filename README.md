# swarm02

##### REF
This repo reference from ***https://github.com/docker/awesome-compose/tree/master/aspnet-mssql***

Project structure:
```
.
├── app
│   ├── aspnetapp
│   │   ├── appsettings.Development.json
|   |   └── ...
│   ├── ...
│   └── Dockerfile
└── docker-compose.yaml
```
[_docker-compose.yaml_](docker-compose.yaml)

```
version: '3'
services:
  web:
    image: 'paowick/swarm02_aspnet:1.0'
    networks: 
      - backend
      - webproxy
    deploy:
        replicas: 1
        labels:
          - traefik.docker.network=webproxy
          - traefik.enable=true
          - traefik.constraint-label=webproxy
          - traefik.http.routers.${APPNAME}-https.entrypoints=websecure
          - traefik.http.routers.${APPNAME}-https.rule=Host("${APPNAME}.xops.ipv9.me")
          - traefik.http.routers.${APPNAME}-https.tls.certresolver=default
          - traefik.http.services.${APPNAME}.loadbalancer.server.port=80
          - traefik.http.routers.${APPNAME}-https.tls=true
        resources:
          reservations:
            cpus: '0.1'
            memory: 10M
          limits:
            cpus: '0.4'
            memory: 250M



  db:
    environment:
      ACCEPT_EULA: "Y"
      SA_PASSWORD: example_123
    # mssql server image isn't available for arm64 architecture, so we use azure-sql instead
    image: mcr.microsoft.com/azure-sql-edge:1.0.4
    # If you really want to use MS SQL Server, uncomment the following line
    #image: mcr.microsoft.com/mssql/server
    networks: 
      - backend
      
networks:
  webproxy:
    external: true
  backend:
    driver: overlay

```

#### Step
&emsp; **1**.clone <ins>*aspnet-mssql porject*</ins> from <ins>*awesome-compose*</ins>\
&emsp; **2**.build image form Dockerfile and push to **Docker Hub**

```
.
├── app
│   ├── aspnetapp
│   │   ├── appsettings.Development.json
|   |   └── ...
│   ├── ...
│   └── Dockerfile  <-- here!!
└── docker-compose.yaml
```
&emsp; build image
```
$docker build -t paowick/swarm02_aspnet:1.0
```
&emsp; push image to **Docker Hub**
```
$docker push paowick/swarm02_aspnet
```
&emsp; **3**.Deoly with docker-compose in https://portainer.ipv9.me/

***url https://spcn23swarm02.xops.ipv9.me/***