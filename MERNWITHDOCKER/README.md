## Building Images For A MERN App thanks to Docker
1. Image Build 1(Client):
```bash
docker build -t "react-app" ./client/
```
2. Image Build 2(Server)
```bash
docker build -t "api-server" ./server/
```

3. I create a docker compose file

    1. I specify the version of the docker compose api I want to use
    
    2. I define a set of services I want running on my app:
        - react-app: client
        - express-api: server
        - mongodb: database
    
    3. I supply the image tags for each of the necessary Services I want running on my container

    
    4. For MongoDB I use the publicly available docker image from docker hub

4. In order to communicate with other services I need to setup access to proper ports for each
- Remember to setup different ports for EACH service

5. For the react container I add the standard input option to true to keep the container alive and listening for requests after start

6. Because the express server needs to connect to the MongoDB service I say it depends on it. This ensures
the containers start in a correct order

7. We can have one application running on one network but if we want isolation we can run multiple applications on different networks

8. I must define the network explicitly. Mern-app is what I called my network uses the default driver

9. I add all the services to the network

10. The services will talk to one another, while providing isolation from other docker containers that run on the same host

11. I define a docker volume to enable persistence of db data across container resources, which is mounted into the mongodb container

```yml
version: "3"
services:
    react-app:
        image: react-app
        stdin_open: true
        ports:
            - "3000:3000"
        networks:
            - mern-app
    api-server:
        image: api-server
        ports:
            - "5000:5000"
        depends_on:
            - mongo
        networks:
            - mern-app
    mongo:
        image: mongo:3.6.19-xenial
        ports:
            - "27017: 27017"
        networks:
            - mern-app
        volumes:
            - mongo-data:/data/db      
networks:
    mern-app:
        driver: bridge
volumes:
    mongo-data:
        driver: local
```

12. To start the application I run the command: 
```bash
docker-compose up
```
This will start all three containers, create and attach the network and volume resources

13. To see if the containers are running I run the command:
```bash
docker ps
```