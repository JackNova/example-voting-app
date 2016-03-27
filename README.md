Example Voting App
==================

# Development

To have the possibility to update sources and immediately see changes reflected on the running container it is necessary to share folders on the host machine. This is done using volumes.

**db**
docker run -d -v db-data:/var/lib/postgresql/data --name db postgres:9.5

**redis**
docker run -d -p 6379 --name redis redis

**worker**
docker build -t example-voting/worker worker/
docker run -d --name worker -v $(pwd)/worker/src:/code/src --link db:db --link redis:redis example-voting/worker

**voting app**
docker build -t example-voting/voting voting-app/
docker run -d --name voting -v $(pwd)/voting-app/:/app -p 5000:80 --link redis:redis example-voting/voting

**result app**
docker build -t example-voting/result result-app/
docker run -d --name result -v $(pwd)/result-app:/app -p 5001:80 --link db:db example-voting/result

# Upstream Documentation

This is an example Docker app with multiple services. It is run with Docker Compose and uses Docker Networking to connect containers together. You will need Docker Compose 1.6 or later.

More info at https://blog.docker.com/2015/11/docker-toolbox-compose/

Architecture
-----

* A Python webapp which lets you vote between two options
* A Redis queue which collects new votes
* A Java worker which consumes votes and stores them in…
* A Postgres database backed by a Docker volume
* A Node.js webapp which shows the results of the voting in real time

Running
-------

Run in this directory:

    $ docker-compose up

The app will be running on port 5000 on your Docker host, and the results will be on port 5001.

Docker Hub images
-----------------

Docker Hub images for services in this app are built automatically from master:

 - [docker/example-voting-app-voting-app](https://hub.docker.com/r/docker/example-voting-app-voting-app/)
 - [docker/example-voting-app-result-app](https://hub.docker.com/r/docker/example-voting-app-result-app/)
 - [docker/example-voting-app-worker](https://hub.docker.com/r/docker/example-voting-app-worker/)
