---
title: Mail
layout: default
parent: Integrations
has_children: false
---
# Mail
Simple SMTP client for e-mail integration.  

## Configuration properties
  
| Variable          | Default value                                                       | Description                                                                                                                |
| --- |---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|INTEGRATION_MAIL_ENABLED| false                                                               | Is email integration active                                                                                                |
|INTEGRATION_MAIL_CRON| 0 */5 * * * ?                                                       | Cron expression that determinates how often the mail job is run                                                            |
|INTEGRATION_MAIL_FROM| no-reply@os2compliance.dk                                           | Sender mail on e-mail sent from OS2compliance                                                                              |
|INTEGRATION_MAIL_FROM_NAME| OS2compliance                                                       | Sender name on e-mails sent from OS2compliance                                                                             |
|INTEGRATION_MAIL_USERNAME|                                                                     | SMTP username                                                                                                              |
|INTEGRATION_MAIL_PASSWORD|                                                                     | SMTP password                                                                                                              |
|INTEGRATION_MAIL_HOST|                                                                     | SMTP host                                                                                                                  |
