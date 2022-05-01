# Crumbs [![Generic badge](https://img.shields.io/badge/Difficulty-Easy-green.svg)](https://shields.io/)

## Description
Follow the crumbs.

Author: JoshDaBosh

## Files

```
.
└── index.js
```

## Solution

The challenge provides us a website with static id.

A look at the source code shows a dictionary and a loop that fills the dictionary with 1000 entries where the entry's key is the randomly generated value of the previous item. The document root gives us the first key, and the flag is in the last key.

We can get the content of a key via the route `/:slug` where `slug` is the key.

The objective is to follow the breadcrumbs to the flag and retrieve the flag from the dictionary. We don't want to follow 1002 crumbs manually, so we need a way to automate this.

First, we retrieve the root content. 

```python
import requests

url = "https://crumbs.web.actf.co/"

r = requests.get(nurl)
print(r.text)
```
```
Go to 24c73741-cdd9-4c76-bf79-fb82304a6ceb
```

Since we only need the UUID, we have to extract it. There are enough ways to get each UUID in its own way. I choose to split at spaces and get the third value.

```python
import requests
url = "https://crumbs.web.actf.co/"

r = requests.get(nurl)
next = r.text.split(" ")
print(next[3])
```
We can put this in a loop until we get the flag.

Then, line 24 sends the flag on its own. Assuming it doesn't have two spaces, we can break the loop when `split` doesn't return an array of size 3.

Even if the flag randomly has two spaces somewhere, the following request should return "Broke the trail of crumbs..." since the third part of the flag shouldn't be a key.

Shouldn't. Yes. There is still a slight chance for an infinite loop. If you find your exploit not stopping after a couple of minutes, change the flag detection.
```python
import requests

url = "https://crumbs.web.actf.co/"
next = ["","",""]

while True:
    nurl = f"{url}{next[2]}"
    print(nurl)
    r = requests.get(nurl)
    next = r.text.split(" ")

    if len(next) == 3:
        print(f"next: {next[2]}")
    else:
        print(f"flag: {next[0]}")
        break

```
This returned the flag after a couple minutes.