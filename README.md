Complete ELK Stack Logging Infrastructure

The Docker Compose file I shared works together with the Filebeat and Logstash configurations to create a comprehensive log collection, processing, and visualisation system. Here's how all the pieces fit together:
Data Flow Architecture
1.	Data Collection (Filebeat)
o	Deploys with access to host logs (/var/log:/var/log:ro)
o	Collects Docker container logs (/var/lib/docker/containers:/var/lib/docker/containers:ro)
o	Uses the configuration I shared first to define what logs to collect
o	Ships logs to Logstash at port 5044

<img width="452" alt="image" src="https://github.com/user-attachments/assets/bd975597-7f9c-4c94-bded-8706d48c1c75" />

3.	Processing Layer (Logstash)
o	Receives logs from Filebeat on port 5044
o	Processes logs using the pipeline configuration I shared second
o	Parses timestamps, adds metadata, and structures logs
o	Authenticates to Elasticsearch using the logstash_internal user

4.	Storage Layer (Elasticsearch)
o	Stores processed logs in daily indices (logstash-YYYY.MM.DD)
o	Provides search functionality
o	Manages security with dedicated users and roles

5.	Visualisation Layer (Kibana)
o	Connects to Elasticsearch as the kibana_system user
o	Provides dashboards and visualisations for the logs

Deployment Architecture (Ongoing)

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.37.jpeg" />

The Docker Compose file creates a complete environment where A dedicated network (elastic) connects all services. This setup allows to deploy the entire logging stack with a single command, creating a cohesive system that implements the configurations for Filebeat and Logstash.


On the localhost:5601 

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.46.jpeg" />

This screenshot is from Kibana, part of the Elastic Stack (ELK: Elasticsearch, Logstash, Kibana), specifically from the Discover tab, which is used to explore and analyse log data stored in Elasticsearch.

There is a huge Research Gap when it comes to Explainable AI (Under Consideration )

SIEM Architecture with Data Integration, AI Detection, and Explainable AI
This architecture provides a comprehensive approach to modern security monitoring by integrating traditional SIEM capabilities, advanced AI detection mechanisms, and explainable AI components (Ongoing)

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2011-05-2025%20at%2022.06.jpeg" />
