# Exercise 2 - Linux container

This exercise shows basic docker commands needed to navigate Docker efficiently.

## Kata
1. Open your favourite terminal and be ready to hack!
2. Run 
```bash 
docker run -d ubuntu bash
```
3. Docker will start by checking if it has requested image already downloaded. Probably not so it will pull image (each layer is downloaded and validated separately to speed up process) from DockerHub and only after that it will create container from it. DockerHub is free image registry
4. By now we know our first Docker command: `run`. It is used to actually take image and create container from it. `-d` switch tells Docker to run container in 'detached' mode. Because our container has no job to do, it will exit immediately.
5. To see that something actually happend type 
```bash
docker ps -a
``` 
6. `docker ps` shows all running containers, while `-a` switch tells docker to show all containers in the host, regardless of their state. Output should show us at least one ubuntu container with 'Exited' status.
Additionally we can see what was default command for that container: ```bash```.
7. Let's remove container, we can do this by typing `docker rm {containerName || containerID}`. Docker will give meaningfull names to your containers that can be observed by `docker ps` command. Container Ids are also visible on that command.
8. Therer is also a way to delete all stopped containers. This will not remove any running container!
```bash
docker rm $(docker ps -a -q)
```
9. Lets run ubuntu container again, this time with specific name and in 'interactive' mode. This command differs a bit:
```bash
docker run -it --name ubuntu ubuntu:18.04
```

* `-it` means 'run this container in interactive mode`
* `--name {NAME}` gives container specific name, WARNING: each container has to have unique name!
* `ubuntu:18.04` means that docker have to fetch image in specific version. If you do not specify tags it defaults to `image:latest` version. It is always good idea to pin specific tag, so we don't operate on unknown version of chosen image.

10. Container starts and suddenly we are in other terminal! We are currently in bash shell inside container. This is full-featured Ubuntu distribution and we have root privileges. Let's do some digging around, type:
```bash
mkdir -p usr/test/trash
cd usr/test/trash
echo "Hi Ubuntu!" >> testFile
cat testFile
exit
```
* We created some directory tree 
* Then we entered in the last directory created
* We created some text file qwith string content
* And outputted that into stdout
* Lastly, we exited container

11. Remove container by running 
```bash
docker rm ubuntu
```
12. This marks the end of 2nd exercise. By now you should have some understading how containers are run, what is the difference between interactive and detached mode, how to name containers and how to remove them. Next exercise will focus on data persitency and exposing container.


