---
title: "Google Dorks"
date: 2021-12-18T09:39:47Z
draft: true
tags: ["google", "osint", "cheatsheet"]
author: "Loz"
cover:
    image: "google-dork.jpg"
    alt: "Google Dorks"
    caption: "Attribution to [Mitchell Luo](https://unsplash.com/@mitchel3uo)"
    relative: false
    hidden: false
categories: ["security"]
---

---
## What is a dork?
A dork is a filter that can be applied to searches to narrow down the results to what you are looking for. This is often used to find potential documents / hidden pages that were accidentally exposed to the internet. This can be used in the recon stage of a target / organisation since it does not interact with them directly.

There are many dork operators here is a list and what effect they have on the results.

| Dork          | Description                                        | Example                              |
| :-------------- |:---------------------------------------------------| :------------------------------------|
| allintext      | Searches for all keywords given. | `allintext:"keyword"` |
| intext      | Searches for the keywords all at once or one at a time. | `intext:"keyword"` |
| inurl      | Searches for a URL matching one of the keywords. | `inurl:"keyword"` |
| allinurl      | Searches for a URL matching all keywords. | `allinurl:"keyword"` |
| intitle      | Searches for keywords in title all or one. | `intitle:"keyword"` |
| allintitle      | Searches for all keywords in title. | `allintitle:"keyword"` |
| site      | Filter results down to specfic site | `Site:laurencejones.dev Or Site:blog.laurencejones.dev` |
| filetype      | Searches for a particular filetype. | `filetype:"pdf"` |
| link      | Searches for external links to pages. | `link:"keyword"` |
| numrange      | Used to locate specific numbers. | `numrange:321-325` |
| before/after      | Used to search within a particular date range. | `filetype:pdf & (before:2000-01-01 after:2001-01-01)` |
| allinanchor (and also inanchor)      | This shows sites which have the keyterms in links pointing to them, in order of the most links. | `inanchor:rat` |
| allinpostauthor (and also inpostauthor)      | Exclusive to blog search, this one picks out blog posts that are written by specific individuals. | `allinpostauthor:"keyword"` |
| related      | List web pages that are “similar” to a specified page. | `related:blog.laurencejones.dev` |
| cache      | Shows the version of the web page that Google has in its cache. | `cache:blog.laurencejones.dev` |

## Typical searches

## GHDB
The [GHDB](https://www.exploit-db.com/google-hacking-database) is a community driven website of search queries they can give you ideas to craft your own query or give you the exact query.