# Leverage Development Environment

This project provides configurations to run the leverage API
and database in a dockerized environment for simple
cross-platform development and testing.

## Creating Local Repository

Create local repository of leverage-devbackend on your computer.
You can follow these instructions if not familiar with git:
https://services.github.com/on-demand/github-cli/clone-repo-cli

## Before Getting Started

The DB container needs a SQL script to
bootstrap the database with. Place a
database dump in `volumes/db/docker-entrypoint-initdb.d`
to have the DB bootstrap itself with that dump.
To get the database dump file - search for leverage_philly file
on leverage slack channel.
The DB will bootstrap that data into a database named
"leverage", which is accessible by a user:password combination
of "leverage:leverage".

## Getting Started

#### Run all three commands the first time you run, or any time you pull the latest from git (if you are using Windows make sure you run them in docker terminal after you cd into your local repository folder from the above step). If it's not your first run - you only need to run the 'up' command:


```
git submodule update --init
docker-compose build
docker-compose up -d
```

## Running Docker

#### After launching Docker :

```
docker images
```

#### See available containers with the following command (but for now only launch with the docker-compose command above:

```
docker images
```

#### The docker-compose command above launches the docker containers that Leverage uses. See what containers are running with:

```
docker ps
```

#### Get the IP address of the containers with with:

```
docker inspect CONTAINER_ID
```
#### If you run windows try the following command to get IP address:

```
docker-machine ip default
```
#### Connect to a command line interface on running container with:

```
docker exec -it CONTAINER_ID /bin/bash
```

Connect to the container for leverage-api. If you type 'ls' you will see all the files from the [feature/v2 branch of the api repository](https://github.com/Lever-age/api/tree/feature/v2). To leave the docker container, type 'exit'  -- but note that this connand does not stop tye container! 

#### To stop containers, see which runs are running with the 'ps' command above, then type:

```
docker stop CONTAINER_ID
```

## Accessing phpmyadmin leverage database in a browser
Type IP address of the container from the step above ':' port number. Port number comes from docker-compose.yml in leverage-devbackend folder
 and can also be found running ```docker inspect``` for phpmyadmin container. Use user:password combination of "leverage:leverage"


## Troubleshooting
Ran into some trouble on Ubuntu. Default MySQL port is taken (for now stop MySQL), and docker-registry uses port 5000 (for now stop docker-registry).

```
sudo /etc/init.d/mysql stop
sudo lsof -i -n -P | grep 5000
sudo /etc/init.d/docker-registry stop
```


## Testing DB Changes

Anytime changes are made to the database
schema, the db container will need to be
destroyed and re-created with the new
data. The steps to do so are as follows:

1. Remove old SQL script from `volumes/db/docker-entrypoint-initdb.d`
2. Add new SQL script to `volumes/db/docker-entrypoint-initdb.d`
3. Stop and remove the db container (`docker-compose down`)
4. Create new db container (`docker-compose up -d`)

## Testing API Changes

Anytime changes are made to the API, the leverage-api
image will need to be rebuilt, then the api container
will need to be destroyed and recreated based on the
new image. The steps to do so are as follows:

1. Build new leverage-api container (`docker-compose build`)
2. Stop and remove the api container (`docker-compose down`)
3. create new api container (`docker-compose up`)

## Tidying up

To remove old builds of the leverage-api container, occasionally
run `docker image prune`. This removes all untagged container
images (full disclosure: this may remove untagged containers
which were built for other unrelated projects, but untagged
containers are almost always old and unwanted, so unless you
know for sure you don't want to do this, you probably do).
