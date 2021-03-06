
# National Park's Resorts Booking

 ##Overview

* Project Original Thoughts:


Have you gotten frustrated by booking popular hotel rooms in national parks yet? This application is going to make your booking experience much easier than ever.
After having your expected room type and specific travel date, this application will send you text messages once your expected room is available.


* Project Technical Overview:

This application is developed in Spring Framework by using Spring Boot, Spring Data, Hibernate, Spring RESTful web services, Postman, Maven, PostgresSql, Docker, Amazon SQS, and Amazon S3.


* Project Business Rules:

 1. Object: User, Order, Payment, Room, Resort, Park

 2. Relationships:
 
    1. One user could have many orders.
    
    1. One order could have several payments.
    
    1. One order only can have one room type with one specific date.
    
    1. One Resort could have many rooms.
    
    1. One park could have many resorts.
    
* Project Approach:

    1. Created User, Order, Payment, Room, Resort, Park domain

    1. Used Hibernate to do the database schema migration

    1. Used JDBC to connect project with Postgres

    1. Configured Spring Security for Authentication

    1. Created repository, service and did test

    1. Did mock test for AWS S3 Storage service

    1. Created Controllers and Restful APIs

    1. Integrated third-party application AWS SQS and did Mock test

    1. Used Postman to interact with back-end project

    1. Package my project into a Docker image

## Configure local environment

```
docker pull postgres

docker run --name ${PostgresContainerName} -e POSTGRES_USER=${username} -e POSTGRES_PASSWORD=${password} -e POSTGRES_DB=${databaseName} -p ${hostport}:${containerport} -d postgres
```
## Environment properity configuration

```
location:./src/main/resources/META-INF/env
   
Template:
database.driverName=${driverName}
database.url=${url}
database.port=${port}
database.name=${name}
database.username=${username}
database.password=${password}
   
mvn compile -Dspring.profiles.active=${env}
```

## Flyway migration

```$xslt
mvn compile flyway:migrate -P unit -Ddb_username=${username} -Ddb_url=localhost:${containerport}/${databasename} -Ddb_password=${password} 
```

## Testing
* Package and install the folder before unit test.

```mvn clean compile install -DskipTests=true```
* Tests are done using JUnit and Mockito. Tests are run using the command:
```$xslt
mvn compile test -Dspring.profiles.active=${env} -P ${env}
mvn compile test -Dspring.profiles.active=${unit} -Daws.region=${region} -Ddb_url=${localhost:5432/pigge_unit} -Ddb.username=${username} -Ddb.password=${password} 
```
```$xslt
location:./src/main/resources/META-INF/env

Template:
database.driverName=${driverName}
database.url=${url}
database.port=${port}
database.name=${name}
database.username=${username}
database.password=${password}
```

## Create war package file
```$xslt
mvn compile package -P dev -DskipTests=true
```
## Reference Demo
### User sign up
```$xslt
POST - http://localhost:8080/api/users/signup
```
 Requestbody
 ```$xslt
{
	"firstName":"testFN",
	"lastName":"testLN",
	"username":"username4",
	"email":"testEmail4",
	"password":"testPassword",
	"phone":"99999999"
}
```

 Responsebody
```$xslt
{
    "firstName": "testFN",
    "lastName": "testLN",
    "username": "username4",
    "email": "testEmail4",
    "phone": "99999999",
    "password": "$2a$10$WJi7R6dAlFfFnlxLH3OX4.84YVQy3iPps7cmQ1INPWatvT6XxSnCS",
    "id": 22,
    "enabled": true,
    "accountNonExpired": true,
    "accountNonLocked": true,
    "credentialsNonExpired": true
}
```
Postman snapshoot for user sign up

![](https://github.com/di1025/NationalResortBooking/blob/master/READMESnapshoot/sign%20up.png?raw=true)
### User login

```$xslt
POST http://localhost:8080/api/users/login
```
 Requestbody
 ```$xslt
{ 
	"username": "testusername3",
	"password": "testPassword"
}
```
Responsebody
```$xslt
{
    "token": "eyJhbGciOiJIUzUxMiJ9.eyJzdWIiOiJ0ZXN0dXNlcm5hbWUzIiwiYXVkaWVuY2UiOiJhdWRpZW5jZSIsImNyZWF0ZWQiOjE1NTMwMTEyNTE4NzcsImV4cCI6MTU1MzA5NzY1MX0.jV0e6fHC1JLxSfoNtAaVqL4ouKyNswnA3m4oGHCVj5V6vdrwmHB0glAx-E2Dxgxg0BXgdLU2eUgrrWbMZSjr1A"
}
```

Postman snapshoot for user login
![](https://github.com/di1025/NationalResortBooking/blob/master/READMESnapshoot/user%20login%20.png?raw=true)

### post image to AWS S3

```$xslt
POST http://localhost:8080/api/image
```
Postman snapshoot for uploading a image to S3

![](https://github.com/di1025/NationalResortBooking/blob/master/READMESnapshoot/AWS%20S3.png?raw=true)

### send message to AWS SQS

```$xslt
POST http://localhost:8080/api/message
```

Postman snapshoot for sending messages to SQS
![](https://github.com/di1025/NationalResortBooking/blob/master/READMESnapshoot/SQS%20consumer.png?raw=true)

