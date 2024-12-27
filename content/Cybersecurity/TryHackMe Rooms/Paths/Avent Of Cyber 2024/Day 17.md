---
title: He analyzed and analyzed till his analyzer was sore!
draft: false
tags:
---

### Investigation Time
We will use Splunk

`index=*` to show all ingested logs

`sourcetype` it's the group of datasets

- `web_logs`
- `cctv_logs`

#### Examining  CCTV Logs
`index=* sourcetype=cctv_logs`

There are some problems:
- Logs are not parsed properly by Splunk
- Splunk does not consider the actual timeline of the event; instead, it uses only the ingestion time
#### Fixing the problem
We use Extract New Fields and select 

| **Timestamp**       | **Event** | **User_id** | **UserName** | **Session_id**             |
| ------------------- | --------- | ----------- | ------------ | -------------------------- |
| 2024-12-16 17:20:01 | Logout    | 5           | byte         | kla95sklml7nd14dbosc8q6vop |

### Investigating the CCTV Footage Logs
#### Event Count by Each User
`index=cctv_feed | stats count(Event) by **UserName**`

#### Summary of the Event Count
`index=cctv_feed | stats count by Event`

#### Examining Rare Events
`index=cctv_feed | rare Event`