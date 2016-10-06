# Knox Dev Docker Container

Builds a Knox container by checking out the source code from the the git url provided in `docker-compose*.yml`. Can be pointed directly to master or a branch to checkout and build a specific release or version.

An example of Zeppelin UI is provided for reference, this by no means is complete and should be used as a reference.

Note - First run will take a long time depending on the docker settings (CPU and Memory) as we'll be downloading
the sources and dependencies from maven and building it. An unfortunate side effect of this is the image size.

## Pre-Req
* docker-compose version 2 required.
* No proxy.
* Please refer to this excellent guild - [Proxying UI using Knox](https://cwiki.apache.org/confluence/display/KNOX/Proxying+a+UI+using+Knox)
* For more information and explanation on Knox please refer the [Dev guide](https://knox.apache.org/books/knox-0-9-1/dev-guide.html)

## Build Knox dev container
Build a Knox dev container using docker-compose. we can use this as a base image to build on.

* Checkout this repository `git clone https://github.com/moresandeep/knox-dev-docker.git`
* `cd knox-dev-docker`
* `docker-compose up -d` This builds the Knox dev container if not already present by checking out code from the provided git repo (in docker-compose.yml) and starts an docker container with test ldap service running and starts a container with knox gateway service running.
* `docker ps` should show your gateway and ldap containers running.
* run `docker-compose stop` to stop the containers.
* To stops containers and removes containers, networks, volumes, and images run `docker-compose down`.

### Settings
* Knox gateway listening on port 8443, to change it, modify the `ports:` setting in `docker-compose.yml`.
* Following is the folder mapping:
  * `./topologies` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/conf/topologies` folder in the container.   
  * `/zeppelinui` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/data/services/zeppelinui` folder in the container.
  * `/zeppelinws` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/data/services/zeppelinws` folder in the container.
* To add and remove mappings use the `volumes:` settings in `docker-compose.yml`.
* Change the git command in `docker-compose.yml` to build from a specific release.

## Demo: Knox dev container + Zeppelin  
An example of Knox proxying Zepplin UI and websocket connections.
Change the git command in `docker-compose-zeppelin.yml` to build from a specific release.
Also refer to the `docker-compose-zeppelin.yml` file for more settings.

* `docker-compose -f docker-compose-zeppelin.yml up -d`
*  Go to `https://localhost:8443/gateway/sandbox/zeppelin/`
