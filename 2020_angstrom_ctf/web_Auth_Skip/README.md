# Confetti [![Generic badge](https://img.shields.io/badge/Difficulty-Easy-green.svg)](https://shields.io/)

## Description

Clam was doing his angstromCTF flag% speedrun when he ran into the infamous timesink known in the speedrunning community as "auth". Can you pull off the legendary auth skip and get the flag?


Author: aplet123

## Files

```
.
└── index.js
```

## Solution

This challenge provides a website with a login screen. The source code reveals two routes. The route "login" compares the given username to a static string and the password to a random string.

If the credentials are correct, it sets a cookie `("user","admin")`.

We can set cookies ourselves using the developer tools or an addon. Refresh the page, and the flag appears.