---
title: Arrange in unit test
public: true
tags:
  - testing
date: 2021-04-13
---

# Arrange in unit test

* Preparing to acting of tests
  * All errors in this phase are not actually errors / failures of tests
  * In terms of [JestJS](JestJS.md), most likely, **beforeEach** or **beforeAll** methods
  * Sometimes we need to mock import/require (in [JavaScript](JavaScript.md))
    * It can be done by means of framework like [JestJS](JestJS.md)
    * It can be done by means of independent library like [Rewire](Rewire.md)
  * Here you should write stubs, mocks, etc ([Test Doubles](Test%20Doubles.md))
