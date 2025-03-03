Complete ELK Stack Logging Infrastructure

The Docker Compose file I shared works together with Ir Filebeat and Logstash configurations to create a comprehensive log collection, processing, and visualization system. Here's how all the pieces fit together:
Data Flow Architecture
1.	Data Collection (Filebeat)
o	Deploys with access to host logs (/var/log:/var/log:ro)
o	Collects Docker container logs (/var/lib/docker/containers:/var/lib/docker/containers:ro)
o	Uses the configuration I shared first to define what logs to collect
o	Ships logs to Logstash at port 5044

flowchart TD
    subgraph Host Machine
        dockerLogs[/Docker Container Logs\n/var/lib/docker/containers/]
        sysLogs[/System Logs\n/var/log/]
    end

    subgraph "Docker Compose Environment"
        subgraph "Collection Layer"
            filebeat["Filebeat\nCollects & Ships Logs\nPort: N/A"]
        end

        subgraph "Processing Layer"
            logstash["Logstash\nProcesses & Transforms\nPorts: 5044, 5000, 9600"]
        end

        subgraph "Storage Layer"
            elastic["Elasticsearch\nStores & Indexes\nPort: 9200"]
        end

        subgraph "Visualization Layer"
            kibana["Kibana\nVisualizes & Explores\nPort: 5601"]
        end

        subgraph "Setup Service"
            setup["Setup Container\nConfigures Security\nCreates Users & Roles"]
        end
    end

    dockerLogs -->|Read Only Mount| filebeat
    sysLogs -->|Read Only Mount| filebeat
    
    filebeat -->|"Send Logs\nPort 5044"| logstash
    logstash -->|"Store Processed Logs\nuser: logstash_internal"| elastic
    elastic -->|"Query Data\nuser: kibana_system"| kibana
    
    setup -->|"Configure Security"| elastic
    
    classDef collection fill:#B5EAD7,stroke:#333,stroke-width:2px
    classDef processing fill:#FFD7BA,stroke:#333,stroke-width:2px
    classDef storage fill:#C7CEEA,stroke:#333,stroke-width:2px
    classDef visualization fill:#FFDAC1,stroke:#333,stroke-width:2px
    classDef setup fill:#E2F0CB,stroke:#333,stroke-width:2px
    classDef hostFiles fill:#D3D3D3,stroke:#333,stroke-width:2px
    
    class filebeat collection
    class logstash processing
    class elastic storage
    class kibana visualization
    class setup setup
    class dockerLogs,sysLogs hostFiles

3.	Processing Layer (Logstash)
o	Receives logs from Filebeat on port 5044
o	Processes logs using the pipeline configuration I shared second
o	Parses timestamps, adds metadata, and structures logs
o	Authenticates to Elasticsearch using the logstash_internal user

4.	Storage Layer (Elasticsearch)
o	Stores processed logs in daily indices (logstash-YYYY.MM.DD)
o	Provides search functionality
o	Manages security with dedicated users and roles

5.	Visualization Layer (Kibana)
o	Connects to Elasticsearch as the kibana_system user
o	Provides dashboards and visualizations for the logs

Deployment Architecture

The Docker Compose file creates a complete environment where:
•	A dedicated network (elastic) connects all services
•	Persistent storage preserves Elasticsearch data (elasticsearch-data volume)
•	Security is configured during initial setup
•	All services have appropriate resource limits and configurations

This setup allows to deploy the entire logging stack with a single command, creating a cohesive system that implements the configurations for Filebeat and Logstash.
