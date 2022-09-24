---
title: Yarn
tags:
  - javascript
public: true
date: 2021-01-03
---

# Yarn

**Yarn** is a package manager for [JavaScript](JavaScript.md) and [TypeScript](TypeScript.md). It is a alternative to [npm](npm.md)

### Links

* [yarnpkg.com](https://yarnpkg.com/)

## Workspaces

Yarn provide a solution to manage [Monorepo](Monorepo.md) - Yarn Workspaces

* Basically a [Monorepo](Monorepo.md) manager
* in comparison with [Lerna.js](Lerna.js.md) Yarn Workspaces are low-level

### Setup from scratch

---

* Source: [yarn-workspaces-sample (GitHub)](https://github.com/maxoidIO/yarn-workspaces-sample)

---

````bash
mkdir yarn-workspaces-sample && cd yarn-workspaces-sample && touch package.json
````

````json
// package.json
{
    "private": true,
    "workspaces": [
        "packages/*"
    ]
}
````

````bash
mkdir packages && mkdir packages/server && mkdir packages/utils
````

````bash
cd packages/utils && yarn init -y
cd ../server && yarn init -y
````

````bash
cd ../utils && touch main.js
````

````js
// packages/utils/main.js
function greeting(name) {
    console.log(`Hello, ${name}`);
}

module.exports = {
    greeting
};
````

````bash
cd ../server && touch main.js
````

````js
// packages/server/main.js

const { greeting } = require('utils');

greeting('byte');

````

````json
// packages/utils/package.json
{
  "name": "utils",
  "version": "1.0.0",
  "main": "main.js", // index.js ==> main.js
  "license": "MIT"
}
````

````json
// packages/server/package.json
{
  // ...
  "dependencies": {
    "utils": "1.0.0"
  }
}
````

````bash
yarn install
````

**yarn install** will link dependency **utils** to **server**

After installing let's check that **server** has workspace dependency **utils**:

````
yarn workspaces info
````

should print this:

````json
{
  "server": {
    "location": "packages/server",
    "workspaceDependencies": [
      "utils"
    ],
    "mismatchedWorkspaceDependencies": []
  },
  "utils": {
    "location": "packages/utils",
    "workspaceDependencies": [],
    "mismatchedWorkspaceDependencies": []
  }
}
````

Add scripts

````json
// packages/server/package.json
{
  // ...
  "scripts": {
    "dev": "node main.js"
  }
}
````

````json
// /package.json (root)
{
    "scripts": {
        "server:dev": "yarn workspace server dev"
    }
}
````
