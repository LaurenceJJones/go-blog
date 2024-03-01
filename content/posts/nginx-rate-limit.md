---
title: "Nginx Rate Limit"
date: 2024-03-01T20:40:50Z
draft: false
tags: ["nginx"]
author: "Loz"
cover:
    image: "rate-limit.jpg"
    alt: "rate limit cars"
    caption: "Attribution to [David Becker](https://unsplash.com/@beckerworks)"
    relative: false
    hidden: false
categories: ["rate-limiting"]
---

# Nginx Rate Limit

## Introduction

Nginx is a powerful web server that can be used to serve static content, load balance, and act as a reverse proxy. It is also capable of rate limiting requests to prevent abuse and protect your server from being overwhelmed.

I seen various guides on how to set up rate limiting in Nginx, but I wanted to write my own since I had a specific use case in mind and I couldn't find an example anywhere.

I wanted to limit the number of requests to all my subdomains, but I also wanted to allow a single subdomain to bypass the rate limit.

This is useful when you host certain services that may require more requests than others, such as a web applications that hosts static files.

## Basic Rate Limiting Example

To set up rate limiting in Nginx, you will need to add a new `limit_req_zone` directive to your `http` block. This directive will define a new rate limiting zone that will be used to track the number of requests to your server.

Personally I created a new file in `/etc/nginx/conf.d/` called `base-rate-limit.conf` and added the following:

```nginx
limit_req_zone $binary_remote_addr zone=rate_limit:10m rate=10r/s;
```

This will create a new rate limiting zone called `rate_limit` that will track the number of requests to your server. The `10m` parameter specifies the size of the zone, and the `10r/s` parameter specifies the rate at which requests will be limited.

Next, you will need to add a new `limit_req` directive to under the original `limit_req_zone` block. This directive will use the zone we defined earlier to limit the number of requests to your server.

```nginx
limit_req_zone $binary_remote_addr zone=rate_limit:10m rate=10r/s;
limit_req zone=rate_limit burst=20 nodelay;
```

This will limit the number of requests to your server to 10 requests per second, with a burst of 20 requests. The `nodelay` parameter will prevent requests from being delayed if the rate limit is exceeded.

### Changing the default status code

If you are like me and the default status code of `503` is not what you want, you can change it by adding the `limit_req_status` under the `limit_req` directive.

```nginx
limit_req_zone $binary_remote_addr zone=rate_limit:10m rate=10r/s;
limit_req zone=rate_limit burst=20 nodelay;
limit_req_status 429;
```

[HTTP 429](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429) is the status code for "Too Many Requests" and is more appropriate for rate limiting.

## Bypass Rate Limiting for a Subdomain

After evaluating my subdomains, I discovered that one was subjected to rate limiting, which was not my intention.

Reading through the Nginx documentation, I found that if you pass an empty string to `limit_req_zone` it will not create a new zone. However, I still want to track requests for some domains? How can I do this?

The easiest solution I found was creating 2 map functions the first would return integer value based on the `$server_name` variable and the second would use the integer value to return either an empty string or `$binary_remote_addr`.

```nginx
map $server_name $limit {
    default 1;
    subdomain1  0;
}
map $limit $limit_key {
    0 "";
    1 $binary_remote_addr;
}
limit_req_zone $limit_key zone=rate_limit:10m rate=10r/s;
limit_req zone=rate_limit burst=20 nodelay;
limit_req_status 429;
```

But how do I know what the value of `$server_name` is? When you define a subdomain in your Nginx configuration, you can use the `server_name` directive to specify the domain that you want to serve.

## Bypass Rate Limiting for a Domain

If you want to bypass rate limiting for a whole domain, you can still use the map functions we created earlier. However, you can provide a modifier to the map function to allow you to match a domain and all its subdomains.

```nginx
map $server_name $limit {
    hostnames;
    default 1;
    subdomain1  0;
    *.example.com  0;
}
map $limit $limit_key {
    0 "";
    1 $binary_remote_addr;
}
limit_req_zone $limit_key zone=rate_limit:10m rate=10r/s;
limit_req zone=rate_limit burst=20 nodelay;
limit_req_status 429;
```

As you can see above we use the `hostnames;` modifier to allow the strings used within the map function to be treated as hostnames. This allows us to use wildcards to match a domain and all its subdomains.

However, the value returned from the map function are based on this heirarchy from the [Nginx documentation](https://nginx.org/en/docs/http/ngx_http_map_module.html):

```
1. string value without a mask
2. longest string value with a prefix mask, e.g. “*.example.com”
3. longest string value with a suffix mask, e.g. “mail.*”
4. first matching regular expression (in order of appearance in a configuration file)
5. default value
```

## Conclusion

Rate limiting is a powerful tool that can be used to protect your server from being overwhelmed by abusive requests. By using the `limit_req_zone` and `limit_req` directives, you can limit the number of requests to your server and prevent abuse.

Having the ability to bypass rate limiting for specific subdomains or domains is also useful, as it allows you to host services that may require more requests than others.

I hope this guide has been helpful, and I encourage you to experiment with rate limiting in Nginx to see how it can be used to protect your server.
