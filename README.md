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

## Getting Started

```
git submodule update --init
docker-compose build
docker-compose up -d
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
