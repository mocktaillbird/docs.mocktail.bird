# 🍹 :bird: setup

Docker hub repository for [mocktailbird](https://cloud.docker.com/repository/docker/mocktailbird/mocktail-bird)

##### Tag Information:

​	<b>qa</b> : base tag for users to deploy mocktail bird on any machine with latest master.

​	<b>1.0.0</b> : Stable base release for production( Still underdevelopment)

#####  Set Up: 🏖️

1. Creating docker network bridge

   >docker network create -d bridge mongodb

   check if all is well:

   > docker network ls

2. Starting mongodb container

   > docker run -it -p 27017:27017 --network=mongodb  --name mocktail-bird-mongo mongo:3.6.11-stretch

   ​	<i>* network mongodb should be the one given as from the step 1 creating docker network driver.</i>

   ​	<i>* name mocktail-bird-mongo is the name of mongodb container.</i>

3. DB Authentication.

   > docker exec -it mocktail-bird-mongo /bin/bash

   the above command opens the conatiner folder and run ```mongo``` command. That will make you enter to mongodb console. enter below command to create a user to access the <b>mocktail</b> db

   ``` mongo js
   // enter into admin db
   use admin;
   db.getUsers();
   //create admin user to create new user and roles
   db.createUser( {
       user: "mockbird",
       pwd: "Ilovethisbird",
       roles: [ { role: "readWrite", db: "admin" },
       				{ role: "read", db: "reporting" } ]
     }
   );
   //create user to read, write record to moctail db
   db.createUser({
       user: "adam",
       pwd: "adamjohn",
       roles: [ { role: "readWrite", db: "mocktail" },
                { role: "read", db: "reporting" } ]
     }
   );
   //Authenticat the user
   db.auth("adam","adamjohn")
   ```

   check if mongodb conatiner created:

   > docker ps -a

   ​	<i>* If this container is restarted all the data is lost and all the steps 2,3 needs to be done again. Will be implementing —volume of docker container soon to avoid data loss.</i>


4. Starting mocktail-bird application container

   >  docker run -it -p 9080:9080 --network=mongodb --name mockatail-bird-qa mocktailbird/mocktail-bird:qa

   <i>	* network connects db and this application container.</i>
   <i> * name of this conatiner is mockatail-bird-qa.</i>
   <i>	* mocktailbird/mocktail-bird:qa  is the docker hub image for conatiner creation</i>

5. Check application running.

   >  http://localhost:9080/swagger-ui.html

   create a mock url using createmock api.

6. Have  🍹 :bird:  app to mock all your apis in few mins. 

