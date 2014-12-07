---
layout: post
title: "Build a Docker container with the gerrit code review system"
description: ""
category: docker
tags: [docker, git, gerrit]
---
{% include JB/setup %}

## Build on top of Ubuntu Trusty (14.04) and gerrit 2.8.5

[Dockerfile](https://github.com/javiosyc/testdocker/tree/master/gerrit-docker)

`docker build -t javio/gerrit .`

## gen All-Projects.git 
`docker run  -p 8099:8080 -p 49418:29418 -d -t --name gerrit javio/gerrit`

`docker run --rm --volumes-from gerrit -v $(pwd):/home/gerrit/gerrit/git_bk/ ubuntu cp -r /home/gerrit/gerrit/git/. /home/gerrit/gerrit/git_bk`

## Backup Git Repository
`docker run --rm --volumes-from gerrit -v $(pwd):/home/gerrit/gerrit/git_bk/ ubuntu cp -r /home/gerrit/gerrit/git/. /home/gerrit/gerrit/git_bk`

## Run Gerrit
`docker run -p 8099:8080 -p 49418:29418 -d -t --name gerrit -v $(pwd):/home/gerrit/gerrit/git/ javio/gerrit`



