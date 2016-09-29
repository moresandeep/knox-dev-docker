# Knox Dev Docker Container

Builds a Knox container by checking out the source code from the the git url provided in Dockerfile. Can be pointed directly to master or a branch to checkout and build a specific release or version.

An example of Zeppelin UI is provided for reference, this by no means is complete and should be used as a reference.

Note - First run will take a long time depending on the docker settings (CPU and Memory) as we'll be downloading
the sources and dependencies from maven and building it. An unfortunate side effect of this is the image size.


## Pre-Req
* Zeppelin running at http://localhost:9995

## Getting Started
* `docker-compose up -d`
* Go to https://localhost:8443/gateway/default/zeppelin/
