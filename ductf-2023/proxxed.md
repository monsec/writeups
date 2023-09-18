---
description: 'Author: Karjun (g00bert)'
---

# Proxxed

Medium article link: [https://medium.com/@g00bert/proxxed-ductf-2023-13f7c9b99ead](https://medium.com/@g00bert/proxxed-ductf-2023-13f7c9b99ead)

Description: Cool haxxorz only

Challenge author: Jordan Bertasso

Category: Web (Beginner)

Source code: [https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/web/grades-grades-grades](https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/beginner/proxed)

## Basic Recon <a href="#0cea" id="0cea"></a>

When we initially load up the website we see

<figure><img src="https://miro.medium.com/v2/resize:fit:921/1*Hlfs7edUVJB3K9x2Afk0NA.png" alt="" height="358" width="614"><figcaption></figcaption></figure>

It seems we can only reach it if we maybe had “trusted IP”.

## Source Code Review <a href="#5277" id="5277"></a>

Looking at the source code, it seems to only accept the IP address of 31.33.33.7. It also seems that the http “X-Forwarded-For” is supported.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*nSTtpf1KJow0rBIaxpkaSw.png" alt="" height="670" width="700"><figcaption></figcaption></figure>

A quick google on the “X-Forwarded-For” header shows that its a http header used to identify originating IP addresses.

Source: [https://en.wikipedia.org/wiki/X-Forwarded-For](https://en.wikipedia.org/wiki/X-Forwarded-For)

## Exploit <a href="#c2da" id="c2da"></a>

Now looking at the http request to reach the website we see a basic GET request.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*nZ55zDnuyIbwOOs_htfWZw.png" alt="" height="231" width="700"><figcaption></figcaption></figure>

We try adding the “X-Forwarded-For” header in here along with the IP 31.33.33.37.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*bnus6OroDvgEWsEQHzzH-w.png" alt="" height="128" width="700"><figcaption></figcaption></figure>

And we get the flag!
