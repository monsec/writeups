---
description: 'Author: Karjun (g00bert)'
---

# Needle in IAM

Medium article link: [https://medium.com/@g00bert/needle-in-iam-ductf-2023-581c297265b5](https://medium.com/@g00bert/needle-in-iam-ductf-2023-581c297265b5)

Challenge Description: I’ve been told the flag I need is in description of this role, but I keep getting an error with the following command. Surely there’s another way?

`gcloud iam roles describe ComputeOperator --project=<PROJECT>`

Challenge Author: BootlegSorcery@

Category: Misc (Beginner)

We are also given this credentials.json file:

We are also given a credentials.json:

> { “type”: “service\_account”, “project\_id”: “needle-in-iam”, “private\_key\_id”: “ff41702e0b51f7a69e8ee1a255b7be6d312a3840”, “private\_key”: “ — — -BEGIN PRIVATE KEY — — -\nMIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCiIr74uUaRmz4J\n3WKSM+G2e4Q8M8easAJnnMnR9aZrJJwVEoqs6TDpHPI1rNVKsbOGlf6ofg1+CA75\nE7V4zDrqd5FyIlE8obkFdJLn0d5C48q2pV51k5V8snGYNmGcyX6swysw9+LOq1kg\nz8fj/LNPDlTUM0V0oYubRX9CKZTFwbNX207hHfyNBMIaErkaSoVt0UNJF8WqeKo8\npdplAsr5zyXf9KEyx5TaeHx0HnTgOFwCq3019pCVpQ9cOpyfPptkuxge5CXK7X0U\nvOlQf7HpW7E7XJH8h+pe9Bt7jeoJThcrlveGYf9/PKWNzTHZy5U++iLTd4f3x4zi\n8YLNf8+fAgMBAAECggEAOkFow55yfCO+7TV10tlAWtxTfXwPVoWyP39GxqFQW8Pq\nLuocGJeq4r9rSZzhgDaMLinbt7ee6m9DzfvmYtJiwtcWU99/t9zVyV+C3zd5eCg3\nsFuHrpBKEGVfSlUTyo1dbf6sGKqgfCh13EO76y9jT97y3NHVPVxD+JTGbkPZeBoX\nUmWfmjGMLegAN3tNAOT19IRsH2dNRwbXqG3CP10bzmC+BOLIDnZNPbszh1mpkO7p\nkTQ2LOf12UlBmwdI7UAkM+iO1A/Ur41R1hRoEFvJgy1ZQzwqbBc3HidFBVBwDqkY\nFwrYNCQBNMPEuV8XTSrxFjeYE2cKUY4qapdvPHZuHQKBgQDg8EXLNtLoNd+saCBh\ndZ7tawi1ngYghzfWJmzPsMFGAh5XxZ98Rz32X2YU65jTLAXBnX/z2aTB4XOnaX6q\n6tbRevVHHaJUnsd6X2M39Xt6xbV9xhqSHTTFyfq1JsLctpyGkUfMeFxW2MfURWW4\nePtTqDPxjocoOMSIs9LPR3Y1ZQKBgQC4hln0O3Be3bODRqjfOKv51u8Y5zvBIOzn\nHr9BeWBuWsvHXG3NPjkwPe+TBPDAODTyKSOmhyhlAzT6gtmPGp1LRNSs5h1DkK2y\nAPy3ieK/mbpGscq5mchm9GmC+4bNH/nLFzmiXD8qf4k+1bij0t2Bh19e5y4/X/Hj\nbM1Df3PyswKBgG4yB18YipYr3lnt4P8dyi/xYaDnu4Sv+ZC13lSY+PY9D3RcYldV\n52sNLUtOZ938EQ3bBNYHZ4l701bOfblptreFDyg5wk7GQl8W39qILmfk95aYOGgg\nWrwSyPl59bh+1YuvHId053e8V5kMLlsDGczP+DJ8aoYv2UhHIB1fmu9pAoGBAJfl\n2RTo/SbKwCR3vToMD93Z5fcNGq5v6TSUpgJC5XPSgF97odPLvg4NXjMbZQgG/Oa/\noN5L8p+8lRcHMgrQcN1uKtitkTd2WNXoZCC+fA8XgDUD1IsWodbGqjitz5j6Eonx\nc3tJDqJwXE2CZ71MLxWal5KrIfH/jEKX5R0ERTFrAoGAZbJxIx6K6v5k8Qcda99S\n2Z61Nkgup5t51RpApiXsWqBXokOMRnmsTO7A4DAHSTJsnLTN5FtCsXKYXy6PQ737\nXJeG9tAVjtTPhffadAfKNtdYazQBzkNPJPLg7CB50f3g8ZKEfjREMxq56QmZ9CfL\nOlVNNSUpdBVFfxJiD90QtxY=\n — — -END PRIVATE KEY — — -\n”, “client\_email”: “[buildkite-agent@needle-in-iam.iam.gserviceaccount.com](mailto:buildkite-agent@needle-in-iam.iam.gserviceaccount.com)”, “client\_id”: “116616311646179064318”, “auth\_uri": “[https://accounts.google.com/o/oauth2/auth](https://accounts.google.com/o/oauth2/auth)", “token\_uri”: “[https://oauth2.googleapis.com/token](https://oauth2.googleapis.com/token)", “auth\_provider\_x509\_cert\_url”: “[https://www.googleapis.com/oauth2/v1/certs](https://www.googleapis.com/oauth2/v1/certs)", “client\_x509\_cert\_url”: “[https://www.googleapis.com/robot/v1/metadata/x509/buildkite-agent%40needle-in-iam.iam.gserviceaccount.com](https://www.googleapis.com/robot/v1/metadata/x509/buildkite-agent%40needle-in-iam.iam.gserviceaccount.com)", “universe\_domain”: “[googleapis.com](http://googleapis.com/)” }

## Basic Setup <a href="#e04e" id="e04e"></a>

From the looks of the challenge, it appears we will be using the GCP platform and using the gcloud command line.

So we first hop on the GCP and access the gcloud command line.

We can try using the provided command:

`gcloud iam roles describe ComputeOperator --project="needle-in-iam"`

Unsurprisingly, it did not work, due to lack of permissions.

However, we were provided with a credentials.json file, maybe we can use that?

## Authentication <a href="#b7c2" id="b7c2"></a>

So we need to use that credentials.json and authenticate ourselves on gcloud.

We start by creating a copy of it that is available on gcloud.\
I did this by copying the text and creating a local copy of the credentials.json on my gcloud home directory using nano.

Based on the credentials, we see that it is for a service account.

> “type”: “service\_account”,

So we use this command to activate it:

gcloud auth activate-service-account — key-file=credentials.json

## Trying Again <a href="#3f27" id="3f27"></a>

After authenticating ourselves lets try again.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*931W6XWwGDqk11ySeKaJpA.png" alt="" height="74" width="700"><figcaption></figcaption></figure>

Still we get permission denied :(, i guess thats the challenge then.\
How do we view the description of ComputeOperator when we cant use describe?

Lets see what other options we have.

When looking at the other options for looking at IAM roles:

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*SR6RWgo-_nt8YN_6lsLpig.png" alt="" height="798" width="700"><figcaption></figcaption></figure>

The list option seems interesting, lets try it.

After running list, it seems to work! However there appears to be 100s of IAM roles, making it difficult to sort through.

So we just run the command again, but grep through the results afterwards using this command:

gcloud iam roles list — project=”needle-in-iam” | grep -C 5 ComputeOperator

We add -C 5 here to show 5 lines preceding and proceeding the line where ComputeOperator occurs.

<figure><img src="https://miro.medium.com/v2/resize:fit:1050/1*STNVKmINQ_tF4iO_Y2Inzw.png" alt="" height="137" width="700"><figcaption></figcaption></figure>

And there it is!
