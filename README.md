# ELK system to colect logs from Suricata IDS and Apache2 HTTP Server on Raspberry Pi 5

## Introduction
This repository is a tutorial for everyone who wants to install an ELK system on a Raspberry Pi 5, to collect logs from your local network devices through Suricata IDS and data logs from your Apache2 web service. 

I want to achieve this project because I had the intention to allocate in my home network a web page opened to the internet. Giving public access to your local network is kind of dangerous and exposes your devices, for that reason I built this monitoring setup, trying to minimize my response time in case of a security breach, and although to practice and learn about new technologies.

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/softwareBanner.png)

## ELK 
The ELK system is a popular open-source collection of tools used for data analysis and visualization. It stands for Elasticsearch, Logstash, and Kibana:
-	Elasticsearch is a powerful search and analytics engine that stores your data.
-	Logstash collects data from various sources and prepares it for Elasticsearch.
-	Kibana provides a user-friendly interface to visualize and analyze the data stored in Elasticsearch.

## Suricata IDS
Suricata is a free, open-source network security tool that acts as both an Intrusion Detection System (IDS) and an Intrusion Prevention System (IPS). It continuously monitors your network traffic for malicious activity by comparing it against a set of rules and signatures.
-	Intrusion Detection: When Suricata detects suspicious activity, it generates alerts to notify you of potential threats.
-	Intrusion Prevention: Suricata can also be configured to actively block malicious traffic, preventing attacks from reaching your systems.
