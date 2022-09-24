---
title: End-to-End tests
aliases:
  - End-to-End testing
tags:
  - testing
public: true
date: 2021-04-06
---

# End-to-End tests

End-to-End (e2e) tests are tests that emulate user's behaviour. Basically, these tests looks like that:

1. automatically opens browser
1. open specific page
1. click on specific field, control or button on the page
1. we should see that happens when "user" interacts with some part of UI

### Why we don't always need End-to-End?

* End-to-End tests show state of the system / UI from perspective of external observer ("user").
* We don't know that *actually* happens inside system / UI. 
* So, basically we just can know about that some error happens but not:
  * *why does this error happen?* or 
  * *where (inside system) does this error happen?*
* End-to-End **tests are slow**
