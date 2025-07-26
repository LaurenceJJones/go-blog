---
title: "Stop Bots on Coolify: Deploy Anubis for WordPress and Beyond"
date: 2025-07-26T12:39:54+01:00
draft: false
tags: ["coolify", "anubis", "bot-protection", "wordpress-security"]
author: "Loz"
cover:
    image: "ai-crawlers.jpg"
    alt: "AI Crawlers"
    caption: "Attribution to [Andrea De Santis](https://unsplash.com/@santesson89)"
    relative: false
    hidden: true
categories: ["security"]
---

## Introduction

Bots are an increasing threat to websites, whether through fake sign ups, brute force login attempts, content scraping, or resource abuse. If you are running applications on [Coolify](https://coolify.io), an open source platform as a service alternative, you need a simple and efficient way to protect them without unnecessary complexity.

This is where **Anubis** comes in. In this guide, you will learn:

- What Anubis is and why it is useful  
- How it helps stop bots before they reach your application  
- A real world example of deploying Anubis with WordPress on Coolify  

---

## What is Anubis and How Does It Stop Bots?

[Anubis](https://anubis.techaro.lol) is an open source challenge proxy that protects your web applications from automated bots and abusive traffic. It works as a reverse proxy that requires clients to solve a computational proof of work challenge before granting access. This mechanism dramatically reduces malicious traffic without impacting legitimate users.

**Key features:**

- Acts as a reverse proxy between the Coolify proxy and your application  
- Uses proof of work challenges to block or slow down bots  
- Lightweight and easy to integrate with containerized environments such as Coolify  

---

## Why Anubis is the Best Bot Protection for Coolify

Bots can cause serious harm:

- Attempt brute force logins on your WordPress site  
- Flood your forms with spam submissions  
- Scrape your content or overload your server with junk traffic  
- Exploit your site for **AI scraping**, an aggressive trend as companies race to collect as much data as possible for training models. This consumes bandwidth, impacts SEO, and steals your valuable content  

**Anubis protects you by:**

- Challenging every connection with a computational proof of work  
- Filtering out malicious traffic before it reaches your application  
- Optionally serving a robots.txt file to discourage crawlers from sensitive pages  

✔ Stops automated abuse  
✔ Lightweight and resource friendly  
✔ Works seamlessly with Docker and Coolify  

---

## The Correct Way to Deploy Anubis on Coolify

A common mistake is deploying Anubis as a standalone application. This is not the correct approach.

Instead:

- Deploy one Anubis instance per application stack  
- For example, to protect WordPress, add Anubis into the same Docker Compose stack as your WordPress service  

This ensures that traffic flows correctly and that Anubis can proxy requests to your application.

---

## WordPress and Anubis Example Using Docker Compose

Here is an example of how you can deploy WordPress with Anubis inside a Coolify stack:

```yaml
services:
  anubis:
    image: ghcr.io/techarohq/anubis:latest
    expose:
      - "8923"
    environment:
      - SERVICE_FQDN_ANUWORDPRESS_8923
      - BIND=:8923
      - DIFFICULTY=5
      - SERVE_ROBOTS_TXT=true
      - TARGET=http://wordpress

  wordpress:
    image: 'wordpress:latest'
    volumes:
      - 'wordpress-files:/var/www/html'
    environment:
      - WORDPRESS_DB_HOST=mariadb
      - WORDPRESS_DB_USER=$SERVICE_USER_WORDPRESS
      - WORDPRESS_DB_PASSWORD=$SERVICE_PASSWORD_WORDPRESS
      - WORDPRESS_DB_NAME=wordpress
    depends_on:
      - mariadb
    healthcheck:
      test: ["CMD", "curl", "-f", "http://127.0.0.1"]
      interval: 2s
      timeout: 10s
      retries: 10

  mariadb:
    image: 'mariadb:11'
    volumes:
      - 'mariadb-data:/var/lib/mysql'
    environment:
      - MYSQL_ROOT_PASSWORD=$SERVICE_PASSWORD_ROOT
      - MYSQL_DATABASE=wordpress
      - MYSQL_USER=$SERVICE_USER_WORDPRESS
      - MYSQL_PASSWORD=$SERVICE_PASSWORD_WORDPRESS
    healthcheck:
      test: ["CMD", "healthcheck.sh", "--connect", "--innodb_initialized"]
      interval: 5s
      timeout: 20s
      retries: 10

volumes:
  wordpress-files:
  mariadb-data:
```

---

### How This Works

- Anubis acts as a reverse proxy between the Coolify proxy and WordPress  
- **TARGET=http://wordpress** tells Anubis where to forward traffic after the proof of work challenge is solved  
- **DIFFICULTY=5** controls the complexity of the challenge (increase for more security or decrease for better user experience)  
- **SERVE_ROBOTS_TXT=true** serves a robots.txt file, which is useful for apps you do not want indexed. For public WordPress sites, disable this option  

---

## Accessing WordPress Through Anubis

Before deployment:

- Configure your domain on the Anubis service, not WordPress  
- Example: https://app.debian.local:8923

![Anubis Example Coolify](/coolify-anubis-example.png)

Traffic flow:  
**Traefik → Anubis → WordPress**

When you visit your domain, you will first see the Anubis challenge, then your WordPress site.

**Important:** HTTPS is mandatory for production. In testing environments, you may ignore TLS warnings, but in production, always use a proper TLS certificate, such as those provided by Let’s Encrypt through Coolify.

---

## Important Notes

- Use HTTPS in production; Anubis requires it  
- Tune the **DIFFICULTY** variable for the right balance between security and usability  
- Read the full documentation for all available environment variables:  
  [Anubis Installation Guide](https://anubis.techaro.lol/docs/admin/installation)  

---

## Conclusion

Anubis provides an easy yet powerful way to protect your Coolify deployed applications from bots and automated attacks. By adding it to your stack, you create an effective security layer without complex tools or application rewrites.

Start with the example above and adapt it for your use case. Your WordPress site and your server resources will thank you.

---

## FAQ

**How does Anubis stop bots?**  
By requiring clients to solve a proof of work challenge, which makes automated attacks costly and inefficient.

**Is Anubis suitable for WordPress?**  
Yes, it works well as a reverse proxy in front of WordPress or any web application running on Coolify.

**Does Anubis protect against AI scraping?**  
Yes, since it forces scrapers to solve proof of work challenges, which increases their resource usage and discourages bulk data collection.
