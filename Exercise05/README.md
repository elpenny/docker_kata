# Exercise 5 - dockerfiles

This exercise shows how to build nodejs app image.

## Description
In previous exercise we created nginx container serving static content. Now we will learn how to perform more operations while creating image, which will let us create image with working nodejs app.

## Kata
1. Start by creating file named 'Dockerfile' in this folder.
2. We need to start from nginx again as its superior webserver. Put this into dockerfile:
```dockerfile
FROM node:10-alpine
```

3. Then include new command ```Label``` that is used to add image metadata, write this:
```dockerfile
LABEL "Maintainer"="You"
```

4. We need some directory to hold all needed data, run this command:
```dockerfile
RUN mkdir -p /src/app
```

5. Rest of commands should be run from this folder. To set path we can use ```WORKDIR``` cmd:
```Dockerfile
WORKDIR /src/app
```

6. Next step is to copy package.json from host to image. We do this as separate command due to layering in Docker images (will explain in next steps)
```dockerfile
COPY package.json /src/app/package.json
```

7. Now we can install all needed dependencies. We are in ```/src/app```, we have ```package.json``` there so we need to write this:
```Dockerfile
RUN npm install
```

8. After dependencies are installed we can copy over source files. We do it separately from copying ```package.json``` so there ill be no need to rerun ```npm install``` hen source code changes. Docker figures out that steps before are not changed and ill reuse previous layers, starting building from copying source code. Of course, when ```package.json``` will change, Docker will need to discard cached layers. You can force docker to create each layer by adding ```--no-cache=true``` to ```docker build``` command. Write to dockerfile:
```dockerfile
COPY . /src/app
```

9. Node uses port 3000 on default to serve application, so wwe should mark it in dockerfile for future user. Add this to dockerfile:
```dockerfile
EXPOSE 3000
```

10. We want this image to run in detached mode, unattended. To ensure this we need to add ```CMD``` : 
```Dockerfile
CMD [ "npm", "start" ]
```

11. We finished writing dockerfile! To build it, make sure you are in correct directory and run:
```bash
docker build -t my-nodejs-app .
```

12. After docker daemon built image we can run ne container from it, type into console:
```bash
docker run -d --name my-running-app -p 3040:3000 my-nodejs-app
```

13. Go to ```localhost:3040``` to see that app is working! This concludes Exercise 5.

By now you should realise that Dockerfiles have quite simple set of rules and can be used to create virtually any distributable. You can use windows OS as base image if you want, but its much better to write images for *nix architecture.

If you have any problem with this exercise you can go to ```./finished``` directory where you have duplicated project and finished dockerfile. From there you can build and run that dockerfile to get understanding of what went wrong.

