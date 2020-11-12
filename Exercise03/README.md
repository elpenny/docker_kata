# Exercise 3 - data persistence

This exercise shows how to persist data in the container and how to expose container to the host it is running in.

## Description
By default, docker containers does not persist any data and are entirely sandboxed from its' host. We know how to run container in interactive mode to gain access to it, but this is not sufficient for many use cases.

## Kata
1. Start by running this command:
```bash
docker run --rm -d -p 8080:80 --name web nginx:1.19.4-alpine
```
* `-rm` and `-d` together means that container will start in detached mode and will be removed once it stops.
* `-p 8080:80` exposes container port 80 onto host port 8080
* `nginx:1.19.4-alpine` requests nginx image based on special Linux distro, Alpine which is very lightweight and stripped of unnecessary stuff.
2. Open a browwser and go to `localhost:8080`, you should see Nginx welcome page.
3. Stop the container by typing:
```bash
docker stop web
```
* This also removed containerr as it was sopecified in run command

4. Running web server without any installation is nice but how to put our content in it? Answwer is: by using mounted volumes. This is special feature that lets Docker mount directory from host onto container. 
Make sure you are in Exercise3 directory in host terminal and run this command:
```bash
docker run --rm -d -p 8080:80 --name web -v $pwd/site-content:/usr/share/nginx/html nginx:1.19.4-alpine
```
* `-v` switch is used to specify bind mounts, it reads as `-v pathToDirOnHost:pathToDirInContainer`

5. Go again to `localhost:8080` to see changed index.html.
6. Now edit index.html inside site-content folder and refresh webpage in browser, you should see your changes.
7. This is cool, isn't it? In that way you can persist changes in container. Just start it with mounted folder and you are good to go. For example, if you want to use some database container, you will need to mount some folder from host onto write location for db in container, that way everytime you recreate container it will have all previous changes to db files.
8. Before we stop and remove web container there are two more thing we can do. Let's connect to container running in detached mode and see content of index.html from inside!
9. Run command:
```bash
docker exec -it web sh
```
* `exec` is command to execute specified program in container
* `sh` brings exec command to interactive mode
* `web` is name of container that is of interest to us
* `sh` is command to run on our container (there is no bash in alpine images)

10.  Now we are inside container. Type `cd /usr/share/nginx/html` and then `cat index.html` to see that it is, in fact, the same file as in `./site-content/`.
11. Type
```bash
echo "<div> Changed index.html from inside container</div>" >> index.html
```
* This will add string to index.html from inside of container. Refresh web page or look into `./site-content/index.html` to see that change.
12. Type `exit` to exit ``shell.
13. Run this command to get details about running container:
```bash
docker inspect web
```
14. Now stop the container (which will also remove it). This is the end of Kata 3.

After this you should have solid understanding of one of methods to persist data for containers, how to enter detached container and how to expose ports in container on host machine.

