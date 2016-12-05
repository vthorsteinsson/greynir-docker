# Docker images for Greynir
## A natural language processor for Icelandic
https://github.com/vthorsteinsson/Reynir

Docker-compose will run the Greynir application cluster. This consists of two Docker
images, a *database image* (exposing PostgreSQL at port 5432 by default) and a *web image*
(exposing a HTTP server on port 5000 by default).

## Third party data
Make sure that Icelandic dictionary files from BÍN (*Beygingarlýsing íslensks nútímamáls*)
are added to the `greynir-docker-db` directory before building the database container.
These files are not included in the Greynir distribution and must be obtained separately.

See the Greynir README for further details.

https://github.com/vthorsteinsson/Reynir

## How to run
Attached to console: `docker-compose up`
Detached from console: `docker-compose -d up`

Attached-mode will display all output from the containers in the console.

# Building and running the images separately

## Building the database container image
If you already have PostgreSQL running on your server at port 5432, stop the service first
or you will get a port conflict. (Alternatively, configure
the Greynir database container to use a different port using the `GREYNIR_DB_PORT`
environment variable).

`cd greynir-docker/db`

Make sure that the files `obeyg.smaord.txt`, `plastur.feb2013.txt` and `SHsnid.csv` are present in the directory
before starting the build.

`docker build -t greynir/db .`

## Running the database container
This will expose the Greynir database on port 5432.

`docker run -d --name greynir_db -p 5432:5432 greynir/db`

## Building the web container image
`cd greynir-docker/web`

`docker build -t greynir/web .`

## Running the web container
The default settings will expose the web server on port 5000.

`docker run -d --name greynir_web -p 5000:5000 --link greynir_db greynir/web`


## Running several containers
`greynir_web` depends on the `GREYNIR_DB_HOST` and `GREYNIR_DB_PORT` environment variables. If there is a need to run multiple instances of Greynir you can set these variables when you run greynir_web.

Example:

`docker run --name <new_greynir_db> -p <new-db-port>:5432 greynir/db`

`docker run --name <new_greynir_web> -e "GREYNIR_DB_HOST=new_greynir_db" -p <new-web-port>:5000 --link <new_greynir_db> greynir/web`
