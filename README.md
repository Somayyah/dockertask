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

  $ docker network create wordpress<br>
  $ docker network create database<br>
  $ docker network create bookshelf<br>
  ![docker network ls](https://github.com/Somayyah/dockertask/blob/master/networkls.png)<br>
  
2. Deploy two mysql databases: wordpress and bookshelf<br>
Since we are adding two databases withen the same mysql container, we need to do the following:<br>
  * In your project folder, create a file to identify the second database (Bookshelf.sql).<br>
  ```
  CREATE DATABASE IF NOT EXISTS bookshelf;
  GRANT ALL PRIVILEGES ON bookshelf.* TO 'somayyah' identified by 'rootaccess';
  ```
  * To mount bookshelf.sql into /docker-entrypoint-initdb.d create a Dockerfile with the following lines:
  ```
  FROM mysql:5.7
COPY ./bookshelf.sql /docker-entrypoint-initdb.d/bookshelf.sql
  ```
  * Build an image of the database:
  ```
  docker build -t custom_mysql -f Dockerfile .</code>
  ```
