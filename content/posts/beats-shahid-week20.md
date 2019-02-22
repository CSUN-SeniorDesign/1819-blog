---
title: "Shahid's Blog. Week 20."
date: 2018-2-22T15:57:16-07:00
draft: false
tags: ["Shahid Karim", "EAT", "Week 20"]
layout: 'posts'
---

## Shahid's Blog. Week 20
#### What's new this week?
This week I had to secure our AWS EC2 Linux Ubuntu 18.04 instance.

#### Installing and Configuring fail2ban

```
  sudo apt-get update && \
  sudo apt-get install fail2ban && \
  cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local && \
  atom /etc/fail2ban/jail.local
```

```
[sshd]
enabled  = true
port     = ssh ( provide the port number if the default port is changed )
protocol = tcp
filter   = sshd
logpath  = /var/log/secure
maxretry = 3 ( max no. of tries after which the host should be banned)
findtime = 600 (This parameter sets the window that fail2ban will pay attention to when looking for repeated failed authentication attempts in seconds)
bantime  = 600 (time duration for which the host is banned -in seconds)
```

#### Useful programs

- Installed automatic updates for critical software

- Updated security group to only accept SSH on CSUN IP and our team's home IPs

- Installed logwatch to make sure we get updates on any suspicious activity

- Secured our apache2 server by turning off the server signature (i.e. Apache2/2.2.22 (Debian) Server at 192.168.1.7 Port 80)

  - This was done by going to
  sudo atom /etc/apache2/conf-enabled/security.conf
  then inputting the following lines:
  ```
    ServerTokens Prod
    ServerSignature Off
    Header always unset X-Powered-By
  ```
