Running Certbot in Docker 
=========================

Docker is an amazingly simple and quick way to obtain a certificate. However, this mode of operation is unable to install certificates automatically or configure your webserver, because our installer plugins cannot reach your webserver from inside the Docker container.
 
**Most users should install Certbot by following the installation instructions at https://certbot.eff.org/instructions. You should only use Docker if you are sure you know what you are doing (you understand [volumes](https://docs.docker.com/storage/volumes/)) and have a good reason to do so, such as following the [one service per container rule](https://docs.docker.com/config/containers/multi-service_container/).**

For more information, please read [Certbot - Running with Docker](https://certbot.eff.org/docs/install.html#running-with-docker).


Certbot Docker Tools
======================

Goal
----

This code is used to build and deploy new versions of the Certbot and Certbot
DNS plugin Docker images to Docker Hub.

High-level behavior
-------------------

Running `./build.sh <TAG> all` causes the Docker images to be built for all 
supported architectures, where `<TAG>` is the base of the tag that should be 
given to the generated images. The tag should either be `nightly` or a git 
version tag like `v2.2.0`. The given tag is only the base of the tag because 
the CPU architecture is also added to the tag. For version tags above `v2.0.0`,
Additional tags for `latest` are also generated. The generated images are stored 
in the local docker image cache.

Running `./deploy_by_arch.sh <TAG> all && ./deploy_multiarch.sh <TAG>` will 
push the previously generated images to Docker Hub and then generate multi-arch
manifests for easy access to the underlying images appropriate for a given 
architecture.

Configuration
-------------

To run these scripts you need:

1. An x86_64 machine with Docker installed and the Docker daemon running. You probably don't want to use the docker snap as these scripts have failed when using that in the past.
2. To be logged into Docker Hub with an account able to push to the Certbot and Certbot DNS Docker images on Docker Hub. Altering the value of `DOCKER_HUB_ORG` in `lib/common` will allow you to push to your own account for testing.
