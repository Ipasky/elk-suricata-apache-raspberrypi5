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

# Instalation Guide
I followed one online video tutorial from [**IppSec**](https://youtu.be/Ts-ofIVRMo4?si=q1j9yGKyTZCcQIbu) that explains how to install all the ELK components. In my case, how I want to install it in a **Raspberry Pi 5** that has the latest **OS Debian Bookworm 12**, the installation will be a bit different that the video one. 
First of all, you proceed to the download page to get ElasticSearch and Kibana, you must have to choose the *deb aarch64* architecture: 
- [ElasticSearch](https://www.elastic.co/downloads/elasticsearch)
- [Kibana](https://www.elastic.co/downloads/kibana)

We start installing elasticsearch, the terminal commands are those:
```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.13.4-arm64.deb
sudo dpkg -i elasticsearch-8.13.4-arm64.deb
```

> [!WARNING]
> During the installation process of ElasticSearch you will receive from command text the password generated automatically, in my case was: **_Hpytz9NXn0w1PoJKxLlh_**

> [!NOTE]
> One clarification, all the passwords and tokens that I share publicly here in this repo aren’t sensitive information for me, this is a test environment where I reinstalled ELK just to document and explain all the processes.

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/01.jpg)

Next, you have to start the systemctl daemon process of elasticsearch and check if it’s working properly:

```shell
sudo systemctl start elasticsearch.service
sudo systemctl enable elasticsearch.service
sudo systemctl status elasticsearch.service
```

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/02.jpg)

Let’s go install Kibana now, as same as previously you have to download the **_deb aarch64_** file and install it with dpkg:

```shell
wget https://artifacts.elastic.co/downloads/kibana/kibana-8.13.4-arm64.deb
sudo dpkg -i kibana-8.13.4-arm64.deb
```

When you have Kibana installed (and ElasticSearch is running) you need to create a token for the Kibana setup, you head towards */elasticsearch/bin* directory and execute the corresponding binary with the flag -s:

```shell
cd /usr/share/elasticsearch/bin
sudo ./elasticsearch-create-enrollment-token -s kibana
```


