
# MIGRATION TO THE СLOUD WITH CONTAINERIZATION. PART 1 – DOCKER &AMP; DOCKER COMPOSE

Untill now we have using Ec2 instance to install and deploy our softwares. While this is easy and fast, it has its own challenges. Consider that you have the requirement into two set of softwares with both needing different version of a dependency say java. This is will lead to conflict. In software speaks it is called dependency matrix.

Another problem encountered in software development is the problem of *IT WORKS IN MY COMPUTER* . This problem arises from the configuration drift between the developers computer and the testers computer.


All the problem highlighted above is solved by containerization. Container solves this problem by creating an isolated environmentment using Linux features like *NAMESPACE and CGROUP* for a process. 

containers are used to package application code, app configuration, dependencies and runtime environment required for running an application. This garanties to a large extent that the application runs efficiently and predictably on any environment it is deployed provider it has a container runtime.

### MySQL in container

- step 1: Pull MySQL Docker Image from Docker Hub Registry
  
  `docker pull mysql/mysql-server:latest`
  
   - List the images to check that you have downloaded them successfully:

     `docker image ls`
  
- Step 2: Deploy the MySQL Container to your Docker Engine

  ```
  docker run --name <container_name> -e MYSQL_ROOT_PASSWORD=<my-secret-pw> -d mysql/mysql-server:latest
  ```
  
   - Then, check to see if the MySQL container is running: Assuming the container name specified is mysql-server 

     `docker ps -a`
     
 ### CONNECTING TO THE MYSQL DOCKER CONTAINER
 
 ## Approach 1:
 
 Connecting directly to the container running the MySQL server:
 
 ```
 
 $ docker exec -it mysql bash

or

$ docker exec -it mysql mysql -uroot -p
```

Approach 2:

 - First, create a network:

   `docker network create --subnet=172.18.0.0/24 tooling_app_network`
   
 - create an environment variable to store the root password:

   `export MYSQL_PW=`
   
 - Then, pull the image and run the container, all in one command like below:

   ` docker run --network tooling_app_network -h mysqlserverhost --name=mysql-server -e MYSQL_ROOT_PASSWORD=$MYSQL_PW  -d mysql/mysql-server:latest`
   
## Connecting to the MySQL server from a second container running the MySQL client utility


   - Run the MySQL Client Container:

     `docker run --network tooling_app_network --name mysql-client -it --rm mysql mysql -h mysqlserverhost -u  -p`
     
     
## Prepare database schema


   - Clone the Tooling-app repository from [here](clone https://github.com/darey-devops/tooling.git)

     `git clone https://github.com/darey-devops/tooling.git`
     
   - On your terminal, export the location of the SQL file

     `export tooling_db_schema=tooling/html/tooling_db_schema.sql`
     
   - Verify that the path is exported:

     `echo $tooling_db_schema`


 
     
   