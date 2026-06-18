# ELK SIEM Stack Infrastructure

**A comprehensive, Docker-based deployment of the Elastic Stack for Security Information and Event Management (SIEM)**

## Overview

This repository delivers a production-ready ELK (Elasticsearch, Logstash, Kibana) infrastructure combined with Filebeat for centralized log collection and processing. The solution is designed to ingest, parse, store, and visualise security-relevant logs from host systems and containerized environments, forming the foundation of a modern SIEM platform.

All components are orchestrated through Docker Compose and secured using dedicated Elastic Stack service accounts.

## Data Flow Architecture

The architecture implements a classic four-tier log pipeline:

### 1. Data Collection Layer — Filebeat

Filebeat is deployed as a lightweight, resource-efficient log shipper:

- Read-only bind mount of the host log directory: `/var/log:/var/log:ro`
- Collection of Docker container stdout/stderr logs: `/var/lib/docker/containers:/var/lib/docker/containers:ro`
- Custom `filebeat.yml` configuration defines inputs, processors, and the Beats output to Logstash
- Logs are shipped securely to Logstash on port **5044**

![Data Collection and Forwarding Flow](https://github.com/user-attachments/assets/bd975597-7f9c-4c94-bded-8706d48c1c75)

### 2. Processing Layer — Logstash

Logstash performs real-time log transformation and enrichment:

- Accepts incoming Beats connections on TCP port 5044
- Applies the pipeline defined in the accompanying Logstash configuration files
- Parses timestamps, normalizes fields, adds metadata, and structures documents for efficient storage and search
- Authenticates to Elasticsearch using the `logstash_internal` user with restricted permissions

### 3. Storage Layer — Elasticsearch

Elasticsearch serves as the scalable, searchable data store:

- Processed events are written to daily indices following the pattern `logstash-YYYY.MM.DD`
- Provides full-text search, aggregations, and anomaly detection capabilities critical for SIEM workloads
- Security is enforced through native RBAC, API keys, and dedicated internal users

### 4. Visualization & Analysis Layer — Kibana

Kibana offers an intuitive interface for security analysts:

- Connects to the Elasticsearch cluster as the `kibana_system` user
- Supports creation of interactive dashboards, visualisations, alerts, and machine learning jobs
- The **Discover** interface enables free-text search and field-level exploration of raw log events

## Deployment Architecture

A single `docker-compose.yml` file defines the complete environment. All services communicate over a dedicated Docker bridge network named `elastic`. This design guarantees consistent configuration and simplifies orchestration.

The entire stack can be started with one command:

```bash
docker compose up -d
```

![ELK SIEM Deployment Architecture](https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.37.jpeg)

## Accessing the User Interface

After deployment, the Kibana web UI is reachable at:

**http://localhost:5601**

![Kibana Discover Tab — Log Exploration View](https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.46.jpeg)

The image above displays the **Discover** tab, the primary workspace in Kibana for querying and analysing log data stored in Elasticsearch.

## Security & Best Practices

- Principle of least privilege is applied to all internal service accounts (`logstash_internal`, `kibana_system`)
- Log collection volumes are mounted read-only
- Inter-service communication occurs over an isolated Docker network
- For production SIEM use, enable TLS on all endpoints, integrate with an identity provider, and implement Index Lifecycle Management (ILM) policies

---

## Footnote

*This documentation describes the ELK SIEM Stack Infrastructure. It is intended for development, testing, and educational purposes. Production deployments require additional hardening, monitoring, and compliance measures (e.g., GDPR, SOC 2, ISO 27001).*

**© 2025 — ELK SIEM Stack Project**  
Maintained by Shoyaib Hossain. All rights reserved.

---

*For questions, issues, or contributions, please open an issue in the project repository.*
