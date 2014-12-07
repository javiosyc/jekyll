---
layout: post
title: "PPA for Ubuntu"
description: "Personal Package Archives for Ubuntu"
category: ubuntu
tagline: ""
tags: [ ubuntu]
---
{% include JB/setup %}

## Personal Package Archives

Personal Package Archives (PPA) allow you to upload Ubuntu source packages to be built and published as an apt repository by Launchpad. 


### 安裝不是 Ubuntu 的預設的軟體，要新增第三方的PPA ( Personal Package Archive ) 才能用 apt-get install


>>1.PPA 的網址加到 /etc/apt/sources.list 

`RUN echo "deb http://archive.ubuntu.com/ubuntu trusty main universe" > /etc/apt/sources.list`

or

`RUN wget -O - http://packages.elasticsearch.org/GPG-KEY-elasticsearch |  apt-key add -`
`RUN echo 'deb http://packages.elasticsearch.org/logstash/1.4/debian stable main' > /etc/apt/sources.list.d/logstash.list`

>>2.add-apt-repository

`add-apt-repository ppa:launchpad`

### add-apt-repository is not default packages

`apt-get install python-software-properties`
