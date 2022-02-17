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

## Reference

* [Docker Official Images](https://hub.docker.com/search?q=node&type=image&image_filter=official)
  * [Node](https://hub.docker.com/_/node)
  * [How to use the Node Image](https://github.com/nodejs/docker-node/blob/master/README.md#how-to-use-this-image)

