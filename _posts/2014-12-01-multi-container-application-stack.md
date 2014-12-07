---
layout: post
title: "multi container application stack"
description: "multi container application stack"
category: docker
tags: [ docker]
---
{% include JB/setup %}


## A Multi-Container Application Stack

>> 1. A node container (jamtur01/nodejs)
>> 2. A redis primary container (jamtur01/redis_primary)
>> 3. Two Redis replica containers (jamtur01/redis_replica)
>> 4. A logger container to capture our applcation logs.

> start redis primary container

>> `docker run -d -h redis-primary --name redis-primary jamtur01/redis_primary`

> get the log in the redis primary
>> `docker run -ti -rm -volumes-from redis_primary ubuntu cat /var/log/redis/redis-server.log`

> start redis replica container

>> `docker run -d -h redis_replica1 --name redis_replica1 --link redis_primary:redis_primary jamtur01/redis_replica`

>> `docker run -d -h redis_replica2 --name redis_replica2 --link redis_primary:redis_primary jamtur01/redis_replica`

> get the log in the redis_replica
>> `docker run -ti -rm -volumes-from redis_replica1 ubuntu cat /var/log/redis/redis-replica.log`

>> `docker run -ti -rm -volumes-from redis_replica2 ubuntu cat /var/log/redis/redis-replica.log`

> start node container
>> `docker run -d --name nodeapp -p 3000:3000 --link redis_primary:redis_primary jamtur01/nodejs`

docker run -d --name logstash --volumes-from redis_primary --volumes-from nodeapp jamtur01/logstash
