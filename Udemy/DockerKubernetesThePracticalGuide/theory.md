## Docker & Kubernetes: The Practical Guide

## Images & Containers

**IMAGES** - Images are blueprints / templates for containers. They are read-only and container the application as well as the necessary application environment (operating system, runtimes, tools, ...). Images do not run themselves, instead, they can be executed as containers. Images are either pre-built (e.g. official Images you find on [Dockerhub](https://hub.docker.com/) or you build your own images by defining a **Dockerfile**)

**CONTAINERS** - Containers are running instances of Images. Multiple Containers can therefore be started based on one and the same Image.

### Key Docker Commands ###

- `docker build .` Build a Dockerfile and create your own image based on the file
  - `-t NAME:TAG` Assign a `NAME` and a `TAG` to an image
- `docker run IMAGE_NAME` Create and start a new container based on image `IMAGENAME` (or use the image id)
  - `--name NAME` Assign a `NAME` to the container. The name can be used for stopping and removing etc.
  - `-d` Run the container in **detached** mode - i.e. output printed by the container is not visible, the command prompt / terminal does NOT wait for the container to stop 
  - `-it`Run the container in **interactive** mode - the container  / application is then prepared to receive input via the command prompt / terminal. You can stop the container with `CTRL + C` when using the `-it` flag
  - `--rm` Automatically **remove** the container when it's stopped
- `docker ps` **List** all **running** containers 
  - `-a` **List** all **containers** - including **stopped** ones
- `docker images` **List** all **locally stored images**
- `docker rm CONTAINER` **Remove** a container with name `CONTAINER` (you can also use the container id)
- `docker rmi IMAGE` Remove an image by name / id
- `docker container prune` **Remove** all **stopped** containers
  - `-a` **Remove** all **locally** **stored** images
- `docker push IMAGE` **Push** an image to **DockerHub** (or another registry) - the image name/tag must include the repository name/url
- `docker pull IMAGE` **Pull** (download) an image **from DockerHub** (or another registry) - *this is done automatically of you just `docker run IMAGE` and the image wasn't pulled before*

 **Examples**

```
docker build . -t feedback-node:volumes
docker rum -d -p 3000:80 --rm --name feedback-app feedback-node:volumes
```

