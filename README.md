# Thank you
We are glad to see you are installing our collectors. This is the AntSec Ansible role for the installation of our collectors. 

If you have any questions, feel free to contact us through your known channels.

For those who come across this page and don't know what it is for. Visit https://www.antsec.io

# Support
If you are unsure of anything we are here to help. Contact us and we will aid you through the process of installing the collectors.

This installer provides a basic configuration out of the box, which might not suit your needs. Feel free to contact us for further configuration.

Currently this works on Debian based linux systems. Tested platforms are:

* Debian 9
* Ubuntu 18.04.3 LTS
* Ubuntu 20.04.4 LTS

# Components
The AntSec collector for Linux consists of 4 components.

* Event Collector (Audit Beat)
  * Collection of logs from the audit log.
* System collector (Metricbeat)
  * Collection of logs about the performance of the system.
* File Collector (Filebeat)
  * Collection of logs from files or receive logs from syslog which we forward to our central system.
* Network collector (Packetbeat)
  * Collection of logs of the network traffic of the system.

# Requirements
Please make sure the following requirements are available on the system:

* Connection to athena.antsec.nl trough TCP port 7201.
* Certificate for connecting to our systems. The certificate is unique per customer.

# Installation
Place the received certificates in a folder (in this example "beats-ssl"). Do not change the names of these files
Configure the default settings in your playbook:

```
- role: beats-linux
  beats_customer_id: <customer_id received from AntSec>
  enabled_collectors: 
      - "filebeat"
      - "packetbeat"
      - "metricbeat"
      - "auditbeat"
    mb_name: {{ beats_customer_id }}
    mb_version_lock: true 
    mb_upgrade: false
    mb_major_version: "7.x"
    mb_version: "7.17.1"
    mb_modules:
      - "system"
    fb_name: {{ beats_customer_id }}
    fb_version_lock: true
    fb_upgrade: false
    fb_major_version: "7.x"
    fb_version: "7.17.1"
    fb_system:
      enable_syslog: true
      enable_auth: true
    pb_name: {{ beats_customer_id }}
    pb_version_lock: true
    pb_upgrade: false
    pb_major_version: "7.x"
    pb_version: "7.17.1"
    ab_name: {{ beats_customer_id }}
    ab_version_lock: true
    ab_upgrade: false
    ab_major_version: "7.x"
    ab_version: "7.17.1"
    ab_modules:
      - "auditd"
      - "file_integrity"
    output:
      type: logstash
      hosts:
        - " {{ as_log_collectors }} "
    ssl_config:
      directory: "/etc/ssl/as-collectors"
      verification_mode: full
      supported_protocols: TLSv1.2
      cert: "beats-ssl/certificate.crt"
      key: "beats-ssl/certificate.key"
      cacerts: "beats-ssl/antsec.cacerts"
    tags: 
      - deploy_collectors
```

# Responsible Disclosure
If you come across any (possible) vulnerabilities or have any security considerations, please contact us at security@antsec.io
