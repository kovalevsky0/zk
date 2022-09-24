---
title: "Name your mocked function with word \"mock\""
tags:
  - javascript
  - jest
  - testing
public: true
date: 2021-04-24
---

# Name your mocked function with word "mock"

Let's look at the example:

````js
const fakeApi = jest.fn().mockImplementation(() => ({
  ok({
    data: []
  })
}));

jest.mock('../some-module.js', () => {
  return jest.fn().mockImplementation(function () {
    this.api = fakeApi;
  });
});
````

It's a part of the test where we mock some module. We also use mocked function "fakeApi" in the mock of module. But it will not work because function "fakeApi" will be in [Temporal Dead Zone In JavaScript](Temporal%20Dead%20Zone%20In%20JavaScript.md).

Basically, it happens because all module's mocks in [JestJS](JestJS.md) moves on the top of test's executing. So, when test is running, `jest.mock('../some-module.js'...` is above than `mockApi`.

We can avoid this behavior. We should name function "fakeApi" with word "mock" at the beginning. Something like that:

````js
const mockApi = jest.fn().mockImplementation(() => ({
  ok({
    data: []
  })
}));

jest.mock('../some-module.js', () => {
  return jest.fn().mockImplementation(function () {
    this.api = mockApi;
  });
});
````
