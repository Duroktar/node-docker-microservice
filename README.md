# node-docker-microservice

Learn Docker by building a Microservice!

This is a companion project to my article on '[Learn Docker by building a Microservice](http://www.dwmkerr.com/learn-docker-by-building-a-microservice/)' demonstrating key concepts of Docker using a Node.js microservice as an example.

# Pre-requisites

You must have Docker installed for this code to work! Check the [Installation Guide](https://docs.docker.com/engine/installation/) if you haven't got it installed.

# If using windows
```CMD
@FOR /f "tokens=*" %i IN ('docker-machine env --shell cmd') DO @%i  #Run this to set the docker env vars
docker-machine start                                                #Run this if docker isn't started yet
docker ps                                                           #Make sure you dont get any errors when you run this
docker-machine env --shell cmd                                      #Optional: Run this to see what happened behind that for loop```

# MySQL
If MySQL is not installed locally then run this:
```docker run --name db -d -e MYSQL_ROOT_PASSWORD=123 -p 3306:3306 mysql:latest```

# Coding

To start or stop the test database, build the test-database image and run it:

```bash
cd ./test-database
docker build -t test-database .
docker run --name mysql -it -p 3306:3306 test-database #Runs interactive
docker run --name mysql -d -p 3306:3306 test-database #Runs in background
```

Some commands for working with the test server:

```bash
cd ./users-service
npm install         # setup everything
npm test 			      # unit test - no need for a test database running
npm start           # run the server - you must have a test database running
npm run debug       # run the server in debug mode, opens a browser with the inspector
npm run lint        # check to see if the code is beautiful
```

You can also run the test server in its own container:

```bash
docker build -t users-service .
docker run -it -p 8123:8123 --link db:db -e DATABASE_HOST=DB users-service
```

# Integration Testing

To test the entire stack, run:

```bash
docker-compose build
docker-compose -d up
sleep 10 # give the database server enough time to start!
cd integration-test && npm start && cd ..
docker-compose -d down
```
