ELK Stack Infrastructure (Under Development )

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

Deployment Architecture 

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.37.jpeg" />

The Docker Compose file creates a complete environment where A dedicated network (elastic) connects all services. This setup allows to deploy the entire logging stack with a single command, creating a cohesive system that implements the configurations for Filebeat and Logstash.


On the localhost:5601 

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2012.46.jpeg" />

This screenshot is from Kibana, part of the Elastic Stack (ELK: Elasticsearch, Logstash, Kibana), specifically from the Discover tab, which is used to explore and analyse log data stored in Elasticsearch.

There is a huge Research Gap when it comes to Explainable AI 

SIEM Architecture with Data Integration, AI Detection, and Explainable AI
This architecture provides a comprehensive approach to modern security monitoring by integrating traditional SIEM capabilities, advanced AI detection mechanisms, and explainable AI components 

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2011-05-2025%20at%2022.06.jpeg" />

Transparency via Explainability Layer (XAI)

<img width="352" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2012-05-2025%20at%2019.55.jpeg" />

THE TRUST GAUGE IN MODERN SECURITY

In today's high-stakes cybersecurity landscape, the question isn't just whether your AI detected a threat—it's how certain the system is about what it found. Enter confidence scores: the digital equivalent of a security analyst's gut feeling, translated into hard numbers that teams can use.

In cybersecurity, overwhelming the team with too many alerts, especially incorrect ones, can lead to alert fatigue. By incorporating confidence scores, organisations can tune alert thresholds or automate actions only for events that surpass a certain level of certainty, improving efficiency and response accuracy.



Security Without Sharing: The Federated Defence Revolution

<img width="252" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2014-05-2025%20at%2008.25.jpeg" />

Federated Graph Neural Networks for Privacy-Preserving Collaborative Threat Detection Using ELK Stack

This approach:

Combines three frontiers: FL + GNN + security

Addresses real-world constraints: privacy, scalability, and label scarcity

Offers broad scope for contributions in theory, implementation, and evaluation




SIEM with AI Components (LSTM & GNN) Architecture

Security operations begin with comprehensive data gathering from logs, network traffic, endpoints, and cloud services. Collection agents capture and normalise this diverse information into standardised formats suitable for analysis, creating a consistent foundation for security intelligence.

SIEM System

The SIEM platform is the central security monitoring hub, providing real-time visibility, event correlation, and data storage. Modern SIEMs extend beyond log management to deliver compliance reporting and prepare optimised data for AI analysis, bridging traditional security monitoring with advanced analytics.

AI Components
The architecture incorporates two specialised neural network types:

LSTM Neural Networks 

Analyse temporal patterns and sequences, detecting anomalies in user behaviours and attack progressions over time

Graph Neural Networks

Model relationships between entities, mapping connections between systems and analysing potential attack paths through complex networks


Integrating these technologies produces enhanced security insights with fewer false positives, supports proactive threat hunting, enables automated responses, and powers predictive analytics. This transforms security from reactive monitoring to proactive defence, helping teams anticipate and prevent threats before exploitation occurs.

<img width="452" alt="image" src="https://github.com/Shoyaib-Hossain/elk_stack/blob/main/Image%2013-05-2025%20at%2009.59.jpeg" />
