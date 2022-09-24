---
title: Test asynchronous code using Jest
tags:
  - testing
  - jest
  - javascript
public: true
date: 2021-04-27
---

# Test asynchronous code using Jest

### Test code that contains method setTimeout

Let's say we have some function that called another function after **N** seconds:

````ts
function completeTask(task: Task) {
  setTimeout(() => {
    task.complete();
  }, DELAY);
}
````

Okay, now we want to test that method **task.complete** was called when we called function **completeTask**:

````ts
const { completeTask } = require("./completeTask");

describe("completeTask", () => {
  const FAKE_TASK = {
    complete: jest.fn(),
  };

  it("should complete task", () => {
    completeTask(FAKE_TASK);

    expect(FAKE_TASK.complete).toHaveBeenCalled();
  });
});
````

The test will fail. 

When test is failed [JestJS](JestJS.md) says:

````
A function to advance timers was called but the timers API is not mocked with fake timers. Call `jest.useFakeTimers()` in this test or enable fake timers globally by setting `"timers": "fake"` in the configuration file. This warning is likely a result of a default configuration change in Jest 15
````

So, we updated [JestJS](JestJS.md) configuration file or added **jest.useFakeTimers()** in test's file but test is still failing. 

Why? Because the line with `expect(FAKE_TASK.complete).toHaveBeenCalled();` is called *AFTER* executing code inside setTimeout (after calling **task.complete**).

We need to call method [jest.advanceTimersByTime(DELAY)](https://jestjs.io/docs/jest-object#jestadvancetimersbytimemstorun):

````ts
  it("should complete task", () => {
    completeTask(FAKE_TASK);

    jest.advanceTimersByTime(2500);

    expect(FAKE_TASK.complete).toHaveBeenCalled();
  });
````

Basically, it makes that test (line expect) is executing like if DELAY time (in our case is *2500*) is passed. 

### Code with Promise

Function that has **Promise** inside:

````js
function completeTask(task: Task, onComplete: CompleteCallback) {
  setTimeout(() => {
    task.complete().then(() => {
      onComplete();
    });
  }, DELAY);
}
````

We want to test that callback function **onComplete** has been called:

````ts
describe("completeTask", () => {
  const FAKE_TASK = {
    complete: jest.fn().mockResolvedValue(null),
  };
  const handleComplete = jest.fn();

  it("should complete task", async () => {
    completeTask(FAKE_TASK, handleComplete);

    jest.advanceTimersByTime(2500);

    expect(FAKE_TASK.complete).toHaveBeenCalled();

    expect(handleComplete).toHaveBeenCalled();
  });
});
````

The test will fail because **handleComplete** has never been called

It is because of Promise inside **completeTask**. We can fix that by ***dirty*** hack with **async / await**:

````ts
  it("should complete task", async () => {
    completeTask(FAKE_TASK, handleComplete);

    jest.advanceTimersByTime(2500);

    expect(FAKE_TASK.complete).toHaveBeenCalled();

    await Promise.resolve(); // you can also write "await undefined". but why?...
    expect(handleComplete).toHaveBeenCalled();
  });
````

 > 
 > Don't do that. If you need to do this hack it means you're doing something wrong. It's just demonstration that this hack exists.
