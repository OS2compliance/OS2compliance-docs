---
title: OS2sync
layout: default
parent: Integrations
has_children: false
---
# OS2sync
Is used to fetch users and organisations, in time hopefully KLE as well.  
Documentation for OS2sync kan be found here [www.os2.eu/os2sync](https://www.os2.eu/os2sync)
  
## Configuration properties
  
| Variable          | Default value                                                       | Description                                                                                                                |
| --- |---------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
|INTEGRATION_OS2SYNC_MUNICIPAL_CVR| 123456                                                              | Municipal CVR for use in OS2sync integration                                                                               |
|INTEGRATION_OS2SYNC_ENABLED| false                                                               | Is OS2sync integration enabled                                                                                             |
|INTEGRATION_OS2SYNC_CRON| 0 0 10 * * ?                                                        | Cron expression that determinates of often OS2sync is syncronized                                                          |
