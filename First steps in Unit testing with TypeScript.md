---
slug: first-steps-in-unit-testing
aliases:
  - /blog/article/first-steps-in-unit-testing
date: "2021-04-18 23:00:00"
title: First Steps in Unit Testing with TypeScript
tags:
  - testing
  - jest
  - javascript
  - react
  - typescript
keywords:
  - testing
  - unit testing
  - tests
  - javascript
  - typescript
  - end to end tests
  - integration tests
  - unit tests
  - test doubles
type: blog
public: true
---

Unit testing is one of the greatest ways to write effective code. In this article, I want to introduce you to what is this type of testing exactly and some basic terms of the Unit testing world.

Because I work mostly with [TypeScript](TypeScript.md) and [React](React.md) ecosystems, I will refer to tools and examples that are commonly used there, but terms and definitions in this article are applicable to all languages and technologies.

## Types of tests

![Pyramid of testing](/images/pyramid-of-testing.png)

Before we dive into the subject of Unit testing, we need to know about other types of testing. In general, the are three types of software testing:

* End-to-End testing
* Integration testing
* Unit testing

### Unit tests

Unit tests, also called as module tests, are tests of modularity. They test specific part of the system (module) independently from other modules of the system.

Unit test should check, for example, module output (the result value that function returns) with different input parameters. This test should not check the result of other modules but output of module that test written for. If module receives data in parameters from another output of another module, we need to mock this data.

Unit test can be a kind of documentation of the module.

### What is a Unit?

Okay, now we know that Unit tests are used to test module (unit). But what is a unit? It depends on the technologies and programming languages that you use. In [TypeScript](TypeScript.md) ([JavaScript](JavaScript.md)) it could be a function or class. In [React](React.md) it will be a component, which is, basically, [JavaScript](JavaScript.md) function.

For each unit, we should write an independent file that contains tests for this unit (module).

But, what if a class or component contains several methods or functions? Do we need to write an independent test for each method/function?

In the case of the class method, it doesn't make sense to write a test for method as for independent module (unit) because methods are inner parts of classes where they are placed. Mostly, methods have no meaning outside their classes, otherwise, they should not be a method of class but an independent function (if it's possible in a programming language).

What about something like [React](React.md) component? Well, it depends. For example, if you have some local state in your component when it doesn't make sense to write a test for the component's function as a unit, because this function, most likely, works with this state. In this case, you should think about the component as a unit itself and it doesn't matter the component has inner functions or not.

Before answering the question *Why we should prefer to write Unit tests as developers?* we should find out about other types of tests.

Typical example of ***Unit*** in [TypeScript](TypeScript.md) - a helper function that doesn't have side effects:

````ts
interface Transaction {
  // ...
  user: User;
}

export const getUsersFromTransactions = (transactions: Transaction[]) =>
  transactions.map(({ user }) => user);
````

Another one is a model class in [TypeScript](TypeScript.md). In this class we have just simple getter methods and fields:

````ts
export class TransactionModel extends Model {
  // some methods and fields

  private get getId(): string {
    return this.id;
  }

  private get getUser(): User {
    return this.user;
  }

  public getPlaceholder(): string {
    const user = this.getUser();
    return `transaction #${this.getId()} for user: ${user.firstName} ${
      user.lastName
    }`;
  }
}
````

An example of ***Unit*** in [React](React.md). Simple component that renders information about user and has inner state:

````tsx
import React, { FC, useState } from "react";

interface Props {
  user: User;
}

export const UserCard: FC<Props> = ({ user }) => {
  const [isPhoneNumberShown, setIsPhoneNumberShown] = useState<boolean>(false);

  const handleBtnClick = (): void => {
    setIsPhoneNumberShown(true);
  };

  return (
    <Card>
      <Avatar src={user.avatarUrl} />
      <table>
        <tbody>
          {/* some code */}
          <tr>
            <td>Phone number:</td>
            <td>
              {isPhoneNumberShown ? (
                <>{user.phoneNumber}</>
              ) : (
                <button onClick={handleBtnClick}>Show phone number</button>
              )}
            </td>
          </tr>
        </tbody>
      </table>
    </Card>
  );
};
````

### End-to-End tests

End-to-End (or e2e for short) tests are used to test software as a whole system from an outside observer's perspective. What does it mean? In [Front End](Front%20End.md) development it looks like this:

