# docker task - Build a wordpress wobsite that interacts with bookstack

A docker example to understand docker architecture:

*Step 1* deploy a wordpress/nginx website (two containers)  - Network 1<br>
*Step 2* deploy a second container called bookshelf from https://hub.docker.com/r/linuxserver/bookstack - Network 2<br>
*Step 3* deploy a mysql database container- Network 3<br>
*Step 4* mysql server should contain two databases: for wordpress and bookshelf.<br>
*Step 5* Both the wordpress container and bookshelf *_should be able to_* comunicate with their corresponding mysql databases.<br>
*Step 6* Define the following entry points for users to use:<br>
    *My IP:4321 - for wordpress<br>
    *My IP:4464 - for bookstack<br>
