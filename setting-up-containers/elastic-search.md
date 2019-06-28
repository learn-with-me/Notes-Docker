# Elastic Search

Goal of this page is to document running a Elastic Search container in local and accessing it with your application from outside

### Steps to setup a running container

#### Pre-requisite

Make sure you have docker installed.

#### Step 1 - start a new running container - docker run

```
To try out Elasticsearch running in a single node, we can do with docker run command shown below:

We're exposing ports 9200 (for the REST API)
Setting up a single node cluster (using an environment variable), from official elasticsearch 6.4.2 image
Also showing how to set up a volume to store the index data in.

$ docker run -d -p 9200:9200 -e "discovery.type=single-node" \
        -v esdata:/usr/share/elasticsearch/data \
        docker.elastic.co/elasticsearch/elasticsearch:6.4.2

And all the Elasticsearch commands we run with curl will work just fine on this single container.
```

#### Step 1 - start a new running container - docker compose

```
With docker-compose we can declare all the containers that make up an application in a YAML format.
For each container we can also configure the environment variables that should be set
And any volumes that are required
And define a network to allow the services to communicate with each other.

Example below defines a simple two-node cluster, and each node in the cluster has a volume so that our
indexes can live independently of our containers, and survive upgrades

To download this file locally as docker-compose-v1.yml you can use the following command:
curl https://gist.githubusercontent.com/markheath/f246ec3aa5a3e7493991904e241a416a/raw/c4fa64575bc854e34a2506291bd14033caf5e9b6/docker-compose-v1.yml > docker-compose-v1.yml
```

```
version: '2.2'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.1
    container_name: elasticsearch
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - esnet
  elasticsearch2:
    image: docker.elastic.co/elasticsearch/elasticsearch:6.4.1
    container_name: elasticsearch2
    environment:
      - cluster.name=docker-cluster
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
      - "discovery.zen.ping.unicast.hosts=elasticsearch"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - esdata2:/usr/share/elasticsearch/data
    networks:
      - esnet

volumes:
  esdata1:
    driver: local
  esdata2:
    driver: local

networks:
  esnet:
```

```
We can use the docker-compose up command to start up the containers
Create all necessary resources like networks and volumes.
We're using -d to run in the background just like we can with docker run

$ docker-compose -f docker-compose-v1.yml up -d
```

#### Step 2 - check cluster health

```
We've exposed port 9200 on one of those containers
This allows us to query the cluster health with the following request:

$ curl http://localhost:9200/_cluster/health?pretty
```

#### Step 3 - create an index

```
Create an index called customer
$ curl -X PUT "localhost:9200/customer?pretty"
```

#### Step 4 - add a new document

```
$ curl -X PUT "localhost:9200/customer/_doc/1?pretty" -H 'Content-Type: application/json' -d'{"name": "Mark Heath" }'
```

#### Step 5 - view documents in index

```
$ curl localhost:9200/customer/_search?pretty
```

#### Step 6 - upgrade cluster to 6.4.2

```
Suppose we now want to upgrade the nodes in our cluster to Elasticsearch 6.4.2
What we can do is update our YAML file with new container version numbers.

Download the updated file from:
$ curl https://gist.githubusercontent.com/markheath/f246ec3aa5a3e7493991904e241a416a/raw/c4fa64575bc854e34a2506291bd14033caf5e9b6/docker-compose-v2.yml > docker-compose-v2.yml

Before we upgrade our cluster, take a look at the container ids that are currently running with docker ps.
These containers are not going to be "upgraded" - they're going to be disposed, and new containers
running 6.4.2 will be created. However, the data is safe, because it's stored in the volumes.
The volumes won't be deleted, and will be attached to the new containers.

To perform the upgrade we can use the following command.
$ docker-compose -f docker-compose-v2.yml up -d

We should see it saying "recreating elasticsearch" and "recreating elasticsearch2" as it discards the
old containers and creates new ones.
Now if we run docker ps again we'll see new container ids and new image versions.
```

#### Step 7 - check if our index is still present

```
To ensure that our index is still present we can search again and check our document is still present.
$ curl localhost:9200/customer/_search?pretty

Let's add another document into the index with:
curl -X PUT "localhost:9200/customer/_doc/2?pretty" -H 'Content-Type: application/json' -d'{"name": "Steph Heath"}'
```

#### Step 8 - upgrade to a three node cluster

```
Something important to note is that we need to set the discovery.zen.minimum_master_nodes=2 environment
variable to avoid split brain problems.

Download the updated YAML file from:
$ curl https://gist.githubusercontent.com/markheath/f246ec3aa5a3e7493991904e241a416a/raw/a2685d1bf0414acbc684572d00cd7c7c531d0496/docker-compose-v3.yml > docker-compose-v3.yml

And then we can upgrade our cluster from two to three nodes with
$ docker-compose -f docker-compose-v3.yml up -d

The change of environment variable means that we will recreate both elasticsearch and elasticsearch2,
and of course the new elasticsearch3 container and its volume will get created.

Check the cluster status and if all went well, we'll see a cluster size of three
$ curl http://localhost:9200/_cluster/health?pretty

Let's check our data is still intact by retrieving a document by id from our index
$ curl -X GET "localhost:9200/customer/_doc/1?pretty"
```

#### Step 9 - add Kibana and Head plugin

```
Download the YAML file by:
$ curl https://gist.githubusercontent.com/markheath/f246ec3aa5a3e7493991904e241a416a/raw/a2685d1bf0414acbc684572d00cd7c7c531d0496/docker-compose-v4.yml > docker-compose-v4.yml

And now we can update our cluster again with
$ docker-compose -f docker-compose-v4.yml up -d
```

#### Step 10 - try out Kibana

```
Once we've done this, we can visit the Kibana site by browsing to localhost:5601
If you were following along in "Play with Docker" then you'll see special links appear for each port
that is exposed (9200, 9100 and 5601).
```

#### Step 11 - clean up

```
To stop and delete all the containers:
$ docker-compose -f docker-compose-v4.yml down

And if you want to delete the volumes as well (so all index data will be lost), add the -v flag:
docker-compose -f docker-compose-v4.yml down -v
```

### References

```
Elastic Search Guide
https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html

Docker
https://markheath.net/post/exploring-elasticsearch-with-docker

Split Brain Problem
https://www.elastic.co/guide/en/elasticsearch/reference/2.3/modules-node.html#split-brain

Other sources
https://blog.ruanbekker.com/blog/2018/04/29/running-a-3-node-elasticsearch-cluster-with-docker-compose-on-your-laptop-for-testing/
```



