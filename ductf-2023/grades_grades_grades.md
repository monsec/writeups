---
description: 'Author: Karjun (g00bert)'
---

# Grades\_grades\_grades

Medium article link: [https://medium.com/@g00bert/grades-grades-grades-ductf2023-d36135489d43](https://medium.com/@g00bert/grades-grades-grades-ductf2023-d36135489d43)

Description: Sign up and see those grades :D! How well did you do this year’s subject?

Challenge author: donfran

Category: Web (Easy)

Source code: [https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/web/grades-grades-grades](https://github.com/DownUnderCTF/Challenges\_2023\_Public/tree/main/web/grades-grades-grades)

## Basic Recon <a href="#3240" id="3240"></a>

When we first access the challenge, we are first greeted with a home screen with a sign up function.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*Ez0MY2I45bv8X-b7fd6I2Q.png" alt="" height="362" width="700"><figcaption></figcaption></figure>

Upon a successful sign up, we have access to the grades screen.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*djIptP7OEYHj7ETvXwUeVQ.png" alt="" height="347" width="700"><figcaption></figcaption></figure>

## Source Code Review <a href="#3355" id="3355"></a>

So now that we have a general idea of how the website functions, lets have a look at the source code.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*DXH4jAYvpAQlgQ49rnMdxA.png" alt="" height="383" width="700"><figcaption></figcaption></figure>

Here we see a particularly interesting route /grades\_flag which seems to print out the flag, however it seems to only do so if the user is authenticated and is a teacher.

So next we try to find out how the server authenticates the user.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*bUxAa3aXAW_MNy2WrWVmng.png" alt="" height="278" width="700"><figcaption><p>User registration function.</p></figcaption></figure>

Here we see that when a user is registering, the server will create a JWT to create a session cookie to handle authentication.

So we look deeper into how the JWT is created.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*gkIkA1BrpI5MhCQyhy8stQ.png" alt="" height="557" width="700"><figcaption><p>JWT token creation function</p></figcaption></figure>

Based on the function, it appears to just take data and encode it into a JWT without sanitizing the data.

## Exploitation <a href="#2341" id="2341"></a>

Now that we know how the server works, lets hop onto BurpSuite to have a look at the HTTP requests.

A normal request to sign up as a student looks like this:

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*ZqSMxSt42Hev5lhG4tb4YQ.png" alt="" height="382" width="700"><figcaption></figcaption></figure>

Based on the code, we found out that the server checks whether a user is a teacher through a boolean is\_teacher, so we can try modifying the request to include that.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*UrY1q73YehzZ9JZB_a0kbQ.png" alt="" height="190" width="700"><figcaption></figcaption></figure>

And it seems to successfully register the user.

Now we try to access the /grades\_flag route (while remembering to change the auth\_token to the one we just got).

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*nBwDkm5dcS-E19SlLJhLHQ.png" alt="" height="375" width="700"><figcaption></figcaption></figure>

And there we have it!