* you write a test that "opens" the browser
* it goes to a specific page or view of your application
* it manipulates with the interface of your application: click on buttons, scrolling, types text in forms, etc

The result of these tests should be *correct* behavior of the application's UI. E2E emulates user's interaction with your application. These tests don't know how the system *actually* works inside.

{{< subscription >}}

Technologies that can be used for writing End-to-End test in [TypeScript](TypeScript.md)/[JavaScript](JavaScript.md) ecosystem are:

* [Puppeteer](Puppeteer.md)
* [Playwright](Playwright.md)
* [Cypress](Cypress.md)

### Integration tests

Integration tests (also called module tests) are used to test a group of modules and interacting modules with each other in the system. They test how individual pieces work together as a whole.

In [Front End](Front%20End.md) a great example of this type of test could be a test that checks that the application works well when a few Units (for example, components in [React](React.md)) interacting with each other.

## Why prefer unit testing?

Alright, because we know about a few types of testing, let's discuss *Why should we prefer Unit tests as developers?* Unit tests have several advantages over other tests:

* Speed. Unit tests are written and, mostly, executed faster than other types of tests.
* Unit tests can show us where exactly the error occurred. End-to-End tests check an application as a whole system and you may not understand which part of the system contains the error.
* Because you write Unit tests for specific units like modules, functions, classes, components - you are mentally closer to the code. It's more understandable for you as a developer because you interact with the same concepts as in the code.

## Structure of Unit test

There is a concept of structuring Unit tests called **AAA** - *Arrange*, *Act*, *Assert*. The idea is simple: you split your unit test into three phases:

* Phase *Arrange*. It is a step where you prepare your test before the next phase (Act). Here you should make stubs, mocks, and other stuff (you will read about this below) that is needed for executing a code that the test is for.
  * In terms of *Jest*, these are methods **beforeEach**, **beforeAll**, **afterEach**, **afterAll**.
  * Sometimes, you should make a mock for some modules that are used in the test (in this case we talk about [JavaScript](JavaScript.md) modules that can be used by constructs *import* or *require*). For this purpose, you can use libraries that contain this feature (*Jest*), or you can use a library that is made just for this specific feature ([Rewire](Rewire.md)).
  * The data for input parameters should be prepared here.
* Phase *Act*. In this phase, you write the execution of the unit (function, class, component, etc) that test is for.
* Phase *Assert*. It is a phase where we should write expectations of the module's execution result. If expectations are the same as the result then the test is passed (green), otherwise the test is failed (red).
  * In this phase, we should use some [Assertion](Assertion.md) framework or library to write expectations. It could be a specific library like [Chai.js](Chai.js.md) or a library that contains an ability to write expectations like *Jest*.

## Test Doubles

I have previously mentioned terms such as *mocks* and *stubs*. What do they mean? As we learned earlier, Unit tests are tests of modules and they have to test modules independently of each other. Mostly, modules have input parameters that receive some data. This data can be an output of another module. But we can't just use this another module's output data in the test. It won't be a Unit test. What if this *another module* will be changed inside? Then, the test of the first module will be failed. The problem here is that test will be failed because of the module that the test not for. It would violate the principle of modularity of tests.

That's why we need to create fake data or to create fake behavior of another module for using it all in the input parameters of the tested module. To do this, we can use *Test Doubles*.

### Dummy Object

The Dummy Object is an object that doesn't have any data inside. They are used in tests more like placeholders, not real objects.

An example of the Dummy Object is using an empty class that replaces a real one. The important thing here is Dummy empty class and real class has to inherit from one "parent" class, or they use the same interface.

the Dummy Object is needed when a module that we test has the required parameter but we don't test the module’s behavior that is based on this parameter. We just need to execute the module with some empty data in the parameter that is required.

Here is a simple example of dummy object:

````ts
import { Player } from "./Player";

export class DummyPlayer extends Player {
  // ...

  public getUsername() {
    return "player1";
  }

  public getLevel() {
    return 42;
  }
}
````

An example of test with dummy object:

````ts
import { DummyPlayer } from "./DummyPlayer";
import { GameSession } from "./GameSession";

describe("GameSession", () => {
  // ...

  it("should start session with players", () => {
    const player = new DummyPlayer();
    const gameSession = new GameSession(player);

    gameSession.start();

    expect(gameSession.isStarted).toBe(true);
  });
});
````

### Fake Object

It contains simplified data of the real object. It used to replace some real object. Fake should contain the same data as a real object but not all.

