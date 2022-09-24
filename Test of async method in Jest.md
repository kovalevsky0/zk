---
title: Test of async method in Jest
public: true
tags:
  - testing
  - jest
  - javascript
date: 2021-04-14
---

# Test of async method in Jest

An example of code:

````js
module.exports = {
  checkUser: (user, cb) => {
    if (user.age < 18) {
      setTimeout(() => {
        // do smth
        cb(user.age);
      }, 500);
    }
  },
};
````

If you write test like this:

````js
describe("example", () => {
  it("First test", () => {
    expect.hasAssertions(); // it checks that at least one "expect" was called

    const user = {
      age: 10,
    };

    checkUser(user, (age) => {
      expect(age).toBe(user.age);
    });
  });
});
````

**This test will fail**

To make this test passed you should use callback function in method **it** or use *Promise*

````js
describe("example", () => {
  it("First test", (done) => {
    expect.hasAssertions();

    const user = {
      age: 10,
    };

    checkUser(user, (age) => {
      expect(age).toBe(user.age);
      done();
    });
  });
});
````
