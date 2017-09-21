# Leverage Development Environment

Before getting started:

The DB container needs a SQL script to
bootstrap the database with. Place a
database dump in `volumes/db/docker-entrypoint-initdb.d`,
and make sure that the first line of the script is
`USE leverage;`. Remove other `USE` statements
if necessary.

Getting started:

```
git submodule update --init
docker-compose build
docker-compose up -d
```
