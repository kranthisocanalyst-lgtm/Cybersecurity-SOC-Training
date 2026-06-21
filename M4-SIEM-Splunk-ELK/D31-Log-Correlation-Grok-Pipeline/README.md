# D31 — Log Correlation: Custom Grok Pipeline (ELK Stack)

## Objective
Build and verify a custom Elasticsearch ingest pipeline to parse raw syslog
data into structured, queryable fields using Grok.

## Environment
- Ubuntu WSL2
- Elasticsearch 8.x
- Kibana
- Filebeat

## What was built
Created an ingest pipeline (`filebeat-syslog-parser`) using a Grok processor
to split unstructured syslog messages into five discrete fields:

| Field          | Description                          |
|----------------|---------------------------------------|
| log_timestamp  | ISO8601 timestamp of the log event   |
| hostname       | Source host generating the log       |
| process        | Process/service name (e.g. sshd)     |
| pid            | Process ID                           |
| log_message    | Remaining log body/message           |

**Grok pattern:**
