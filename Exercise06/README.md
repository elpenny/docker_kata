# Exercise 6 - docker-compose

This exercise shows basic of docker-compose

## Description
In previous exercises we learnt a lot of docker basics, now is the time to learn simplest orchestration tool possible: docker-compose.

## Kata
1. Start by creating dir named 'composetest' in this dir and enter it.
```bash
mkdir composetest
cd composetest
```

2. Create a file called ```app.py``` in your project directory and paste this in:
```python
import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)

```
3. Create another file called ```requirements.txt``` in your project directory and paste this in:
```python
flask
redis
```
4. In this step, you write a Dockerfile that builds a Docker image. The image contains all the dependencies the Python application requires, including Python itself. In your project directory, create a file named ```Dockerfile``` and paste the following:
```docker
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

This tells Docker to:

* Build an image starting with the Python 3.7 image.
* Set the working directory to /code.
* Set environment variables used by the flask command.
* Install gcc and other dependencies
* Copy requirements.txt and install the Python dependencies.
* Add metadata to the image to describe that the container is listening on port 5000
* Copy the current directory . in the project to the workdir . in the image.
* Set the default command for the container to flask run.

5. Create a file called ```docker-compose.yml``` in your project directory and paste the following:
```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```

This Compose file defines two services: web and redis.

**Web service**

The web service uses an image that’s built from the Dockerfile in the current directory. It then binds the container and the host machine to the exposed port, 5000. This example service uses the default port for the Flask web server, 5000.

**Redis service**

The redis service uses a public Redis image pulled from the Docker Hub registry. 
<br>

6. From your project directory, start up your application by running ```docker-compose up```

Compose pulls a Redis image, builds an image for your code, and starts the services you defined. In this case, the code is statically copied into the image at build time.

7. Enter http://localhost:5000/ in a browser to see the application running.

If you’re using Docker natively on Linux, Docker Desktop for Mac, or Docker Desktop for Windows, then the web app should now be listening on port 5000 on your Docker daemon host. Point your web browser to http://localhost:5000 to find the Hello World message. If this doesn’t resolve, you can also try http://127.0.0.1:5000.

If you’re using Docker Machine on a Mac or Windows, use ```docker-machine ip MACHINE_VM``` to get the IP address of your Docker host. Then, open ```http://MACHINE_VM_IP:5000``` in a browser.

8. Refresh the page.
9. Switch to another terminal window, and type ```docker image ls``` to list local images.
10. You can inspect images with ```docker inspect <tag or id>```
11. Stop the application, either by running ```docker-compose down``` from within your project directory in the second terminal, or by hitting CTRL+C in the original terminal where you started the app.
12. Edit ```docker-compose.yml``` in your project directory to add a bind mount for the web service:
```yaml
version: "3.9"
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
    environment:
      FLASK_ENV: development
  redis:
    image: "redis:alpine"
```
The new volumes key mounts the project directory (current directory) on the host to /code inside the container, allowing you to modify the code on the fly, without having to rebuild the image. The environment key sets the FLASK_ENV environment variable, which tells flask run to run in development mode and reload the code on change. This mode should only be used in development.

13. From your project directory, type ```docker-compose up``` to build the app with the updated Compose file, and run it.

14. Because the application code is now mounted into the container using a volume, you can make changes to its code and see the changes instantly, without having to rebuild the image.

Change the greeting in ```app.py``` and save it. For example, change the Hello World! message to Hello from Docker!:
```python
return 'Hello from Docker! I have been seen {} times.\n'.format(count)
```

15. Refresh the app in your browser. The greeting should be updated, and the counter should still be incrementing.

16. If you want to run your services in the background, you can pass the ```-d``` flag (for “detached” mode) to ```docker-compose up``` and use ```docker-compose ps``` to see what is currently running:
17. The docker-compose run command allows you to run one-off commands for your services. For example, to see what environment variables are available to the web service:
```bash
docker-compose run web env
```
18. You can bring everything down, removing the containers entirely, with the down command. Pass ```--volumes``` to also remove the data volume used by the Redis container:
```
docker-compose down --volumes
```
