# Host-Based SIEM ELK

# Introduction

"ELK" is the acronym for three open source projects: Elasticsearch, Logstash, and Kibana. Elasticsearch is a search and analytics engine. Logstash is a server‑side data processing pipeline that ingests data from multiple sources simultaneously, transforms it, and then sends it to a "stash" like Elasticsearch. Kibana lets users visualize data with charts and graphs in Elasticsearch.

*[Visit Elastic](https://www.elastic.co/elk-stack)*

# ELK Server Setup

The server will run a combination of services including Elasticsearch, Logstash, and Kibana. Logstash will be responsible for listening on the network and receiving the logs from remote hosts, it then forwards those logs to Elasticsearch to be indexed and stored.On the remote hosts, a software agent is used to forward the local logs to the ELK instance.

**Setting up ELK in windows**

#### Download and install Elasticsearch

Download the most recent build from the [Releases](https://www.elastic.co/downloads/elasticsearch) page.

The Installation Directory in Windows

Install Elasticsearch as a service

```
C:\ProgramData\Elastic
```

### Download and install Logstash

Download the most recent build from the [Releases](https://www.elastic.co/downloads/logstash) page.

Extract the Zip file into 
```
C:\ProgramData\Elastic
```

Download the configuration

```
git clone https://github.com/RichardPhilipsRoy/Host-Based-SIEM_ELK.git
```


Copy logstash.conf file into C:\ProgramData\Elastic\Logstash\

Test the configuration and start logstash

```
C:\ProgramData\Elastic\Logstash\bin\logstash -f logstash.conf --config.test_and_exit
C:\ProgramData\Elastic\Logstash\bin\logstash -f logstash.conf --config.reload.automatic
```

Or Install Logstash using NSSM (Non-Sucking Service Manager)

Download the most recent build from the [Releases](https://nssm.cc/download) page.

Extract the zip file

Powershell

```
C:\nssm-2.24\win64\start-process ".\nssm.exe" "install Logstash"
```
Set the following values on the Application tab:

```
Path: C:\ProgramData\Elastic\logstash\bin\logstash.bat
Arguments: -f C:\ProgramData\Elastic\logstash\config\logstash.conf
Service name: Logstash
Dependencies: Elasticsearch
```

Start the logstash from Services.msc


### Download and install Kibana

Download the most recent build from the [Releases](https://www.elastic.co/downloads/kibana) page.

Extract the Zip file into 

```
C:\ProgramData\Elastic
```

Edit the kibana.yml file in 


```
C:\ProgramData\Elastic\kibana\config\
```

Make the following changes

```
# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "0.0.0.0"
```

Start Kibana

```
C:\ProgramData\Elastic\Kibana\bin\kibana.bat
```
Or Install Logstash using NSSM (Non-Sucking Service Manager)

Download the most recent build from the [Releases](https://nssm.cc/download) page.

Extract the zip file

Powershell

```
C:\nssm-2.24\win64\start-process ".\nssm.exe" "install Kibana"
```
Set the following values on the Application tab:

```
Path: C:\ProgramData\Elastic\Kibana\bin\logstash.bat
Service name: Kibana
Dependencies: Elasticsearch
              Logstash
```

Start the Kibana from Services.msc


Kibana is accessible at https://localhost:5601


**Setting up ELK in Ubuntu**

#### Download and install Elasticsearch

```

$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update && sudo apt-get install elasticsearch
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable elasticsearch.service
$ sudo systemctl start elasticsearch.service
```

#### Download and install Logstash

```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update && sudo apt-get install logstash
```

Download the configuration

```
git clone https://github.com/RichardPhilipsRoy/Host-Based-SIEM_ELK.git
```

Copy the logstash.conf file into /etc/logstash/

Test and start logstash

```
$ .\bin\logstash -f logstash.conf --config.test_and_exit
$ .\bin\logstash -f logstash.conf --config.reload.automatic
```

#### Download and install Kibana

```
$ wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
$ sudo apt-get install apt-transport-https
$ echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
$ sudo apt-get update && sudo apt-get install kibana
```

Configure kibana.yml file in /etc/kibana/config/
Make the following changes

```
# Kibana is served by a back end server. This setting specifies the port to use.
server.port: 5601

# Specifies the address to which the Kibana server will bind. IP addresses and host names are both valid values.
# The default is 'localhost', which usually means remote machines will not be able to connect.
# To allow connections from remote users, set this parameter to a non-loopback address.
server.host: "0.0.0.0"
```

Start Kibana

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable kibana.service
$ sudo systemctl start kibana.service
```

Kibana is accessible at https://localhost:5601


**Setting up Windows Clients**

Download Sysmon

Download sysmon(https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)page.

* Extract the zip file 
* git clone https://github.com/RichardPhilipsRoy/Host-Based-SIEM_ELK.git
* Copy the sysmonconfig.xml file into the extracted directory

Installing Sysmon

```
C:\Sysmon\Sysmon -i sysmonconfig.xml
```

Download Winlogbeat

* Download the most recent build from the [Releases](https://www.elastic.co/downloads/beats/winlogbeat) page.
* Extracted the contents
* git clone https://github.com/RichardPhilipsRoy/Host-Based-SIEM_ELK.git
* Copy the winlogbeat.xml file into the extracted directory and replace 
* Edit the winlogbeat.xml 
```
#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
   hosts: ["ELKSERVERIP:5044"] 
```

Installing Winlogbeat

Powershell

```
PS C:\Users\Administrator> cd 'C:\Program Files\Winlogbeat'
PS C:\Program Files\Winlogbeat> .\install-service-winlogbeat.ps1
PS C:\Program Files\Winlogbeat> Start-service winlogbeat
```

**Setting up Ubuntu Clients**

Download and install auditbeat

```
$ curl -L -O https://artifacts.elastic.co/downloads/beats/auditbeat/auditbeat-6.0.1-amd64.deb
$ sudo dpkg -i auditbeat-6.0.1-amd64.deb 
```
```
$ git clone https://github.com/RichardPhilipsRoy/Host-Based-SIEM_ELK.git
```

Copy the auditbeat.xml file into /etc/auditbeat/auditbeat.yml
* Edit the winlogbeat.xml 

```
#----------------------------- Logstash output --------------------------------
output.logstash:
  # The Logstash hosts
   hosts: ["ELKSERVERIP:5044"] 
```

Start auditbeat

```
$ sudo /bin/systemctl daemon-reload
$ sudo /bin/systemctl enable auditbeat.service
$ sudo systemctl start auditbeat.service
```


!!!!HAPPY HUNTING!!!!