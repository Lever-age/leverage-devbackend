# Leverage Development Environment

This project provides configurations to run the leverage API
and database in a dockerized environment for simple
cross-platform development and testing.

## Before Getting Started

The DB container needs a SQL script to
bootstrap the database with. Place a
database dump in `volumes/db/docker-entrypoint-initdb.d`,
and make sure that the first line of the script is
`USE leverage;`. Remove other `USE` statements
if necessary.

You will need to create a user. Create a file in that directory (such as grant_user.sql) and put in a line like this:
```
grant all privileges on [replace with database name].* to [replace with database user]@localhost identified by '[replace with database password]';
```

An example grant_user.sql file is in the root directory that you can copy into `volumes/db/docker-entrypoint-initdb.d`

## Getting Started

#### Run all three commands the first time you run, or any time you pull the latest from git. After that you only need to run the 'up' command:


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

#### Connect to a command line interface on running container with:

```
docker exec -it CONTAINER_ID /bin/bash
```

Connect to the container for leverage-api. If you type 'ls' you will see all the files from the [feature/v2 branch of the api repository](https://github.com/Lever-age/api/tree/feature/v2). To leave the docker container, type 'exit'  -- but note that this connand does not stop tye container! 

#### To stop containers, see which runs are running with the 'ps' command above, then type:

```
docker stop CONTAINER_ID
```



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
