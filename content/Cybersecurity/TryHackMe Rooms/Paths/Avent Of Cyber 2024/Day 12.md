---
title: If I can’t steal their money, I’ll steal their joy!
draft: false
tags:
  - cybersecurity
  - webtiming
---
### Web Timing and Race Conditions

A web timing attack means we glean information from a web application by reviewing how long it takes to process our request
Race conditions are a subset of web timing. We are no longer simply looking to fain access but can cause the web application to perform unintended actions on our behalf.

### The rise of HTTP/2

This supports a feature called single-packet multi-request. With this we can stack multiple requests in the same TCP packet, eliminating network latency, that can cause misunderstanding in timing

### Typical Timing Attacks

Two main categories:
- Information Disclosure
- Race conditions

We will focus on `Time-of-Check to Time-of-Use (TOCTOU)`
When the user submits their coupon code, in the actual code of the web application, at some point, we perform a check that the coupon is valid an hasn't been used before. We apply the discount, and only then do we update the coupon code to indicate that it has already been used.
If a threat actor can send two requests so close together in time, it might happen that before the coupon is updated in request 1, it has already been checked in request 2, meaning that both requests will apply the coupon!