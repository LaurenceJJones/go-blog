---
title: "Certificate Transparency Bots"
date: 2022-06-29T21:27:36+01:00
draft: false
tags: ["bots", "ips", "ids"]
author: "Loz"
cover:
    image: "bad-robots.jpg"
    alt: "bad bots"
    caption: "Attribution to [Eric Krull](https://unsplash.com/@ekrull)"
    relative: false
    hidden: false
categories: ["crowdsec"]
---

---
# What happened?
## Backstory

I was setting up a new subdomain on my VPS, I thought I had everything correctly configured but the new subdomain was being routed to another application. After an hour of troubleshooting I made the decisions to reconfigure the whole server using [nginxconfig.io](https://nginxconfig.io/) as a baseline. Within 15 minutes of entering all the information, downloading and extracting to my server I had everything ready to go.

## The attack

I requested certificates from [Lets Encrypt](https://letsencrypt.org/) and had malicious requests sent to my services that were mitigated by Crowdsec.

## Breakdown of what happened 🔥
#### Certificate Transparency 📝
Publicly trusted CA (Certificate Authorities) have adopted a internet security standard that provides a system of public logs that inform everybody about what certifcate have been issued and what are revoked. I highly encourage you to go to the [Wiki](https://en.wikipedia.org/wiki/Certificate_Transparency) and read the history section to understand why this was so important to implement.

You can go to [crt.sh](https://crt.sh/) type in your domain to get a list of certificates that have been issued. Depending on how you generate certificates for your subdomains you may be leaking information to an attacker. For example if you issue per subdomain you will have a entry per domain instead of *.domain.tld.

#### The bad bots 🤖
Bots must be monitoring newly issued certificates for the ability to automate attacks to new domains. Like I said earlier I set up a complete new subdomain which was not indexed by any search engines, so how could bots find this within seconds of issuance? These bots had a set pattern of attack trying these resources:
```text
/
/config.json
/.DS_Store
/ecp/Current/exporttool/microsoft.exchange.ediscovery.exporttool.application
/.env
/.git/config
/info.php
/login.action
/s/3138382e3131342e39362e33/_/;/META-INF/maven/com.atlassian.jira/jira-webapp-dist/pom.properties
/s/3138382e3131342e39372e33/_/;/META-INF/maven/com.atlassian.jira/jira-webapp-dist/pom.properties
/server-status
/telescope/requests
```
Interesting these paths are a mixture of CVE's and general information exposure through misconfiguration. Most likely the CVE's they are testing for [Outlook CVE-2021-28481](https://msrc.microsoft.com/update-guide/vulnerability/CVE-2021-28481) and [Atlassian CVE-2021-26086](https://cve.circl.lu/cve/CVE-2021-26086).

#### The "good" bots 😇
These bots include Censys and Google, Yes the term good bots is used lightly but at least they did not try to request CVE resources. However, Censys bot IP was already marked on the community blocklist due to the fact Censys tries to "detect" what services an IP is running. As you can see from the below picture the scanning behind this IP is very aggresive:
![Censys bot](/censys-bad-ip.png "'Good' Bot")
**Chart and information provided via [Crowdsec Cloud Console](https://app.crowdsec.net) please consider about joining the crowd!**

## What is Crowdsec 🦙
CrowdSec is an open-source and collaborative IPS (Intrusion Prevention System) cybersecurity stack.
It leverages local behavior analysis to create a global IP reputation network. [taken from website](https://www.crowdsec.net/)

I have been using Crowdsec for about 7 months on my primary VPS server. Ever since installing the IDS and IPS components I have felt more consious about what attacks are happening and more importantly how it is preventing these attacks.

I use multiple bouncers (IPS component), however, [cloudflare bouncer](https://github.com/crowdsecurity/cs-cloudflare-bouncer) took the brunt of the attacks that happened following issuing the certficates. 

Since these IP's were within the community blocklist, they were already banned before even getting to my server. All this for free! Please checkout out [Crowdsec](https://www.crowdsec.net/) and if you wish to join the community I suggest joining the [Discord Server](https://discord.gg/crowdsec).