---
title: The problem with mutation of object data in Jest
public: true
tags:
  - testing
  - jest
  - javascript
date: 2021-04-14
---

# The problem with mutation of object data in Jest

Let's say we have some method that just set fields like this:

````js
joinServer({ host, user }) {
    user.serverId = host.serverId;
}
````

Here is the example of test of this method:

````js
  it("should move user to host's server", () => {
    const user1 = {
      id: 1,
      serverId: 1,
    };
    const user2 = {
      id: 2,
      serverId: 2,
    };

    joinServer({
      host: user1,
      user: user2,
    });

    expect(user2.serverId).toBe(user1.serverId);
  });
````

Okay, this test is passed. Now, for some reason, we changed the code of the method. It could be just a mistake. Now it looks like this:

````js
  joinServer({ host, user }) {
    // before
    // user.serverId = host.serverId;
    // after
    host.serverId = user.serverId;
  },
````

Let's run test. It's still successful! But it's wrong.

To solve this problem we should write fixtures in test like this:

````js
  it("should move user to host's server", () => {
    const USER1_SERVER_ID = 1;

    const user1 = {
      id: 1,
      serverId: USER1_SERVER_ID,
    };
    const user2 = {
      id: 2,
      serverId: 2,
    };

    joinServer({
      host: user1,
      user: user2,
    });

    expect(user2.serverId).toBe(USER1_SERVER_ID);
  });
````

And now test is failed because method has wrong behavior.
