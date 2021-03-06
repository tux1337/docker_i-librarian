
This is a Dockerfile for [I, Librarian][1]. Now using php7. Please, Look at the following instructions.

# Fork
This is a fork from [cgrima/docker_i-librarian](https://github.com/cgrima/docker_i-librarian). This fork uses Debian stretch as base instead of jessie.

# Prerequisites

If you do not already have an [I, Librarian][1] library, you must download a blank library folder to initiate the library on the host before to run the container, and set up its ownership correctly. To do so, from within the location you want the library  (you might need to install the *xz-utils* package):

```
wget -O i-librarian.tar.xz http://i-librarian.net/downloads/I,-Librarian-4.8-Linux.tar.xz
unxz i-librarian.tar.xz && tar -xvf i-librarian.tar library && rm i-librarian.tar
sudo chown -R www-data:www-data library
sudo chown root:root library/.htaccess
```

# Run the container (start [I, Librarian][1])

Assume that `{LIBRARY_PATH}` is the path to your library location on the host.

## On the command line

```
sudo docker run -d -p 8080:80 \
            -v {LIBRARY_PATH}:/library \
            -v /etc/localtime:/etc/localtime:ro \
            --name=i-librarian \
            tux1337/docker_i-librarian
```

## Docker-compose

Create your docker-compose.yml file such as

```
version: "2"

services:

  app:
    image: tux1337/docker_i-librarian
    privileged: true
    ports:
      - "8080:80"
    volumes:
      - {LIBRARY_PATH}:/library
      - /etc/localtime:/etc/localtime:ro
```

Then, start docker-compose

```
docker-compose up -d
```

# Access your I-librarian instance

Open *http://localhost:8080* on your web browser and follow instructions.


# Update
Simply stop, remove, and launch your container again. With docker-compose:
```
docker-compose down
docker-compose up -d
```

# Troubleshoot

If you have rights/permissions issues with the library folder try to add the `--privileged=true` option to the docker run command.

  [1]: http://i-librarian.net/
