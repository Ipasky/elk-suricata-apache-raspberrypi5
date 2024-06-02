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

## Index
- [ElasticSearch Installation](#Elasticsearch-Installation)
- [Kibana Installation](#Kibana-Installation)
- [Nginx Installation](#Nginx-Installation)
- [Elastic Panel](#Elastic-Panel)
- [Suricata Installation](#Suricata-Installation)
- [Fleet Server](#Fleet-Server)
- [Add Elastic Suricata and Apache Integrations](#Add-Elastic-Suricata-and-Apache-Integrations)
- [My Dashboard](#My-Dashboard)

# Instalation Guide
I followed one online video tutorial from [**IppSec**](https://youtu.be/Ts-ofIVRMo4?si=q1j9yGKyTZCcQIbu) that explains how to install all the ELK components. In my case, how I want to install it in a **Raspberry Pi 5** that has the latest **OS Debian Bookworm 12**, the installation will be a bit different that the video one. 
First of all, you proceed to the download page to get ElasticSearch and Kibana, you must have to choose the *deb aarch64* architecture: 
- [ElasticSearch](https://www.elastic.co/downloads/elasticsearch)
- [Kibana](https://www.elastic.co/downloads/kibana)
<br></br>

## ElasticSearch Installation
We start installing ElasticSearch, the terminal commands are those:
```shell
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-8.13.4-arm64.deb
sudo dpkg -i elasticsearch-8.13.4-arm64.deb
```

> [!WARNING]
> During the installation process of ElasticSearch you will receive from command text the password generated automatically, in my case was: **_Hpytz9NXn0w1PoJKxLlh_**

> [!NOTE]
> One clarification, all the passwords and tokens that I share publicly here in this repo aren’t sensitive information for me, this is a test environment where I reinstalled ELK just to document and explain all the processes.

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/01.jpg)

Next, you have to start the systemctl daemon process of ElasticSearch and check if it’s working properly:

```shell
sudo systemctl start elasticsearch.service
sudo systemctl enable elasticsearch.service
sudo systemctl status elasticsearch.service
```

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/02.jpg)
<br></br>

## Kibana Installation
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

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/03.jpg)

Then you need to move to /kibana/bin directory and execute the kibana-setup binary to put the token, then start the service and check if it runs.

```shell
cd /usr/share/kibana/bin
sudo ./kibana-setup
sudo systemctl start kibana.service
sudo systemctl enable kibana.service
```
| | |
|:------:|:------:|
| <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/04.jpg" width="500" > | <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/05.jpg" width="500" > |

<br></br>
## Nginx Installation
Now we’re going to run an nginx service to redirect all the petitions from your local IP, in my case *192.168.1.152*, to loopback IP *127.0.0.1:5601*. In that loopback address is running kibana, if we want to get access from another machine in the local network that nginx configuration is required.

Before installing nginx you may need to change the nginx default port listen or apache2 because by default both are listening in port 80. In my case I change the apache2 listen port to 8080:

```shell
sudo nano /etc/apache2/ports.conf
sudo apt install nginx
sudo nano /etc/nginx/sites-enabled/default
```

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/06.jpg)

```shell
sudo systemctl restart nginx.service
sudo systemctl enable nginx.service
```
<br></br>

## Elastic Panel
At this point, if all is running, we can put in your browser your local IP. Just enter your credentials, the default username is *elastic*, and the password was previously generated by installing ElasticSearch:
- http://192.168.1.152

| | |
|:------:|:------:|
| <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/07.jpg" width="500" > | <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/08.jpg" width="500" > |

<br></br>
## Suricata Installation
We proceed to install our Suricata IDS system:
```shell
sudo apt install suricata
sudo systemctl start suricata.service
sudo systemctl enable suricata.service
sudo systemctl status suricata.service
```
<br></br>

## Fleet Server
If we want to add Suricata Integration to our Elastic service first of all we need to configure a Fleet Server integration inside Elastic, you may follow the next steps inside the Elastic web panel:

**Management -> Integrations -> search: Fleet Server -> Add Fleet Server -> Existing Hosts -> select: Agent policy 1 -> Save and Continue**

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/09.jpg)

**Add Elastic Agent to your hosts -> Enroll in Fleet -> name: fleet-server url: https://fleet01:8220 -> Add host -> Generate Token -> Install Fleet Server**

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/10.jpg)

Then we're going to install Elastic Agent, just by following the steps that appear in the local web browser (remember that we're using deb aarch64, the commands that appear in the browser do not contemplate that version, you may need to download from Elastic public web page, as same as ElasticSearch or Kibana installation):

```shell
wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.13.4-arm64.deb
sudo dpkg -i elastic-agent-8.13.4-arm64.deb
sudo elastic-agent enroll \
  --fleet-server-es=https://192.168.1.152:9200 \
  --fleet-server-service-token=AAEAAWVsYXN0aWMvZmxlZXQtc2VydmVyL3Rva2VuLTE3MTcyMzQ3NjEzNzk6N0hTOGxyLTJSS3laYkNVX1hHenNPQQ \
  --fleet-server-policy=c08e4e58-b77a-4c77-a501-c59321765158 \
  --fleet-server-es-ca-trusted-fingerprint=a62a6f0698aaabc7f471a961c347a25928c075789771b0b8838df025b3d469e1 \
  --fleet-server-port=8220
sudo systemctl enable elastic-agent
sudo systemctl start elastic-agent
```
Here at this point, you must see on the local web browser that the installation process was successful. 

**Continue enrolling Elastic Agent -> Run standalone -> Agent policy name: Agent policy 2 -> Create policy -> Close**

<br></br>

## Add Elastic Suricata and Apache Integrations
Ultimately we add Suricata and Apache Integrations to our Elastic service:

**Management -> Integrations -> search: Suricata -> Add Suricata -> Existing hosts: Agent policy 1 -> Save and continue -> Save and deploy Changes**

**Management -> Integrations -> search: Apache HTTP -> Add Apache HTTP Server -> Existing Hosts: Agent policy 1 -> Save and continue -> Save and deploy Changes**

If you head towards the dashboard at this point you must see default panels from Suricata and Apache:

| **Dashboards -> [Log Suricata] Event Overview** | **Dashboards -> [Log Apache] Access and error logs** |
|:------:|:------:|
| <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/11.jpg" width="500" > | <img src="https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/12.jpg" width="500" > |

<br></br>
## My Dashboard
Now you're free to explore all the different panels, data logs, and many other functionalities that Elastic brings to you. I'm glad to share my own panel configuration that I use to track all the data from both services. I'm sure that I will change some panels as I discover new functionalities:

![](https://github.com/Ipasky/elk-suricata-apache-raspberrypi5/blob/main/img/13.jpg)
<br></br>

## Local Network Structure
For this project to work, it's necessary that all the traffic that you want to monitorize arrive at the Raspberry Pi, in my case, I needed to change the configuration of my Router. You have multiple options to build this: 

- Putting Raspberry Pi as a Proxy server, all the local traffic will go through your device, nevertheless adding another device in the middle of your communications can produce more latency even more if it's a low-budget hardware.
- Setting your router to port mirror mode, allows you to send a copy of all your communications that go through the other router ports to one specific port, the only inconvenience of this build is that not all the routers have this configuration.
<br></br>
