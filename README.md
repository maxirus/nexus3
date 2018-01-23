# Nexus3 Docker Image

## Overview

This Docker image was built to reduce the size of the official Nexus3 Docker image and to add AWS S3 Blobstore support. There are a couple different images to choose from based on your individual needs.

* maxirus/nexus3:<nexus_version>-alpine - Nexus3 using Alpine Linux
* maxirus/nexus3:<nexus_version>-alpine-s3 - Nexus3 using Alpine Linux with S3 Blobstore installed
* maxirus/nexus3:<nexus_version>-centos-s3 - Nexus3 using the official Nexus3 image with S3 Blobstore installed

## Getting Started

```
docker run -d -p 8081:8081 --ulimit nofile=65536:65536 --name nexus maxirus:latest-alpine-s3
```

After a few mins, you'll be able to access you Nexus3 repo via [http://localhost:8081](http://localhost:8081). The default username/password is ``admin/admin123``

## Config

Please see the [official repo](https://github.com/sonatype/docker-nexus3) and the Nexus3 [documentation](https://help.sonatype.com/display/NXRM3/Installation) for more info