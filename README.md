# Knox Dev Docker Container

Build a development environment that can be used to develop service definitions for introducing UI and REST API proxying capabilities for Apache Knox.

## Pre-Req
* docker-compose version 2 required.
* No proxy.
* To understand UI Proxying with Knox please refer to this excellent guide - [Proxying UI using Knox](https://cwiki.apache.org/confluence/display/KNOX/Proxying+a+UI+using+Knox)
* For more information and explanation on Knox please refer to the [Dev guide](https://knox.apache.org/books/knox-0-9-1/dev-guide.html)

## Build Knox dev container
Build a Knox dev container using docker-compose. we can use this as a base image to build on.

* Checkout this repository `git clone https://github.com/moresandeep/knox-dev-docker.git`
* `cd knox-dev-docker`
* `docker-compose up -d` This builds the Knox dev container if not already present by checking out code from the provided git repo (in docker-compose.yml) and starts an docker container with test ldap service running and starts a container with knox gateway service running.
* `docker ps` should show your gateway and ldap containers running.
* run `docker-compose stop` to stop the containers.
* To stops containers and removes containers, networks, volumes, and images run `docker-compose down`.

### Settings
This section talks about common configuration settings used by Knox dev container and how they can be changed.
* Knox gateway listening on port 8443, to change it, modify the `ports:` setting in `docker-compose.yml`, note the format is `HOST-PORT:CONTAINER-PORT`.
* Following is the folder mapping:
  * `./topologies` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/conf/topologies` folder in the container.   
  * `/zeppelinui` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/data/services/zeppelinui` folder in the container.
  * `/zeppelinws` folder will be mapped to `/knox/knox-0.10.0-SNAPSHOT/data/services/zeppelinws` folder in the container.
* To add and remove mappings use the `volumes:` settings in `docker-compose.yml`.
* Change the git command in `docker-compose.yml` to build from a specific release.

## Adding new services
Here we describe how a new service can be added. We will need the containers to be stopped in order for the new service to be added this is because we will be adding new mappings to our existing containers (e.g. volumes and/or ports).

* Please refer to [Proxying UI using Knox](https://cwiki.apache.org/confluence/display/KNOX/Proxying+a+UI+using+Knox) to understand the basic concepts, especially how to add services (rewrite.xml, service.xml) and how to update the topology file.

* We will try to add a new service to Knox, let's take an example of adding a Weather service as described here - [Adding a service to Apache Knox](https://cwiki.apache.org/confluence/display/KNOX/2015/12/17/Adding+a+service+to+Apache+Knox). Make sure you get the App Id from http://openweathermap.org (if using this example).

* Under the `knox-dev-docker` folder (where this repository is checked out) add the service definition files needed by your service e.g.  
  * `weather/0.0.1/service.xml` - The service.xml file defines the high level URL patterns that will be exposed by the gateway for a service.
  *  `weather/0.0.1/rewrite.xml` - The rewrite.xml is configuration that drives the rewrite provider within Knox.

* Now go to `topologies/sandbox.xml` file on your machine and add the following service definition
`
<service>
    <role>WEATHER</role>
    <url>http://api.openweathermap.org:80</url>
</service>
`
* Add the following line to the `volumes:` section in `docker-compose.yml` file
`
- ./weather:/knox/knox-0.10.0-SNAPSHOT/data/services/weather
`
* Start the docker containers `docker-compose up -d`
* curl -ku guest:guest-password 'https://localhost:8443/gateway/sandbox/weather/data/2.5/forecast/city?id=524901&APPID=<OPENWEATHERMAP_APPID>'
* You should see the output like:
`
{"city":{"id":524901,"name":"Moscow","coord":{"lon":37.615555,"lat":55.75222},"country":"RU","population":0,"sys":{"population":0}},"cod":"200","message":0.172,"cnt":40,"list":[{"dt":1475798400,"main":{"temp":282.64,"temp_min":282.64,"temp_max":282.644,"pressure":1014.9,"sea_level":1035.01,"grnd_level":1014.9,"humidity":94,"temp_kf":0},"weather":[{"id":500,"main":"Rain","description":"light rain","icon":"10n"}],"clouds":{"all":92},"wind":{"speed":3.71,"deg":84.5015},"rain":{"3h":0.8325},"sys":{"pod":"n"},"dt_txt":"2016-10-07 00:00:00"},{"dt":1475809200,"main":{"temp":282.3,"temp_min":282.296,"temp_max":282.3,"pressure":1013.21,"sea_level":1033.38,"grnd_level":1013.21,"humidity":97,"temp_kf":0},"weather":[{"id":500,"main":"Rain","description":"light rain","icon":"10n"}],"clouds":{"all":92} ...
`

### Demo: Knox dev container + Zeppelin  
An example of Knox proxying Zeppelin UI and websocket connections. This example is already configured and runs out of the box.

This example demos how Knox can proxy Zeppelin UI and proxy underlying Websocket connections using the necessary service files and service definitions.

You can change the git command in `docker-compose-zeppelin.yml` to build from a specific release if you choose.
Also refer to the `docker-compose-zeppelin.yml` file for more settings if you want more control on the docker instances.

Instructions to run Knox Zeppelin demo:
* `docker-compose -f docker-compose-zeppelin.yml up -d`
*  Go to `https://localhost:8443/gateway/sandbox/zeppelin/`
