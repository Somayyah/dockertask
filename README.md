# docker task - Build a wordpress wobsite that interacts with bookstack

A docker example to understand docker architecture:

**Step 1** deploy a wordpress/nginx website (two containers), and attach them to wordpress network<br>
**Step 2** deploy a second container called bookshelf from https://hub.docker.com/r/linuxserver/bookstack, and attach it to bookshelf network<br>
**Step 3** deploy a mysql database container, and attach it to database network<br>
**Step 4** mysql server should contain two databases: for wordpress and bookshelf.<br>
**Step 5** Both the wordpress container and bookshelf *_should be able to_* comunicate with their corresponding mysql databases.<br>
**Step 6** Define the following entry points for users to use:<br>
1. My IP:4321 - for wordpress<br>
2. My IP:4464 - for bookstack<br>

_________________________________________________________________________________________________________________________

1. Create the needed networks:<br>
  ```
  $ docker network create wordpress<br>
  $ docker network create database<br>
  $ docker network create bookshelf<br>
  ```
  ![docker network ls](https://github.com/Somayyah/dockertask/blob/master/networkls.png)<br>
  
2. Deploy two mysql databases: wordpress and bookshelf<br>

Since we are adding two databases withen the same mysql container, we need to do the following:<br>
  * In your project folder, create a file to identify the second database (Bookshelf.sql).<br>
  ```
  CREATE DATABASE IF NOT EXISTS bookshelf;
  GRANT ALL PRIVILEGES ON bookshelf.* TO 'somayyah' identified by 'rootaccess';
  ```
  * To mount bookshelf.sql into /docker-entrypoint-initdb.d create a Dockerfile with the following lines:<br>
  ```
  FROM mysql:5.7
COPY ./bookshelf.sql /docker-entrypoint-initdb.d/bookshelf.sql
  ```
  * Build an image of the database:<br>
  ```
  docker build -t custom_mysql -f Dockerfile .
  ```
To verify if build is successful:<br>
![mysqldb image](https://github.com/Somayyah/dockertask/blob/master/mysqldb.png)<br>
* Now we can run our mysql container:<br>
```
docker run --name mysqldb -e MYSQL_ROOT_PASSWORD=rootaccess -e MYSQL_USER=somayyah -e MYSQL_PASSWORD=rootaccess -e MYSQL_DATABASE=wordpress  --network database -d mysqldb
```
* To verify the mysql configuration:
```
$ docker exec -it e10b78d2449 /bin/bash   # to interact with mysql
# mysql -u root -p      # to access mysql as root
mysql> show databases;
```
If configuration is correct, both bookshelf and wordpress should be present in the table.<br>
![show databases](https://github.com/Somayyah/dockertask/blob/master/showdb.png)<br>
3. Deploy nginx/wordpress container and attach it to the database and wordpress networks<br>
```
docker run -e WORDPRESS_DB_PASSWORD=rootaccess --network database --name wordpress --link mysqldb:mysql -p 0.0.0.0:4321:80 -v "$PWD/html":/var/www/html -d wordpress
```
to verify connection:<br>
  ```
# cat /wp-config.php    ===> inside wordpress container
  ```
![connected?](https://github.com/Somayyah/dockertask/blob/master/connected.png)<br>

