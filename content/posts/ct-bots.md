---
title: "Certificate Transparency Bots"
date: 2022-06-29T21:27:36+01:00
draft: true
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
# Backstory

I was setting up a new subdomain on my VPS, I thought I had everything correctly configured but the new subdomain was being routed to another application. After an hour of troubleshooting I made the decisions to reconfigure the whole server using [nginxconfig.io](https://nginxconfig.io/) as a baseline. Within 15 minutes of entering all the information, downloading and extracting to my server I had everything ready to go.

## What happened next?

I requested certificates from [Lets Encrypt](https://letsencrypt.org/) and had malious requests sent to my services that were mitigated by Crowdsec.

## Breakdown of what happened
#### Certificate Transparency
Publicly trusted CA (Certificate Authorities) have adopted a internet security standard that provides a system of public logs that inform everybody about what certifcate have been issued and what are revoked. I highly encourage you to go to the [Wiki](https://en.wikipedia.org/wiki/Certificate_Transparency) and read the history section to understand why this was so important to implement.

You can go to [crt.sh](https://crt.sh/) type in your domain to get a list of certificates that have been issued. Depending on how you generate certificates for your subdomains you may be leaking information to an attacker. For example if you issue per subdomain you will have a entry per domain instead of *.laurencejones.dev.
