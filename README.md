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
  ![docker network ls](https://github.com/Somayyah/dockertask/blob/master/networkls.png)
  
2. 
