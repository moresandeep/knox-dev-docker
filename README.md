# Knox Dev Docker Container

Builds a Knox container by checking out the source code from the the git url provided in `docker-compose*.yml`. Can be pointed directly to master or a branch to checkout and build a specific release or version.

An example of Zeppelin UI is provided for reference, this by no means is complete and should be used as a reference.

Note - First run will take a long time depending on the docker settings (CPU and Memory) as we'll be downloading
the sources and dependencies from maven and building it. An unfortunate side effect of this is the image size.

## Pre-Req
* docker-compose version 2 required.
* No proxy.

## Knox dev container
Basic Knox dev container example using docker-compose, mainly as a base to build on.
Change the git command in `docker-compose.yml` to build from a specific release.

* `docker-compose up -d`

## Demo: Knox dev container + Zeppelin  
An example of Knox proxying Zepplin UI and websocket connections.
Change the git command in `docker-compose-zeppelin.yml` to build from a specific release.
Also refer to the `docker-compose-zeppelin.yml` file for more settings.

* `docker-compose -f docker-compose-zeppelin.yml up -d`
*  Go to `https://localhost:8443/gateway/sandbox/zeppelin/`
