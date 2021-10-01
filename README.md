# Welcome to DateLife's docker GitHub repository!

![Docker Pulls](https://img.shields.io/docker/pulls/bomeara/datelife?color=green)
![Docker Cloud Build Status](https://img.shields.io/docker/cloud/build/bomeara/datelife)
![Docker Automated build](https://img.shields.io/docker/automated/bomeara/datelife?color=green)
[![NSF-1458603](https://img.shields.io/badge/NSF-1458603-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1458603)
[![NSF-0905606](https://img.shields.io/badge/NSF-0905606-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=0905606)
[![NSF-1458572](https://img.shields.io/badge/NSF-1458572-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1458572)

`datelife` is an R package that gets all available information on time of divergence of a group of lineages, and uses this information to date a phylogenetic tree.
`datelife` functionalities can be accessed via its [code](https://github.com/phylotastic/datelife), or through its [website application]().
`datelife`'s webiste app is [developed with shiny apps and is containerized with docker](https://www.r-bloggers.com/2021/05/dockerizing-shiny-applications/). To "dockerize" the `datelife` web app we followed a bunch of [resources](#resources).
Having a "dockerized" application means that you can set up your own `datelife` server webb app anywhere. There are a few alternative ways to do this. For all of them you require [Docker Desktop installed](https://www.docker.com/products/docker-desktop). Below you can find instructions on:

1. [setting up a prebuilt docker image](#1-using-a-prebuilt-datelife-docker-image),
2. [building a local image on your own](#2-building-your-own-datelife-docker-image),
3. [running multiple instances of a shiny app](#3-running-multiple-instances), and
4. [using swarm](#4-swarm).

## 1. Using a prebuilt DateLife docker image

Ideally, docker hub automatically builds the DateLife docker image at [bomeara/datelife](https://hub.docker.com/r/bomeara/datelife/dockerfile) with pushes to the GitHub [phylotastic/datelifedocker repo](https://github.com/phylotastic/datelifedocker).

  1.1) To get that prebuilt DateLife docker image you need to have [Docker Desktop installed](https://www.docker.com/products/docker-desktop). Then, get the prebuilt DateLife docker image from Docker Hub with `docker pull`:

```shell
    docker pull bomeara/datelife
```

  1.2) Now you can start the image using `docker run`. You can do this in "bash mode" so you can look around (i.e., in the /srv dir for the shiny app):

```shell
    docker run -t -i -p 80:3838 bomeara/datelife sh -c '/bin/bash'
```

Once you've finished looking around, just type `exit` and you will be logged out.

  1.3) To start the image in serve mode, do:

```shell
    docker run -t -i -d -p 80:3838 datelife
```

Go to http://localhost on any browser to checkout your Datelife website and shiny app running.

Argument `-d` is optional, it stands for `--detach`, allowing you to keep on using your terminal while the DateLife website is being served.


## 2. Building your own DateLife docker image

This is very useful for debugging. If you tried setting up a prebuilt docker image and it fails, this is the way to go.

2.1. Make sure you have [Docker desktop installed](https://www.docker.com/products/docker-desktop). Then, download [datelifedocker repository](https://github.com/phylotastic/datelifedocker) to your computer. One way is to type from the terminal:

```shell
    git clone https://github.com/phylotastic/datelifedocker.git
```

2.2. Change directories to your newly created datelifedocker directory using `cd`, and build the DateLife docker image with:

```shell
    docker build -t datelife .
```

To build with no cache type `docker build -t datelife --no-cache .`

2.3. Now you can start the DateLife server from your newly created docker image with:

```shell
    docker run -t -i -p 80:3838 datelife
```

Go to http://localhost on any browser to checkout your Datelife shiny app running.

To stop serving, type `Ctrl + c` or `exit`

2.4. To explore the contents of the DateLife docker image on terminal, you can do:

```shell
docker run -t -i datelife sh -c '/bin/bash'
```

If you want to explore a particular container, first list them all with `docker container ls`, and identify the `container-name` of the one you are interested in.

Then restart your container and execute it with:

```shell
docker container restart <container-name>
docker exec -it <container-name> sh -c '/bin/bash'
```

For more tips see [how-do-i-get-into-a-docker-containers-shell](https://stackoverflow.com/questions/30172605/how-do-i-get-into-a-docker-containers-shell)
and [15-docker-commands-you-should-know](https://towardsdatascience.com/15-docker-commands-you-should-know-970ea5203421).

2.5. After building and making changes, you can push (if you have permissions) the new DateLife docker image to docker hub with:

```shell
    docker tag datelife bomeara/datelife
    docker login
    docker push bomeara/datelife
```

## 3. Running multiple instances

You can run multiple shiny app instances with:

```shell
    docker-compose up -d --scale datelife=10 # for ten instances
```

## 4. Swarm

4.1 Go to node where swarm is being managed. You can change the number of workers in replicas in docker-compose-swarm.yml

```shell
    sudo docker stack deploy --compose-file docker-compose-swarm.yml datelife
```

4.2. See how it's doing with

```shell
    sudo docker stack services datelife
```

4.3. And stop it with

```shell
    sudo docker stack rm datelife
```

For domain, *, @, www all resolve to `datelife19.campus.utk.edu.` (yes, with a period after edu) using CNAME

## Resources

- https://www.r-bloggers.com/2021/05/dockerizing-shiny-applications/
- https://github.com/flaviobarros/shiny-wordcloud/blob/master/Dockerfile
- https://www.statworx.com/at/blog/how-to-dockerize-shinyapps/
- https://www.youtube.com/watch?v=ARd5IldVFUs
- https://hub.docker.com/u/rocker
- https://www.dabbleofdevops.com/blog/deploy-rshiny-with-the-rocker-shiny-docker-image
- https://blog.sellorm.com/2021/04/25/shiny-app-in-docker/
- https://juanitorduz.github.io/dockerize-a-shinyapp/
- https://bhoom.wordpress.com/2020/06/18/using-multiple-r-versions-with-docker-rocker-rstudio-project/
- https://githubmemory.com/repo/rocker-org/shiny/issues/92
- https://stackoverflow.com/questions/30172605/how-do-i-get-into-a-docker-containers-shell


## Docker info

<details>
<summary>Docker version info. Obtained with <code>docker version</code> command.</summary>


```shell
Client: Docker Engine - Community
 Cloud integration: 1.0.7
 Version:           20.10.2
 API version:       1.41
 Go version:        go1.13.15
 Git commit:        2291f61
 Built:             Mon Dec 28 16:12:42 2020
 OS/Arch:           darwin/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.2
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       8891c58
  Built:            Mon Dec 28 16:15:28 2020
  OS/Arch:          linux/amd64
  Experimental:     true
 containerd:
  Version:          1.4.3
  GitCommit:        269548fa27e0089a8b8278fc4fc781d7f65a939b
 runc:
  Version:          1.0.0-rc92
  GitCommit:        ff819c7e9184c13b7c2607fe6c30ae19403a7aff
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```

</details>
