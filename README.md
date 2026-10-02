# DockerContainersLab1

## Basics
The following screenshot is after the creation of the tp1 folder and the Dockerfile which is exactly as in the subject:
<img width="945" height="459" alt="image" src="https://github.com/user-attachments/assets/1297f3b0-e25d-4985-bdfc-8625ff691048" />

After that I checked if it worked correctly with docker port in another terminal because I had forgotten to add -d:
<img width="945" height="77" alt="image" src="https://github.com/user-attachments/assets/4a61baba-5c26-4df1-a6de-7ec3e578cf62" />

Creation of the network:
<img width="945" height="44" alt="image" src="https://github.com/user-attachments/assets/f44c66d1-4165-45e7-93fc-ea9cea1b138a" />

Re-running with adminer (I changed the ports partway through while messing with the flags because it wasn’t working for a while):
<img width="945" height="119" alt="image" src="https://github.com/user-attachments/assets/c4a5a439-6b50-41a4-986d-f0201174039b" />

*Question 1-1: Because leaving the confidential data in the Dockerfile can lead to the leaking of that data if that file is public.*

<img width="945" height="600" alt="image" src="https://github.com/user-attachments/assets/538e6916-f165-4c2a-b108-1c1e352c4bce" />

<img width="659" height="541" alt="image" src="https://github.com/user-attachments/assets/997373f1-fbba-40c3-a46a-34bc40f9d4bb" />

Persist data test:
<img width="945" height="270" alt="image" src="https://github.com/user-attachments/assets/687ea811-2136-450b-b779-aaa9d0c41e1c" />
<img width="945" height="169" alt="image" src="https://github.com/user-attachments/assets/f85608ad-3e3a-4f4e-aa72-1b16c2f2bd20" />

*1-2: so that our data doesn’t get lost when we destroy our containers*
1:3
Dockerfile:
FROM postgres:17.2-alpine

ENV POSTGRES_DB=db \
POSTGRES_USER=usr \
POSTGRES_PASSWORD=pwd

COPY CreateScheme.sql /docker-entrypoint-initdb.d/01-create-scheme.sql
COPY InsertData.sql /docker-entrypoint-initdb.d/02-insert-data.sql

Commands:
<img width="945" height="37" alt="image" src="https://github.com/user-attachments/assets/893a8e69-23e6-4fc4-8aa7-9f5b156e55e9" />
<img width="945" height="166" alt="image" src="https://github.com/user-attachments/assets/2b417f5b-cb55-403f-8c19-1f613fe4eb99" />


## Backend API

FROM eclipse-temurin:21-jre
ADD Main.class .
CMD \["java", "Main"]

<img width="945" height="66" alt="image" src="https://github.com/user-attachments/assets/c5163e5d-d906-4a84-b175-71c6fcf1c6f8" />
(changed terminal this time I’m using Intellij IDEA)

*Question 1-4: Multistage builds are useful to keep Dockerfiles easy to read and maintain. The three lines under the build stage comment are used to establish the working environment and download JDK 21 and Alpine Linux, then we add Maven to the project, then we just copy the contents of pom.xml as well as the source code in src into the image and we compile it into a jar using Maven. and then we start a whole new image and redefine the working environment, then we copy the jar from before into here and rename is myapp.jar and then we just run it*


After changing the src folder with the one provided to us (in the "backend api" commit), all I had to do was change the application.yml which I did after making the backend database as follows (also I have changed IDE again now I'm back on VS Code):
<img width="1496" height="276" alt="image" src="https://github.com/user-attachments/assets/2107b5fa-628f-45f3-a89a-5147bab1ce0b" />
Here we can see that I already have a postgres image set up so I'll be using it to make the container database.

<img width="1490" height="48" alt="image" src="https://github.com/user-attachments/assets/c8d4a171-fde0-4b16-af79-5e00a7d5d948" />
I also have to rebuild the image for this part since I changed application.yml, I called the image "springg" and the container "springtest".
<img width="1057" height="106" alt="image" src="https://github.com/user-attachments/assets/fe4903f2-f765-4e18-920f-91c88c2e064f" />
<img width="1046" height="50" alt="image" src="https://github.com/user-attachments/assets/993cf396-f38f-4132-b820-f4a7de0efad5" />
<img width="1488" height="155" alt="image" src="https://github.com/user-attachments/assets/29a8d106-f272-47d8-a32f-186ae525f5bd" />


## HTTP Server
I made a very simple html page called index.html to test out the server connection and I also chose to use the httpd image as hinted in the lab.
Moreover I also used a trick to get the httpd.conf as you will see below:
<img width="1349" height="216" alt="image" src="https://github.com/user-attachments/assets/1cc02881-8687-4ea7-930e-b8cf3c7cffa8" />
note that this is in a new folder tp1-http, and this gives us three files for the new image I chose to name http1, and I also added "ServerName 127.0.0.1" to avoid the apache2 warning

<img width="1067" height="442" alt="image" src="https://github.com/user-attachments/assets/1fa1b720-ef91-43c4-8cb4-930cf174380e" />
then I made the container httpcontainer and also ran docker stats
<img width="1391" height="161" alt="image" src="https://github.com/user-attachments/assets/52f55826-807a-4aa9-a6b9-cc461803ff67" />
docker logs
<img width="1301" height="88" alt="image" src="https://github.com/user-attachments/assets/d348ec5c-7c4e-4f26-ad53-e18a93967665" />

<img width="1486" height="254" alt="image" src="https://github.com/user-attachments/assets/01cc0335-7998-4de1-b164-e5be33393239" />

## Reverse proxy
changes I made to httpd.conf (after changing the name of the spring container "springtest" to "backend"):
```
<VirtualHost *:80>
ProxyPreserveHost On
ProxyPass / http://backend:8080/
ProxyPassReverse / http://backend:8080/
</VirtualHost>
```

*Question 1-5: We need a reverse proxy to not only hide the port in the url but also make it so that when a user logs into the page, we can secretly send them to different ports without them noticing, and also it allows us to not have to specify the ports when linking the container to another one*

## Docker.compose
*Question 1-6: Docker.compose is this important because it allows us to not have to manually run and stop every container necessary to the launch of an application, and it also allows to not mess up the order to do so thanks to the depends-on line that makes it so that a container will explicitely wait for the specified other container to be up before running.*

*Question 1-7: the most important docker compose command is docker compose up --build because it's what launches all the containers for the application.*

Docker Desktop:
<img width="517" height="303" alt="image" src="https://github.com/user-attachments/assets/1f68d0a4-dae9-4b1e-ba6e-0c252623c058" />
<img width="1601" height="162" alt="image" src="https://github.com/user-attachments/assets/e150f06c-8194-4c9b-a587-3410b0b48f25" />
