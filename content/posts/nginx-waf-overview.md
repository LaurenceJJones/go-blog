---
title: "Nginx Waf Overview"
date: 2021-12-08T21:50:07Z
draft: false
tags: ["nginx", "waf"]
cover:
    image: "nginx-waf-overview-header.jpg"
    alt: "firewall"
    caption: "Attribution to [Michael Dziedzic](https://unsplash.com/@lazycreekimages)"
    relative: false
    hidden: false
categories: ["Security"]
---
## What is NGINX?
NGINX is open source software for web serving, reverse proxying, caching, load balancing, media streaming, and more. It started out as a web server designed for maximum performance and stability. In addition to its HTTP server capabilities, NGINX can also function as a proxy server for email (IMAP, POP3, and SMTP) and a reverse proxy and load balancer for HTTP, TCP, and UDP servers. source
## What is a WAF?
A WAF or web application firewall helps protect web applications by filtering and monitoring HTTP traffic between a web application and the Internet. It typically protects web applications from attacks such as cross-site forgery, cross-site-scripting (XSS), file inclusion, and SQL injection, among others. A WAF is a protocol layer 7 defense (in the OSI model), and is not designed to defend against all types of attacks. This method of attack mitigation is usually part of a suite of tools which together create a holistic defense against a range of attack vectors. source
We now have a general understanding of what Nginx is and what a WAF can do here are the two main products that can be used:
## Nginx App Protect
Nginx App Protect is the main commercial WAF provided by F5 as an addon to Nginx Plus. Using this product can reduce the complexity and time to go live compared to "rolling your own".
## Nginx Mod Security 3.0
Nginx Mod Security 3.0 is an open-source dynamic module that can be used with the paid and open-source version of Nginx. However, there is a catch with the open-source version you must compile the module from source which can be a barrier for entry.
Now we know which products we can use in conjunction with Nginx. We must now define a set of rules to control which request should be dropped from the application, it is best practice to use a base rule set that has been thoroughly tested.
## OWASP CRS
OWASP Core Rule Set is an open-source rule set provided by the OWASP Foundation to define a set of general detections to protect web applications with the focus to have minimum false positives.
In the next post, we will work on setting up a lab environment to get hands-on experience and how we can use docker to streamline the process.
