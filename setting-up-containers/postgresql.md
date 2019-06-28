# PostgreSQL

Goal of this page is to document running a PostgreSQL container in local and accessing it with your application from outside

### Steps to setup a running container

#### Pre-requisite

Make sure you have docker installed.

#### Step 1 - start a new running container

```
Start a new running container
$ docker run -d -p 5432:5432 -v postgres-data:/var/lib/postgresql/data --name test-postgres postgres

• We're running detached (-d) mode (so in the background).
• We're also going to mount a volume (with -v), which will be used to store the database we create.
• PostgreSQL stores its data in /var/lib/postgresql/data, so we're mounting our volume to that path.
• The volume name will be postgres-data, and Docker will automatically create it
   (just using storage on the Docker host's local disk) if a volume with this name doesn't already exist.

Check it is running
$ docker ps

View the log output
$ docker logs test-postgres
```

#### Step 2 - create a database

```
Launch an interactive shell running inside our postgres1 container
$ docker exec -it test-postgres sh

Create a new database with the name mydb
# createdb -U postgres mydb

Launch the psql utility which is a CLI tool for PostgreSQL
# psql -U postgres mydb
```

#### Step 3 - explore the database

```
Now inside psql, let's run some basic commands. \l lists the databases.
mydb=# \l
mydb=# select version();
mydb=# select current_date;

Create a table
mydb=# CREATE TABLE people (id int, name varchar(80));
CREATE TABLE

Insert a row
mydb=# INSERT INTO people (id,name) VALUES (1, 'Mark');
INSERT 0 1

Quit from psql with \q and exit from our shell
mydb=# \q 
# exit
```

#### Step 4 - link another container

```
Let's test from another linked container
$ docker run -it --rm --link test-postgres:pg --name test-client postgres sh

Launch psql but connect to the other container (-h) which we've given the name pg in our link configuration
# psql -U postgres -h pg mydb

From this test-client container we can access data in the database stored in the test-postgres container
mydb=# SELECT * FROM people;
 id | name 
----+------
  1 | Mark
(1 row)

Quit from psql and exit from our shell
mydb=# \q 
# exit
```

#### Step 5 - inspect the volume

```
Find out information about the volume, including where on our local disk the data is being stored. 
$ docker volume inspect postgres-data
```

#### Step 6 - attach an existing volume to a new container

```
$ docker run -d -p 5432:5432 -v postgres-data:/var/lib/postgresql/data --name test2-postgres postgres
```

#### Step 7 - cleanup everything

```
$ docker rm -f test-postgres
$ docker rm -f test2-postgres
$ docker volume rm postgres-data
```

### References

```
Docker
https://markheath.net/post/exploring-postgresql-with-docker
```



