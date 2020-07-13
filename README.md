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
4) [SSL Certificate](#ssl-certificate)
5) [Confidential Data]($confidential-data)
6) [Run and manage the Docker Server](#run-docker)
7) [Browse localhost](#browse)

## Clone this repository

Open de root projects folder in the terminal and type:

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

You must to edit ./docker/nginx/default.conf with the server name desired.

```
server_name dev.local www.dev.local;
```
After that edit the /etc/hosts file in your OS adding the following line:

```
127.0.0.1 dev.local www.dev.local
```

Note: as default, you can use **dev.local**, this local url run out of the box with a SSL certificate. If you opt to change you will need to get a new SSL certificate for the new development domain.

## SSL Certificate

To access with **https** over localhost or custom Virtual Server (above), you have to generate a SSL Certificate. I recommend to use only the dev.local like the oficial local host, that way you need to generate only once the SSL certificate independent of the project.

### Install the mkcert ###

**macOS**

```
brew install mkcert
brew install nss # if you use Firefox
```

**Linux**

```
# only on linux must install Certutil first
sudo apt install libnss3-tools -y

sudo apt install libnss3-tools
    -or-
sudo yum install nss-tools
    -or-
sudo pacman -S nss
    -or-
sudo zypper install mozilla-nss-tools
```

**Windows**

```
# via Chocolatey (https://chocolatey.org/)
choco install mkcert
```

### Generate Local CA ###

```
mkcert -install
````

### Generate Local SSL Certificates ###

```
sudo mkcert example.com '*.example.com' localhost 127.0.0.1 ::1
````

### Copy, Move and Rename ###

Copy the certificate example.com+4.pem and key example.com+4-key.pem into folder .docker/nginx of your project.

Rename these files to server.pem and server-key.pem and give the permission 644.

```
sudo chmod 644 server.pem
sudo chmod 644 server-key.pem
````

## Confidential Data

All confidential data, like passwords, e-mail credential and more must be saved on **secret/Config.php**.

This file aren't track by git, to generate it you must edit **secret/Config-Sample.php** and save as like **secret/Config.php**.

Note: when the project go live you must have to generate the Config.php with the server credentials and upload manually.

## Run Docker

After all done, you need to up the server only.

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

To access the server please go to:

**http**
[http://localhost](http://localhost) or [http://dev.local](http://dev.local)

**https**
[https://localhost](https://localhost) or [https://dev.local](https://dev.local)
