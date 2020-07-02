![](https://img.shields.io/github/issues/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/forks/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/stars/marcelocostabr/docker-template-default)
![](https://img.shields.io/github/license/marcelocostabr/docker-template-default)

# Docker Server Template [Nginx + PHP 7.4 + MariaDB 10.5]

Docker template with default images and customs on docker-compose.yml

By default this container runs:

| Service  | Version      |
|----------|--------------|
| Server   | Nginx        |
| PHP      | FPM:7.4      |
| Database | MariaDB:10.5 |

All Docker customization stay at ".docker" folder. At the repository root you have to put the "docker-compose.yml" along with the app files.

Please pay attention at assets folder organization:

| Folder        | Description        |
|---------------|--------------------|
| .docker/db    | folder for data persistence inside the container |
| .docker/nginx | custom configuration for Nginx server |
| .docker/php   | custom Dockerfile from PHP |

To use, please follow the steps bellow:

1) [Clone this repository](#clone-this-repository)
2) [Install Docker](#install-docker)
3) [Setting up the Virtual Server](#setting-up-the-virtual-server)
4) [Run and manage the Docker Server](#run-docker)
5) [Browse to localhost:8010](#browse)

## Clone this repository

Open de root folder in the terminal and type:

```
git clone https://github.com/marcelocostabr/docker-template-default.git
```

## Install Docker

Make sure you have Docker installed, in the terminal type:

```
docker -v
```

If you have something like this: "Docker version 19.03.8, build afacb8b" you can continue, if not please go to [https://www.docker.com/](https://www.docker.com/).

## Setting up the Virtual Server

You most to edit ./docker/nginx/default.conf with the server name desired.

```
server_name site.local www.site.local;
```
After that edit the /etc/hosts file in your OS adding the following line:

```
127.0.0.1 site.local www.site.local
```

## Run Docker

Start Server (in background)

```
docker-compose up -d
```

Stop Server and keep the images

```
docker-compose down
```

Rebuild Server

```
docker-compose up -d --build
```

List active containers

```
docker ps -a
```

Stop and Remove all Containers

```
docker stop $(docker ps -a -q) && docker rm $(docker ps -a -q)
```


## Browse

To access the server please go to [http://site.local](http://site.local).