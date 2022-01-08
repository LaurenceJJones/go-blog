---
title: "Google Dorks"
date: 2022-01-08T00:00:00Z
draft: false
tags: ["google", "cheatsheet", "recon"]
author: "Loz"
cover:
    image: "google-dork.jpg"
    alt: "Google Dorks"
    caption: "Attribution to [Mitchell Luo](https://unsplash.com/@mitchel3uo)"
    relative: false
    hidden: false
categories: ["osint"]
---

---
## What is a dork?
A dork is a filter that can be applied to searches to narrow down the results to what you are looking for. This is often used to find potential documents / hidden pages that were accidentally exposed to the internet. This can be used in recon stage since it does not interact with a target / organisation directly.

There are many dork operators here is a list and what effect they have on the results.

| Dork          | Description                                        | Example                              |
| :-------------- |:---------------------------------------------------| :------------------------------------|
| site      | Filter results down to specfic site | `Site:laurencejones.dev Or Site:blog.laurencejones.dev` |
| filetype      | Searches for a particular filetype. | `filetype:"pdf"` |
| cache      | Shows the version of the web page that Google has in its cache. | `cache:blog.laurencejones.dev` |
| intext      | Searches for the keywords all at once or one at a time. | `intext:"keyword"` |
| inurl      | Searches for a URL matching one of the keywords. | `inurl:"keyword"` |
| intitle      | Searches for keywords in title all or one. | `intitle:"keyword"` |
| allintext      | Searches for all keywords given. | `allintext:"keyword"` |
| allinurl      | Searches for a URL matching all keywords. | `allinurl:"keyword"` |
| allintitle      | Searches for all keywords in title. | `allintitle:"keyword"` |
| link      | Searches for external links to pages. | `link:"keyword"` |
| numrange      | Used to locate specific numbers. | `numrange:300-325` |
| before/after      | Used to search within a particular date range. | `filetype:pdf & (before:2020-01-01 after:2021-01-01)` |
| allinanchor (and also inanchor)      | This shows sites which have the keyterms in links pointing to them, in order of the most links. | `inanchor:dog` |
| allinpostauthor (and also inpostauthor)      | Exclusive to blog search, this one picks out blog posts that are written by specific individuals. | `allinpostauthor:"keyword"` |
| related      | List web pages that are “similar” to a specified page. | `related:blog.laurencejones.dev` |

Additional search operators:

OR operator
```js
intitle:admin | intitle:finance
```

AND operator
```js
intitle:admin & inurl:ftp
```

## Example searches
### Website
Interesting website files:
```js
site:<target> (ext:pdf | ext:txt | ext:log | ext:doc | ext:docx | ext:pptx | ext:xlsx |  ext:xlsm | ext:xlsb | ext:xltx | ext:xltm | ext:xlt |ext:xls | ext:xml | ext:xlam | ext:xla | ext:xlw | ext:xlr | ext:docm | ext:dot | ext:dotm | ext:dotx | ext:htm | ext:mht | ext:mhtml | ext:odt | ext:rtf | ext:wps | ext:xps | ext:ini)
```
Index of:
```js
site:<target> intitle:"index of /*" //Optional > (inurl:ftp | inurl:login | inurl:smb | inurl: admin)
```
### Username
```js
intitle:<username> | inurl:<username> | intext:<username>
```

---
## Extras
- List of dorking tools: [Github](https://github.com/enaqx/awesome-pentest#dorking-tools)
- Google Hacking Database: [GHDB](https://www.exploit-db.com/google-hacking-database "GHDB")
- Cheatsheet: [Gist](https://gist.github.com/sundowndev/283efaddbcf896ab405488330d1bbc06)