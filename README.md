# Docker-project
Creating webapp using docker on the top of wordpress and creating whole infrastructure as code using docker compose
# INTODUCTION
Docker is an open platform for developing, shipping, and running applications. Docker enables you to separate your applications from your infrastructure so you can deliver software quickly. With Docker, you can manage your infrastructure in the same ways you manage your applications. By taking advantage of Docker’s methodologies for shipping, testing, and deploying code quickly, you can significantly reduce the delay between writing code and running it in production.
# DOCKER
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and deploy it as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code.
# Docker commands
First of all we download  wordpress and mysql
 
 ''docker pull wordpress:5.1.1=php7.3-apache''
 
''docker pull mysql:5.7''

now it is good to check that both are downloaded or not 

''docker images | grep word''

''docker images | grep mysql''

now both having its own storage but by chance if wordpress or mysql terminated all the data were removed so it is good to create storage in our base os

we can create storage by using

''docker volume create mysql_storage''

''docker volume create wp_storage''

it is good to check 

''docker volume ls | grep mysql''

''docker volume ls | grep wp''

# launching mysql container
for launching mysql container we use user , password , database variables and storage path and host name


''docker run -dit     -e MYSQL_PASSWORD=rootpass      -e MYSQL_USER=arsh        -e MYSQL_PASSWORD=redhat      -e MYSQL_DATABASE=mydb             -v mysql_storage:/var/lib/mysql     --name dbos     mysql:5.7 ''


# client

for client we use mysql command to connect to mysql database server

''yum install mysql''

now we give some information  to mysql to connect to mysql database server 

''mysql    -h     IP of mysql(172.17.0.2)     -u arsh   -predhat ''

# launching wordpress container

for launching wordpress container we give some variables of mysql database server to wordpress so that it connect to mysql database

''docker   run   -dit        -e WORDPRESS_DB_HOST=dbos         -e WORDPRESS_DB_USER=arsh               -e WORDPRESS_DB_PASSWORD=redhat          -e WORDPRESS_DB_NAME=mydb       -v wp_storage:/var/www/html         --name wpos        -p 8080:80             --link dbos               wordpress:5.1.1-php7.3-apache''

after run this command we check it is running or not by

''docker logs wpos''

if its running then just go to google and give your system IP adress with port number 8080 like this
''167.53.4.1:8080''
# Docker compose 

for creating whole infrastructure as a code we use docker compose for setup docker compose we need to install by using command

''curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose''


and now run a command for setup it

''chmod +x /usr/local/bin/docker-compose''

now we create a folder then create a file of code

''mkdir /mycompose''

''çd /mycompose''

''vim docker-compose.yml''  PRESS ENTER
# DOCKER-COMPOSE CODE

version: ‘3’


services:


    dbos:


image: mysql:5.7


volumes:


- mysql_storage_new:/var/lib/mysql


restart: always


environment:


MYSQL_ROOT_PASSWORD: rootpass


MYSQL_USER: arsh


MYSQL_PASSWORD: redhat


MYSQL_DATABASE: mydb





wordpressos:


image: wordpress:5.1.1-php7.3-apache


restart: always


depends_on:


- dbos


ports:


- 8081:80


environment:


WORDPRESS_DB_HOST: dbos


WORDPRESS_DB_USER: arsh


WORDPRESS_DB_PASSWORD: redhat


WORDPRESS_DB_NAME: mydb


volumes:


- wp_storage_new:/var/www/html




volumes:


wp_storage_new: 


mysql_storage_new:

# Run docker compose

to run the whole infrestructure we need just one command

''docker-compose up''

