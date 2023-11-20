---
title: Kitos
layout: default
parent: Integrations
has_children: false
---
# Kitos
Kitos is used to fetch it-systems and suppliers and create create them automatically in OS2compliance.  
At time of writing further documentation can be found here 
[Teknisk dokumentation](https://os2web.atlassian.net/wiki/spaces/KITOS/pages/657391621/Teknisk+dokumentation)  

## Configuration properties
  
| Variable          | Default value                                                       | Description                                                                                                                |
| --- |---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|INTEGRATION_KITOS_ENABLED| false                                                               | Is Kitos integration enabled                                                                                               |
|INTEGRATION_KITOS_CRON| 0 */30 * * * ?                                                      | Cron expression that determinates how often Kitos data is syncronize                                                       |
|INTEGRATION_KITOS_BASE_PATH| https://kitos.dk                                                    | Url to kitos                                                                                                               |
|INTEGRATION_KITOS_USER_EMAIL|                                                                     | Email of the kitos API user                                                                                                |
|INTEGRATION_KITOS_PASSWORD|                                                                     | Password for the kitos API user                                                                                            |

