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

