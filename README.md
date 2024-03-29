# Docker Notes

## Basics

| Task                    | Command                                  |
|-------------------------|------------------------------------------|
| List Images             | ```docker images -a```                   |
| Remove Image            | ```docker rmi Image```                   |
| Remove All Images       | ```docker system prune -a```             |
| List All Containers     | ```docker ps -a```                       |
| List Running Containers | ```docker ps```                          |
| List Stopped Containers | ```docker ps --filter "status=exited"``` |
| Remove Container        | ```docker rm [container id]```           |
| Run a container         | ```docker run -p 3000:3000 geocolumbus/webby``` |    
| Remove all Containers | ```docker container prune -y``` |
| Connect to mongo | ``` mongo mongodb://mongo:27017/dev-environment -u application -p test``` |
| Shell into container | ```docker exec -it <container> /bin/sh``` |

## Create a Node Image

1. Create this Dockerfile:
   ```dockerfile
   FROM node:12
   EXPOSE: 3000
   ```
1. Build the project
   ```bash
   docker build -t my-nodejs-app .
   ```
1. Run the project

   You can type ```.exit``` to quit.
   ```bash
   docker run -it --rm --name my-running-app my-nodejs-app
   ```
1. Run ```docker images -a``` to see the node container.
   ```text
   REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
   my-nodejs-app       latest              28d7ff5c0c61        43 seconds ago      913MB
   node                12                  839a5e8f04b4        3 days ago          913MB
   ```
      
## Composing

In your node project directory, create a ```docker-compose.yml``` file

```yaml
version: "2"
services:
  node:
    image: "node:12"
    user: "node"
    working_dir: /home/node/app
    environment:
      - NODE_ENV=production
    volumes:
      - ./:/home/node/app
    expose:
      - "8081"
    command: "npm start"
```
Run the node source code in the current directory (assumes that package.json has "start" configured properly and the node_modules is present).
```bash
docker-compose up -d
```

## Exporting Images

Example of creating a dockerized UI, saving it as a tar file, then importing it and running it.

    vue create hello-world
    cd hello-world
    npm run build
    cat > Dockerfile

    FROM node:16
    WORKDIR /app
    COPY dist .
    RUN npm install -g serve
    CMD ["serve"]
    
    docker build -t vue1 .
    
    docker image save -o vue1.tar e366ac54d050
    docker image rm e366ac54d050
    docker load -i vue1.tar
    docker run -p 3000:3000 vue1


## List of Docker Platforms

    arm32v6
    arm32v7
    arm64v8
    amd64

    arm32v5
    ppc64le
    s390x
    mips64le
    riscv64
    i386

## Reference

* [Docker Official Images](https://hub.docker.com/search?q=node&type=image&image_filter=official)
  * [Node](https://hub.docker.com/_/node)
  * [How to use the Node Image](https://github.com/nodejs/docker-node/blob/master/README.md#how-to-use-this-image)