An example of the Fake Object is a fake instance of a database class that stored data in memory. You wouldn't need to read data from the database every time to use it in a test.

A good example of using Fake is replacing **XMLHttpRequest** object by fake one using library [Sinon.js](Sinon.js.md) - [Fake XHR and server](https://sinonjs.org/releases/latest/fake-xhr-and-server).

### Stub

**Stub** is an object which functions return predefined output data. It contains specific rules like *"when parameters are **x1** and **x2** we should return result **y**"*. Stub doesn't need to have parameters: a function can return some predefined data no matter what the parameters are. Predefined data is values that we need to make tests passed.

Stubs guarantee us that test of a specific module won't fail when modules (the outputs of which is used in this module's test) were changed. However, there is another side to the coin. What if the results of these modules were changed too? Then, we will have not actual data (stubs) in the module's test.

How can we avoid this problem? [Static typing](static%20typing.md) can help us here. If you use [TypeScript](TypeScript.md) and you specified interface or type of some module's output, you need to change Stubs in every test where a type of module's output and type of stub's output is different.

Here is an example. In *Jest* you can create stub by using method **spyOn**. It creates stub but it also can be used as a **Spy**:

````ts
import * as helpers from "./helpers";

describe("moveFiles", () => {
  // ...
  it("should return failed status", () => {
    jest.spyOn(helpers, "moveFiles").mockReturnValue({ success: false });

    expect(helpers.moveFiles([], [])).toStrictEqual({
      success: false,
    });
  });
});
````

### Spy

It is a method that is *spying on* specific functions. Spy is tracking information from function about:

* how many times was the function called
* what was the result of the function's call
* with what parameters were the function called

Let's use *Jest* again. We can start to spy on specific function what should be called inside another function which is test for:

````ts
it("should call helper `checkFile`", () => {
  jest.spyOn(helpers, "checkFile");

  helpers.moveFiles(
    [
      {
        name: "file 1",
        ext: "txt",
        path: "/home",
      },
      {
        name: "file 1 // ",
        ext: "txt",
        path: "/home",
      },
    ],
    [
      {
        path: "/usr/etc",
      },
    ]
  );

  expect(helpers.checkFile).toHaveBeenCalledTimes(2);
  expect(helpers.checkFile).toHaveBeenLastCalledWith({
    name: "file 1 // ",
    ext: "txt",
    path: "/home",
  });
});
````

### Mock

**Mock** is an object which functions have specific rules (or expectations), or is just a function with predefined **behavior** and predefined **expectations**. We can avoid API calls and other side effects by using mock.

Okay, let's mock entire implementation of the function from previous example:

````ts
import * as helpers from "./helpers";

const file = {
  name: "file 000",
  ext: "md",
  path: "/home",
};
const checkFile = jest.fn().mockReturnValue(true);

jest.mock("./helpers.ts", () => {
  return {
    moveFiles: jest.fn().mockImplementation(() => {
      checkFile(file);

      return {
        success: true,
      };
    }),
  };
});

describe("moveFiles", () => {
  it("should call helper `checkFile`", () => {
    const result = helpers.moveFiles([], []);

    expect(result).toStrictEqual({
      success: true,
    });
    expect(checkFile).toHaveBeenCalledTimes(1);
    expect(checkFile).toHaveBeenLastCalledWith(file);
  });
});
````

### Fixtures

There is another type of test doubles - Fixtures. They are more used in [Front End](Front%20End.md) development. Fixtures are fake data that replace in test real data from API. Instead of sending a request to a real API, you can use methods that return the same data as from API (fixtures).

In [Back End](Back%20End.md) is used for replacing requests to the real database. If you need some specific state of the database, you can make fixtures that replace data with a specific state from that database.

How to create fixtures? There are several options. If you work on [Front End](Front%20End.md) side, [Back End](Back%20End.md) that you work with can provides you JSON file that generated based on the type of API responses. Sometimes you don't work closely with [Back End](Back%20End.md) engineers (for example - it's API of some external service). Then, you can generate JSON schemes based on API documentation like Swagger / Open API.

## Conclusions

Unit tests help you to write more effective, security code that you can easily change and refactor without fear that you will disrupt a working system. It is not a silver bullet, but there are some techniques and methods that can help you to fix and avoid the issue in Unit testing and development. We will talk about that in the next materials.

Be in touch! 🔔 ✨
