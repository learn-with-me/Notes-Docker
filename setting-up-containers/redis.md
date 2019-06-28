# Redis

Goal of this page is to document running a Redis container in local and accessing it with your application from outside

### Steps to setup a running container

#### Step 0

Make sure you have the following installed:

1. Docker

List below is not required, I am just doing it to play around

1. Redis - `brew install redis`

#### Step 1

```
Start a new running container
$ docker run -d -p 6379:6379 --name test-redis redis

Check it is running
$ docker ps

View the log output
$ docker logs test-redis
```

#### Step 2

```
Start an interactive session inside a running container
$ docker exec -it test-redis sh

Run redis-cli inside the container
# redis-cli

Try some basic commands
127.0.0.1:6379> ping
PONG
127.0.0.1:6379> set name mark
OK
127.0.0.1:6379> get name
"mark"
127.0.0.1:6379> incr counter
(integer) 1
127.0.0.1:6379> incr counter
(integer) 2
127.0.0.1:6379> get counter
"2"
127.0.0.1:6379> exit
# exit
```

#### Step 3

```
Start another container, called test-client, based off the redis image.
We're only using it to run the redis-cli. This means it won't be running Redis itself.
We specify --rm so it will delete itself after the shell exits.
$ docker run -it --rm --link test-redis:redis --name test-client redis sh

Now in this container let's start redis-cli and connect to test-redis which has the name redis:
# redis-cli -h redis
redis:6379>

Issue some commands to check the keys we created earlier should still be there
redis:6379> get name
"mark"
redis:6379> get counter
"2"

Exit out of redis-cli and sh. This has actually caused our test-client container to delete itself.
redis:6379> exit
# exit

Connect from another linked container

```

### References

```
Installation
https://medium.com/@petehouston/install-and-config-redis-on-mac-os-x-via-homebrew-eb8df9a4f298

Commands
https://redis.io/topics/rediscli

Docker Container
https://markheath.net/post/exploring-redis-with-docker
https://www.oskarlind.com/running-a-redis-instance-on-docker/

```



