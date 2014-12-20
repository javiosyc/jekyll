---
layout: post
title: "Launching Jekyll site using docker"
description: "Launching Jekyll site using docker"
category: docker
tagline: ""
tags: [docker, jekyll]
---
{% include JB/setup %}

## Get Source Code For My Blog

`cd $workspace`

`git clone https://github.com/javiosyc/jekyll.git`


## Compile Source Code And Build It

`cd jekyll`

`docker run -v $(pwd):/data/ --name javio_blog jamtur01/jekyll`

`docker run -d -p 8099:80 --volumes-from javio_blog jamtur01/apache`


## Updating The Jekyll Site

`docker start javio_blog`

## Backing Up The Jekyll volume

`docker run --rm --volumes-from javio_blog -v $(pwd):/backup/ ubuntu 
tar cvf /backup/javio_blog.tar /var/www/html`