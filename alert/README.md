## Architecture Diagram of the alerting system:
![alt text](https://github.com/anjanpaul/wallStreetDocs/blob/main/alert/Screenshot%20from%202024-09-10%2015-28-19.png)



 #### How Prometheus Works
Prometheus is an open-source monitoring and alerting toolkit designed for reliability and scalability. Here's a high-level overview of how it works:

* Data Collection: 
Prometheus uses a pull model to scrape metrics from endpoints that expose data in a specific format. These endpoints are usually provided by applications, services, or exporters. Prometheus periodically sends HTTP requests to these endpoints to collect metrics.

* Storage: 
Prometheus stores time-series data in its time-series database. Each time-series is uniquely identified by its metric name and a set of key-value pairs called labels. Data is stored in a compressed format optimized for querying and retrieval.

* Querying: 
Prometheus provides a powerful query language called PromQL (Prometheus Query Language). With PromQL, users can perform a variety of queries to extract and manipulate time-series data.

* Alerting: 
Prometheus integrates with an alertmanager component to handle alerts. Users define alerting rules based on PromQL queries, and Prometheus evaluates these rules periodically. If an alert condition is met, Prometheus sends notifications to the Alertmanager, which then handles routing and sending alerts to various notification channels (e.g., email, Slack).

* Visualization: 
Prometheus itself does not provide built-in visualization. However, it integrates well with Grafana, a popular open-source visualization tool. Grafana allows users to create dashboards and visualizations based on Prometheus data.

2) Creating Custom Prometheus Alerts and Alerting Rules for Kubernetes Monitoring
To create custom alerts, you need to define alerting rules in Prometheus' configuration. Here’s a basic example for monitoring Kubernetes:

Define an Alerting Rule:

Create a YAML file for your alerting rules


```
groups:
- name: example_alerts
  rules:
  - alert: HighMemoryUsage
    expr: sum(container_memory_working_set_bytes{job="kubelet", cluster="", container!="", container!="POD"}) by (namespace, pod) / sum(container_spec_memory_limit_bytes{job="kubelet", container!="", container!="POD"}) by (namespace, pod) > 0.9
    for: 5m
    labels:
      severity: critical
    annotations:
      summary: "High memory usage on {{ $labels.pod }}"
      description: "Memory usage is above 90% on pod {{ $labels.pod }} in namespace {{ $labels.namespace }}."

```

In this example:

* The alert HighMemoryUsage is triggered if the memory usage of a container exceeds 90% of its memory limit for more than 5 minutes.
* The expr field contains a PromQL query to calculate the ratio of memory usage to memory limit.

Update Prometheus Configuration:

Update the Prometheus configuration to load the alerting rules. In your prometheus.yml file, include the rule file:

```
rule_files:
  - "alerts.yml"

```
