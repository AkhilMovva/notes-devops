# Docker

## Commands

```bash
docker build --tag akhilmovva/flask-image .
docker run -p 5000:5000 akhilmovva/flask-image
docker push akhilmovva/flask-image

docker run -d -p 5000:5000 --restart unless-stopped --name flask-app akhilmovva/flask-image
docker run -d -e ENV=dev -e MYSQL_HOST=18.205.243.191 -p 5000:5000 --name flask-app akhilmovva/flask-image

docker run -e MYSQL_ROOT_PASSWORD=akhil123 -d -p 3306:3306 --restart unless-stopped --name mysql8 mysql:8
docker exec -it mysql8 bash
```

```bash
$ docker version
$ docker info
$ docker
$ docker container run --publish 80:80 nginx
$ docker container ls
$ docker stop <CONTAINER_ID>
$ docker container run --publish 80:80 --detach --name <Name> nginx
$ docker container ls -a
$ docker container logs <Name>
$ docker container top <Name>
$ docker container rm -f 9d5 772 ebe
$ docker run --name mongo -d mongo
$ docker ps

$ docker container run -d -p 3306:3306 --name db -e MYSQL_RANDOM_ROOT_PASSWORD=yes mysql

docker container run --rm -it centos:7 bash
docker container run --rm -it ubuntu:14.04 bash
```
