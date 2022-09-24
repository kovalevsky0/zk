---
title: Test Doubles
tags:
  - testing
public: true
date: 2021-04-12
---

# Test Doubles

* Dummy (Dummy Object)
  * doesn't have any data inside
  * can be empty object {}
  * just exists
  * that's the purpose of the Dummy Object
* Fake
  * has simple data
    * contains simplified data of the object that is replaced by this fake object
    * example: using in-memory db or some "light" db (SQLite?) instead of real db on the project (like PostgreSQL, Oracle)
* [Stub](Stub.md)
* [Spy (in testing)](Spy%20%28in%20testing%29.md)
* Mock
  * it contains expectations about what parameters should be when the function will be called
    * if function was called with different parameters than expected parameters then test will be failed
* [Fixtures](Fixtures.md)

### Implementation

* For writing all this stuff you can use:
  * Functionality of the framework like [JestJS](JestJS.md)
  * Independent library like [Sinon.js](Sinon.js.md)
