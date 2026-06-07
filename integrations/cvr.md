---
title: CVR
layout: default
parent: Integrations
has_children: false
---
# CVR - Datafordeler middleware

The CVR integration will lookup common information on suppliers, when they are created and periodically if entities are marked for update.  
  
## Configuration properties
  
|INTEGRATION_CVR_ENABLED| false                                                               | IS CVR integration is enabled                                                                                              |
|INTEGRATION_CVR_API_KEY|                                                                     | API Key for the datafordeler middleware integration                                                                        |
|INTEGRATION_CVR_ENDPOINT|                                                                     | Datafordeler middleware endpoint url                                                                                       |
|INTEGRATION_CVR_CRON| 0 11 * * * ?                                                        | Cron expression that determinates how often CVR is syncronized                                                             |
