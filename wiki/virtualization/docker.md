# Docker

### Containerize an app

Download the **getting started** repository and create the `Dockerfile`.

```bash
git clone https://github.com/docker/getting-started-app.git
cd ./getting-started-app
vim Dockerfile
```

Add this content in the `Dockerfile`.

```dockerfile
# syntax=docker/dockerfile:1
FROM node:18-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
EXPOSE 3000
```

Create an image from the `Dockerfile`.

```bash
docker build -t getting-started .
```

Create a container from the image `getting-started` and expose the port 3000.

```bash
docker run -dp 0.0.0.0:3000:3000 getting-started
docker ps
```

### Update the app

Edit the file `src/static/js/app.js` (`No items yet!` → `You have no todo items yet!`).

```bash
docker ps
docker stop <id_container>
docker rm <id_container>

docker build -t getting-started .
docker run -dp 0.0.0.0:3000:3000 getting-started
```

### Share the app

* Sign in to [Docker Hub](https://hub.docker.com/).
* Create a public repository named `getting-started`.

```bash
docker login -u katseyres                             # login to Docker Hub
docker tag getting-started katseyres/getting-started  # rename image
docker push katseyres/getting-started                 # push image
```

* Run this image from a new instance.

```bash
docker run -dp 0.0.0.0:3000:3000 YOUR-USER-NAME/getting-started
```

### Persist the DB

* Create a file `/data.txt` in the container with a random number.

```bash
docker run -d ubuntu bash -c "shuf -i 1-10000 -n 1 -o /data.txt && tail -f /dev/null"
docker exec <container-id> cat /data.txt
```

* Create another instance with the same image.

```bash
docker run -it ubuntu ls /
```

* Create a volume and run a new container.

```bash
docker volume create todo-db
docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
```

* Add some tickets on the web app.
* Remove the container.

```bash
docker rm -f <id>
```

* Even if the container gets removed, the data is still there.

```bash
docker run -dp 127.0.0.1:3000:3000 --mount type=volume,src=todo-db,target=/etc/todos getting-started
docker volume inspect todo-db
```

### Use bind mounts
