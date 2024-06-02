# ELK system to colect logs from Suricata IDS and Apache2 HTTP Server on Raspberry Pi 5

## Introduction
This repository is a tutorial for everyone who wants to install an ELK system on a Raspberry Pi 5, to collect logs from your local network devices through Suricata IDS and data logs from your Apache2 web service. 

The main purpose of this project was to add an extra layer of security to my local network, because I had the intention to build a public web server and allocate some other useful services.
Nonetheless, this project was for me an excellent way to learn and improve my blue team defensive skills.

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

## Instalation Guide
I followed one online video tutorial from IppSec that explains how to install all the ELK components https://youtu.be/Ts-ofIVRMo4?si=q1j9yGKyTZCcQIbu. In my case, how I want to install it in a Raspberry Pi 5 that has the latest OS Debian Bookworm 12, the installation will be a bit different that the video one. 
First of all, you proceed to the download pages to get ElasticSearch and Kibana: 
- [ElasticSearch](https://www.elastic.co/downloads/elasticsearch)
- [Kibana](https://www.elastic.co/downloads/kibana)
