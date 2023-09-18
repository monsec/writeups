---
description: 'Author: Karjun (g00bert)'
---

# Actually Proxxed

Medium article link: [https://medium.com/@g00bert/actually-proxxed-ductf-2023-d8fd71487f9f](https://medium.com/@g00bert/actually-proxxed-ductf-2023-d8fd71487f9f)

Challenge Description: Still cool haxxorz only!!! Except this time I added in a reverse proxy for extra security. Nginx and the standard library proxy are waaaayyy too slow (amateurs). So I wrote my own :D

Challenge Author: Jordan Bertasso

Category: Web (Easy)

Source code: [https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/web/actually-proxed](https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/web/actually-proxed)

## Basic Recon <a href="#6f19" id="6f19"></a>

Looking at the initial http request to connect to the challenge we see this:

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*zy9nrZs9U0Ld9joS7EIlkw.png" alt="" height="249" width="700"><figcaption></figcaption></figure>

This feels awfully similar to the Proxxed challenge where we needed to put in an “X-Forwarded-For” header. Also it seems to claim our IP address is localhost.

The challenge description hints that it uses a reverse proxy.

## Source Code Review <a href="#d9eb" id="d9eb"></a>

Having a look through the source code we see a few points of interest:

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*UCJvZhmS96TbcQGIZBUw8Q.png" alt="" height="668" width="700"><figcaption></figcaption></figure>

It appears that it does indeed support the “X-Forwarded-For” headers and we need to change the IP to “31.33.33.37” again.

## Exploit <a href="#ef21" id="ef21"></a>

Lets try the same thing as we did in Proxxed and add in the header just to see what happens.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*ZS5z9WwGU3dXxFB3AZ07TQ.png" alt="" height="241" width="700"><figcaption></figcaption></figure>

As expected it does not work, however the IP address seems to have changed. But it does not match the one we provided in the header.

There must be some additional trick we need to overcome.

Lets consult the wikipedia page about this header.

> The X-Forwarded-For header is added or edited by HTTP [proxies](https://en.wikipedia.org/wiki/Proxy\_server) when forwarding a request. The server appends the address of the client to an existing X-Forwarded-For header separated by a comma, or creates a new X-Forwarded-For header with the client address as the value.
>
> Since it is easy to forge an X-Forwarded-For field the given information should be used with care. The right-most IP address is always the IP address that connects to the last proxy, which means it is the most reliable source of information. X-Forwarded-For data can be used in a forward or reverse proxy scenario. If the server is behind a trusted [reverse proxy](https://en.wikipedia.org/wiki/Reverse\_proxy) and only allows connections from that proxy, the header value can usually be assumed to be trustworthy.
>
> Just logging the X-Forwarded-For field is not always enough as the last proxy IP address in a chain is not contained within the X-Forwarded-For field, it is in the actual IP header. A web server should log both the request’s source IP address and the X-Forwarded-For field information for completeness.

It seems the header can support multiple values separated by commas and multiple headers of the same type as well.

First we try have multiple values separated by commas.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*PguAFEW-UokO8WnX1HJhLQ.png" alt="" height="270" width="700"><figcaption></figcaption></figure>

No luck :(

Next we try multiple headers:

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*S4oLZ41CKtCc3x7CxLYDBw.png" alt="" height="299" width="700"><figcaption></figcaption></figure>

And that works!
