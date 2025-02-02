+++
title = 'Information Gathering Web Edition'
date = 2025-02-01T20:40:16-06:00
draft = false
author = "Obsan Muzemil"
tags = ["CBBH", "cybersecurity", "Recon"]
updated = 2025-02-01
+++



# Information Gathering - Web Edition
This project was  a personal skill assessment after completing the learning path on HTB. The goal is to show information gathering techniques while providing clues for others to use the process  

## Reconnaissance
Before we start we need to put our assinged IP address in to virtual host.
```
nano /etc/hosts 
```
![hosts](/images/Cover/Recon/vHost.png)

To kick off the reconnaissance phase, I started with a WHOIS lookup on `inlanefreight.com` to gather registrar-specific details. This step is crucial for identifying administrative and technical contacts, registration dates, and registrar information, which can often provide leads for further exploration or social engineering.

**Command:**
```
whois inlanefreight.com | grep -i 'Registrar IANA ID'
```

**Output:**

![WHOIS Lookup](/images/Cover/Recon/IANA.png)

The WHOIS query revealed that the **Registrar IANA ID** is **\*\*\***, indicating the domain is registered with a specific registrar that may be worth investigating further for any potential vulnerabilities or historical data leaks.

Following this, I aimed to identify the HTTP server software that powers the `inlanefreight.htb` domain. Understanding the server type helps in tailoring specific exploits or identifying known vulnerabilities associated with that server software.

**Command:**
```
curl -i inlanefreight.htb:43906
```

**Output:**

![HTTP Server Info](/images/Cover/Recon/server.png)

The HTTP response headers indicated the site is running **\*\*\***. Knowing this, I can cross-reference nginx-specific vulnerabilities or misconfigurations, especially if the server is outdated.

Next, I used **Gobuster** to enumerate subdomains of `inlanefreight.htb`. This process helps uncover hidden or less obvious subdomains that might host sensitive or development environments.

**Command:**
```
gobuster vhost -u http://inlanefreight.htb:43906 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

**Output:**

![Gobuster Subdomain Enumeration](/images/Cover/Recon/hiddenweb.png)

The scan revealed a subdomain: `webxxxx.inlanefreight.htb`. After discovering this, I added it to my `/etc/hosts` file to ensure proper DNS resolution.

**Command to Add to /etc/hosts:**
```
XXX.XXX.XX.XX webxxxx.inlanefreight.htb
```

With `webxxxx.inlanefreight.htb` resolved, I examined its `robots.txt` file to identify any restricted directories.

**Command:**
```
curl -i webxxxx.inlanefreight.htb:33972/robots.txt
```

**Output:**

![robots.txt Content](/images/Cover/Recon/robots.png)

The `Disallow` directive revealed a hidden admin directory: `/admin_h1dd3n`. I probed this directory for more information.

**Command:**
```
curl -i webxxxx.inlanefreight.htb:33972/admin_h1dd3n/
```

**Output:**

![Hidden Admin Panel](/images/Cover/Recon/admin.png)

The admin page returned a message indicating that the panel was under maintenance, but it unexpectedly disclosed an API key:

**API Key:** `**********************`

After this discovery, I decided to run **Gobuster** again on `webxxxx.inlanefreight.htb` to identify any additional subdomains.

**Command:**
```
gobuster vhost -u http://webxxxx.inlanefreight.htb:33972 -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt --append-domain
```

**Output:**

![Gobuster VHost Enumeration](/images/Cover/Recon/dev.png)

This scan revealed another subdomain: `dev.webxxxx.inlanefreight.htb`. I added this to my `/etc/hosts` file as well.

**Command to Add to /etc/hosts:**
```
XXX.XXX.XX.XX xxx.webxxxx.inlanefreight.htb
```
![etc](/images/Cover/Recon/devVhost.png)

To automate and deepen the information gathering process, I utilized **ReconSpider** targeting `xxx.webxxxx.inlanefreight.htb:33972`. ReconSpider helped me crawl amd find  data from multiple sources, providing a good overview of the target.

**Command:**
```
python3 ReconSpider.py http://xxx.webxxxx.inlanefreight.htb:33972
```
By pointing the discovered subdomains to the correct IP addresses in my `/etc/hosts` file, I ensured seamless navigation and interaction with the target systems.

![host](/images/Cover/Recon/devVhost.png)

**Output:**

![ReconSpider Output](/images/Cover/Recon/reconspider.png)

The scan revealed an internal contact email:

**Email:** `***************`

it also showed me the api key

![Source Code Comment with API Key](/images/Cover/Recon/API2.png)

`<!-- Remember to change the API key to ********************** -->`

This shows that the API key may soon be rotated, telling us the current key might still be in use but will likely be replaced in the near future. This information could be crucial for timing any attacks that rely on API key authentication.



## Conclusion

By using a combination of **WHOIS lookups**, **HTTP header analysis**, **Gobuster enumeration**, **ReconSpider scanning**, **source code inspection**, and **virtual host enumeration**, I gathered alot of critical information about the target system. The data collected includes registrar details, server configurations, API credentials, internal contact information, and an extensive list of active subdomains.


