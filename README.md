# Docker Fundamentals

### Images
```
An image is a template for creating an environment of your choice. This environment can be a DB, Web App
or an App that does some time of processing.

An image is also a snapshot a version at any specific moment in time.
Imagine I deploy an image to production and my application was not error free. I can always
roll back to the previous image.
An image contains all the necessities for my application to run: OS, Software and Application Code.

```

### Container
```
A container is a running instance of an image
To run an nginx container:
```

```docker
docker pull nginx
```
### Check if there is any running container
```docker
docker ps
```


### To see the list of running images
```docker
docker images
```

### How To Spin a new container e.g. in this case I am spinning a redis container
```docker
docker run redis
```

### run the latest container from an image
```docker
docker run imagename:tag
docker run nginx:latest
```

### Query The list of Running Containers
```docker
docker container ls
```

### Stop A Running Container Container id is a numerical value
```docker
docker stop containerid
```

### Map Ports From Local Machine to Container here mapping port 256837 on my local machine to port 80 on the container
```docker
docker run -d 256837:80 nginx:latest
```

### How To Map Multiple Ports -d means in detached mode
```docker
docker run -d -p 3000:80 -p 8080:80 nginx:latest
```

### How To Restart A Container
```docker
docker stop 252656837c263727
docker start 252656837c263727
```

### Say you finished your day and the next day you do not recall which container you ran
### because docker ps doesn't show you the full list well -a shows you the running and stopped 
### container
```docker
docker ps -a
```

### Sometimes some users phone do not support the latest version which is 5.0 and you want
### to keep version 4.0 so you can run the image 4.0 and 5.0 the bottom command pulls and start container
```docker
docker run redis:4.0
```


### Ports 
```
I must create a binding port between my local machine and the container.
So say my container is listening on port 3000 I must bind the listening
port 3000 on my machine to the container port 3000

IF YOU OPEN TWO OF THE SAME PORTS(ON YOUR MACHINE) AND MAP THEM TO DIFFERENT PORTS ON THE CONTAINER
YOU WILL FACE A PROBLEM

HOWEVER IF YOU TWO DIFFERENT PORTS ON YOUR MACHINE TO THE SAME PORT IN THE CONTAINER THAT IS OKAY
```


### DEBUGGING CONTAINERS
```docker
docker logs
```

```docker
docker exec -it
```



