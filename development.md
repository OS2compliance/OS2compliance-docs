---
title: Development
layout: default
nav_order: 2
---
# Developer guide

This document is intended for developers, who are going to work on OS2compliance.  

## Prerequisites
* Java JDK 17+
* Docker & docker-compose
* A SAML IDP (eg. AD-FS, OS2faktor etc.)

As a developer, you will most likely be using an IDE like Eclipse or IntelliJ.  
The OS2compliance uses **[MapStruct](https://mapstruct.org/)** and **[Project Lombok](https://projectlombok.org/)** for generating a range of boilerplate code, 
both Lombok and MapStruct has made plugins available for most common Java IDEs, so the generated code is made available for auto-completion in the IDEs.

## Optional: Setup hosts alias
It is recommended to add an alias in /hosts/env (linux & mac) or C:\Windows\System32\drivers\etc\hosts (windows)   
Eg.  
```127.0.0.1 os2compliance```  
this will ensure you can access you local instance at https://os2compliance:8343 and cookies will be on that name.

## Recommended: Configuring an SAML IDP
If you need to access the UI a SAML IDP is needed to login.  
In the doc/ folder there is two guides to configuring either AD-FS or OS2faktor, take a look at those.  
Note that since you are running locally the IDP will not be able to resolve the metadata at the address in those guides, instead save the metadata.xml to a local file and upload it when creating the relaying party / service provider.  
To download the metadata, start everything in docker-compose like described below and download the metadata at ```https://os2compliance:8343/saml/metadata```  

## Start everything using docker-compose
In the project root a docker-compose.yml file is included, running this will start both MySQL and OS2compliance.  
First create a .env file with the following settings
```
SAML_METADATA_LOCATION=url:URL-TO-METADATA-FROM-SAML-IDP 
SAML_ENTITY_ID=EntitityId
```

To start run the following commands:  
```
docker-compose build    
docker-compose up
```

## Seeding users
Now you have both SAML IDP and OS2compliance running, but unless you have configured OS2sync, you have no users in OS2compliance.    
Open the MySQL database with your favorite tool, and add a row in the users table:
```
INSERT INTO os2compliance.users (uuid, active, name, email, user_id) 
VALUES ('matching-uuid', true, 'An user', 'some@email.com', 'user');
```
The UUID id is the one sent from the IDP, and needs to match the OS2compliance user's uuid.  

## Debugging
Start the database from the docker-compose like so:    
```    
docker-compose up -d db
```
Now you can run the OS2compliance project either from your editor or using maven.    
Using maven is simple just run the following command
```
./mvnw package
./mvnw spring-boot:run -Dspring-boot.run.profiles=dev
```
To enable remote debug use the following command instead:    
```
./mvnw spring-boot:run  -Dspring-boot.run.profiles=dev "-Dspring-boot.run.jvmArguments=-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=y,address=5005"
```
This will start the OS2compliance in suspended mode and when an remote debugger is connected on port 5005 it will continue.

## Happy Coding
