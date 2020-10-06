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

### To see the list of running images
```docker
docker images
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


