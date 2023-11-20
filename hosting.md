---
title: Hosting
layout: home
nav_order: 4
---
# Hosting
OS2compliance is a Java spring application, that can compile to a jar file and be run with a JDK.  
To make it easier, we create docker images where the JDK is build in, so all it really takes is docker, a database server and some configuration.

## Requirements
* Environment capable of running docker containers.  
* MySQL or MariaDB database server.  
* A SAML IDP (eg. AD-FS, OS2faktor etc.)  

## Docker
Docker images are released automatically. They are tagged as follows:
  
- ```latest``` most recent tag on main branch
- ```x.y.z``` release tag on main branch
- ```develop``` most recent tag on develop
  
## Configuration
It is easy to run OS2compliance just spin up the docker container, with the correct configuration.  
The configuration values are read from environment variables.  
All conguration properties are shown below.
  
## List of configuration properties
Below is a list of all the properties that can be modified through environments variables.

| Variable          | Default value                                                       | Description                                                                                                                |
| --- |---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|SSL_ENABLED| true                                                                | SSL enabled                                                                                                                |
|SSL_KEYSTORE_LOCATION| security/ssl-demo.pfx                                               | path to ssl certificate,                                                                                                    |
|SSL_KEYSTORE_PASSWORD| Test1234                                                        | SSL keystore password                                                                                                      |
|SSL_KEY_PASSWORD| Password1234                                                        | SSL key password                                                                                                           |
|MUNICIPAL_CVR| 123456                                                              | CVR of the municipal                                                                                                       |
|MUNICIPAL_NAME| Ukendt Kommune                                                      | Name of the municipal                                                                                                      |
|DB_URL| jdbc:mysql://localhost/os2compliance?useSSL=false&serverTimezone=UTC | JDBC database connection string                                                                                            |
|DB_USERNAME| root                                                                | Database user                                                                                                              |
|DB_PASSWORD| Test1234                                                            | Database password                                                                                                          |
|SAML_ENTITY_ID| https://os2compliance                                          | Entity ID used in SAML metadata                                                                                            |
|SAML_ENTITY_BASE_URL| https://os2compliance:8343                                          | Entity base url used in SAML metadata                                                                                      |
|SAML_METADATA_LOCATION|                                                                     | Url to the IDPs metadata should start with url:                                                                            |
|SAML_KEYSTORE_LOCATION| security/saml-keystore-dev.pfx                                      | SAML keystore location                                                                                                     |
|SAML_KEYSTORE_PASSWORD| Password1234                                                        | SAML keystore password                                                                                                     |
|SAML_ACCEPT_SELF_SIGNED| true                                                               | Accept self signed certificate                                                                                             |
|SAML_ROLE_CLAIM_NAME| roles                                                               | The name of the clain that contains the users roles                                                                        |
|SCHEDULING_ENABLED| true                                                                | If scheduled task should run on this instance, if running multiple instance, make sure only one is running scheduled tasks |
|INTEGRATION_OS2SYNC_MUNICIPAL_CVR| 123456                                                              | Municipal CVR for use in OS2sync integration                                                                               |
|INTEGRATION_OS2SYNC_ENABLED| false                                                               | Is OS2sync integration enabled                                                                                             |
|INTEGRATION_OS2SYNC_CRON| 0 0 10 * * ?                                                        | Cron expression that determinates of often OS2sync is syncronized                                                          |
|INTEGRATION_CVR_ENABLED| false                                                               | IS CVR integration is enabled                                                                                              |
|INTEGRATION_CVR_API_KEY|                                                                     | API Key for the datafordeler middleware integration                                                                        |
|INTEGRATION_CVR_ENDPOINT|                                                                     | Datafordeler middleware endpoint url                                                                                       |
|INTEGRATION_CVR_CRON| 0 11 * * * ?                                                        | Cron expression that determinates how often CVR is syncronized                                                             |
|INTEGRATION_KITOS_ENABLED| false                                                               | Is Kitos integration enabled                                                                                               |
|INTEGRATION_KITOS_CRON| 0 */30 * * * ?                                                      | Cron expression that determinates how often Kitos data is syncronize                                                       |
|INTEGRATION_KITOS_BASE_PATH| https://kitos.dk                                                    | Url to kitos                                                                                                               |
|INTEGRATION_KITOS_USER_EMAIL|                                                                     | Email of the kitos API user                                                                                                |
|INTEGRATION_KITOS_PASSWORD|                                                                     | Password for the kitos API user                                                                                            |
|INTEGRATION_MAIL_ENABLED| false                                                               | Is email integration active                                                                                                |
|INTEGRATION_MAIL_CRON| 0 */5 * * * ?                                                       | Cron expression that determinates how often the mail job is run                                                            |
|INTEGRATION_MAIL_FROM| no-reply@os2compliance.dk                                           | Sender mail on e-mail sent from OS2compliance                                                                              |
|INTEGRATION_MAIL_FROM_NAME| OS2compliance                                                       | Sender name on e-mails sent from OS2compliance                                                                             |
|INTEGRATION_MAIL_USERNAME|                                                                     | SMTP username                                                                                                              |
|INTEGRATION_MAIL_PASSWORD|                                                                     | SMTP password                                                                                                              |
|INTEGRATION_MAIL_HOST|                                                                     | SMTP host                                                                                                                  |

