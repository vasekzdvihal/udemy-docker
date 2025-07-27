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

```cmd
docker build . -t feedback-node:volumes
docker rum -d -p 3000:80 --rm --name feedback-app feedback-node:volumes
```

**Dockerfile Example**

```dockerfile
FROM node
WORKDIR /nodeapp
COPY package.json /nodeapp
RUN npm install
COPY Udemy/DockerKubernetesThePracticalGuide ./
EXPOSE 80
CMD ["node", "server.js"]
```

## Data & Volumes ##

**VOLUMES** - Folders/files on your **host machine** connected to folders/files **inside container**. Data persists even when container is removed. **Managed by Docker** - you don't control exact storage location.

**ANONYMOUS VOLUMES** - Created via `-v /some/path/in/container`. **Automatically removed** when container removed (with `--rm` flag). Useful for preventing **Bind Mount overwrites**.

**NAMED VOLUMES** - Created via `-v some-name:/some/path/in/container`. **NOT automatically removed** - must remove manually. Best for **persisting data** (logs, uploads, databases).

**BIND MOUNTS** - **Developer sets the path** on host machine. Created via `-v /absolute/path/on/host:/path/in/container`. Great for **development** - sharing source code that changes while container runs. **Not for production**.

**ENVIRONMENT VARIABLES** - Configuration values available at **runtime**. Set with `ENV` in Dockerfile or `-e` flag when running container. Available to application inside container.

**BUILD ARGUMENTS** - Values passed **during image build** with `ARG` in Dockerfile and `--build-arg` flag. **Not available at runtime** - only during build process. Can set default values.

**Key Docker Commands**

- `docker run -v /path/in/container IMAGE` Create an **Anonymous Volume**
- `docker run -v some-name:/path/in/container IMAGE` Create a **Named Volume**
- `docker run -v /path/on/host:/path/in/container IMAGE` Create a **Bind Mount**
- `docker volume ls` **List** all currently active/stored **volumes**
- `docker volume create VOL_NAME` **Create** a new Named Volume (usually **automatic**)
- `docker volume rm VOL_NAME` **Remove** a volume by name/ID
- `docker volume prune` **Remove** all **unused volumes**
- `docker run -e ENV_VAR=value IMAGE` Set **environment variable** at runtime
- `docker build --build-arg ARG_NAME=value .` Pass **build argument** during image build

**Examples**

```cmd
# Named Volume for persistence
docker run -d -p 3000:80 --rm --name feedback-app -v feedback:/app/feedback feedback-node:volumes

# Bind Mount for development
docker run -d -p 3000:80 --rm --name feedback-app -v /Users/max/feedback-node:/app feedback-node:volumes

# Anonymous Volume to prevent overwrites
docker run -d -p 3000:80 --rm --name feedback-app -v /app/node_modules -v /Users/max/feedback-node:/app feedback-node:volumes

# Environment variables at runtime
docker run -d -p 3000:80 --rm --name feedback-app -e PORT=8000 feedback-node:volumes

# Build arguments during image build
docker build --build-arg DEFAULT_PORT=3000 -t feedback-node:volumes .
```

**Dockerfile Example** 

```dockerfile
FROM node:14
ARG DEFAULT_PORT=80          # Build argument with default value
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
ENV PORT=$DEFAULT_PORT       # Environment variable using build argument
EXPOSE $PORT
CMD ["npm", "start"]
```

