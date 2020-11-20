# Exercise 4 - dockerfiles

This exercise shows how to build and run your own docker image.

## Description
Running already built images is nice but it does not exhaust docker use cases. In order to fully leverage docker capabilities you need to learn how to create your own images, so let's start hacking!

## Kata
1. Start by creating file named 'Dockerfile' in this folder.
2. Dockerfile is instruction set for docker daemon, it tells docker how to build your image.
3. Each image is based on some other image. First line of each image must be: 
```dockerfile 
FROM Image:Tag
``` 
4. You should pick some Linux distro as a base image - or something that is built on top of it. There is one special case here - if you want to build base OS image, you should start 
```dockerfile
FROM scratch
```
5. Now that we know howw to start, enter first line into the dockerfile
```dockerfile
FROM nginx:1.11-alpine
```
6. Great! Now docker knows that we want to use nginx image as a base for our image.
7. There are some commonly used dockerfile commands:
```dockerfile
COPY <src> <dst>                    # Copies content
ADD <src> <dst>                     # Adds content (can add from URL and can extract tarballs)
RUN 'command'                       # Executes command, e.g RUN mkdir -p /new/dir
EXPOSE number                       # Directive for letting know image user which ports are used
CMD ['exec', 'param1' ...]          # What will be fed to entrypoint
ENTRYPOINT ['exec', 'param1' ...]   # Changes default entrypoint
# Each container has a default ENTRYPOINT /bin/sh -c 
```
8. So we want to add index.html file onto docker image we are creating. Put this line in dockerfile:
```dockerfile
COPY index.html /usr/share/nginx/html/index.html
```
9. This is quite straightforward, but it is important to knoww that all files added from host should be located using path relative to dockerfile.
10. Next we want to inform which port  image ill use to expose iotself to the world, so write:
```dockerfile
EXPOSE 80
```
11. Lastly we need to define CMD that will be run:
```dockerfile
CMD ["nginx", "-g", "daemon off;"]
``` 
12. Hurray! Our first dockerfile is ready! Time to build it. Make sure you are in directory of dockerfile and run:
```
docker build -t ng .
```
13. After its built we should test it by running:
```
docker run -d --name test -p 80:8080 ng
```
14. Go to ``` localhost:8080 ```  to see that image is doing its job!
15. If anything is not working as expected, go to ./finished directory, there you have correct dockerfile with index.html so you can run build command from that folder.

After this exercise you should understand how to build own images ;)