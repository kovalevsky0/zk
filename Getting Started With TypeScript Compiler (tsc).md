---
slug: getting-started-using-and-configuring-typescript-compiler
aliases:
  - /blog/article/getting-started-using-and-configuring-typescript-compiler
date: "2021-05-16 20:00:00"
image: images/getting-started-with-typescript-compiler.png
images: ['images/getting-started-with-typescript-compiler.png']
imageCopyright: Francesco Ungaro
imageCopyrightUrl: "https://www.pexels.com/@francesco-ungaro"
title: Getting Started With TypeScript Compiler (tsc)
tags:
  - javascript
  - typescript
  - typescriptbook
keywords:
  - javascript
  - typescript
type: blog
public: true
---

From the previous post [TypeScript - What Is All About And Why Should You Use It?](https://maxoid.io/blog/article/typescript-what-is-all-about-and-why-should-you-use-it) we know that TypeScript is a superset of JavaScript and programming language. But how can you use it? If you worked with JavaScript in Front End you know that your code is executing by [Web browser](Web%20browser.md). In Back End, your code is running by [Node](Node.md). What about TypeScript?

The thing is that TypeScript is provided with a special program, tool - compiler. A compiler is a program that compiles (transforms) one code to another.

## What is TypeScript compiler?

As we mentioned before, TypeScript compiler is a tool, or program, that *compiles* (transforms) **valid** TypeScript code into JavaScript code. It is also a type checker and it validates TypeScript code

When you install TypeScript by [npm](npm.md) or [Yarn](Yarn.md) globally, the TypeScript compiler will be available on your local machine as a command **tsc**:

````
npm i -g typescript
tsc --version
````

TypeScript compiler has many flags and options to use in various types of projects. You can use it in Front End project with libraries like [React](React.md). In [Angular](Angular.md) it is already used inside Angular's toolchain. You can also use **tsc** in Back End development with [Node](Node.md). Here is the post about [How To Setup Node TypeScript Workflow](https://maxoid.io/blog/article/setup-simple-workflow-to-write-node-typeScript-application-in-live-reload).

In this post, we will explore how to use **tsc** with a few general options.

## Usage

We will use **tsc** with a simple example. It is an [command-line interface](command-line%20interface.md) app that asks us to type our first name and username and then greetings us. It is a Node.js application and we will execute it by Node. If you didn't install Node or you have Node with a version that is lower than 15 on your local machine, check out the post [How To Install or Update Node by Using nvm (Node Version Manager)](https://maxoid.io/blog/article/how-to-install-or-update-node-by-using-nvm).

If you don't know how to run a TypeScript compiler, I recommend checking out the post [Detecting Errors Before Running Code With TypeScript](https://maxoid.io/blog/article/detecting-errors-before-running-code-with-typescript). We will use pretty much the same example as in that post but with small differences.

Let's create a folder called **tsc-intro** or whatever you want. First of all, create two helper modules (files) with the following code:

**createQuestioner.ts**:

````ts
import { createInterface } from "readline";
import { promisify } from "util";

interface Questioner {
  ask(text: string): Promise<string>;
  finishUp(): void;
}

export function createQuestioner(): Questioner {
  const rlInterface = createInterface({
    input: process.stdin,
    output: process.stdout,
  });

  const ask = promisify(rlInterface.question).bind(rlInterface);

  const finishUp = () => {
    rlInterface.close();
  };

  return {
    ask,
    finishUp,
  };
}
````

**greeting.ts**:

````ts
export function greeting(firstName: string, username: string) {
  console.log(`Hello, ${firstName} (@${username})!`);
}
````

These two functions will be used in **main** module of our app which is an entry point. Let's create file **main.ts**:

````ts
import { createQuestioner } from "./createQuestioner";
import { greeting } from "./greeting";

async function main() {
  try {
    const questioner = createQuestioner();
    const firstName = await questioner.ask("Type your first name: ");
    const username = await questioner.ask("Type your username: ");

    greeting(firstName, username);

    questioner.finishUp();
  } catch (e) {
    console.error(e);
  }
}

main();
````

Also, you need to install Type Declarations of Node as a dev dependency in your local project. If you don't know about Type Declarations, check about the post [What Are Type Declaration Files In TypeScript](https://maxoid.io/blog/article/what-are-type-declaration-files-in-typescript).

````
npm install --save-dev @types/node
````

Don't focus on the code too much. It's just an example of TypeScript code but first of all, we need to focus on using the TypeScript compiler.

Alright. Now it's time to use the TypeScript compiler to *transform* the TypeScript code into JavaScript code that we will execute by Node:

````
tsc main.ts
````

Great! Now you have compiled file **main.js** that you can execute by command:

````
node main.js
````

You should have noticed that there are also new files that we didn't create in our folder: **createQuestioner.js** and **greeting.js**. Although we compile only file **main.ts**, TypeScript also compiles all modules that were used in **main.ts** - greeting.ts and createQuestioner.ts. The code from these modules will be executed by Node when we will run it by `node main.js`.

If you are worked with Node.js before you may noticed that we imported modules in **main.ts** by using `import smth from 'module'` ([ES Modules](ES%20Modules.md)) not `const smth = require('module')` ([CommonJS Modules](CommonJS%20Modules.md)). Of course, modern Node.js can work with ECMAScript modules. However, CommonJS Modules are still a general way to import and export modules in Node.

{{< subscription >}}

So, how does it works? The thing is that TypeScript by default compiles code that we wrote using ECMAScript modules into the JavaScript code with CommonJS Modules. Let's look into compiled files:

**createQuestioner.js**:

````js
"use strict";
exports.__esModule = true;
exports.createQuestioner = void 0;
var readline_1 = require("readline");
var util_1 = require("util");
function createQuestioner() {
  var rlInterface = readline_1.createInterface({
    input: process.stdin,
    output: process.stdout,
  });
  var ask = util_1.promisify(rlInterface.question).bind(rlInterface);
  var finishUp = function () {
    rlInterface.close();
  };
  return {
    ask: ask,
    finishUp: finishUp,
  };
}
exports.createQuestioner = createQuestioner;
````

**greeting.js**:

````js
"use strict";
exports.__esModule = true;
exports.greeting = void 0;
function greeting(firstName, username) {
  console.log("Hello, " + firstName + " (@" + username + ")!");
}
exports.greeting = greeting;
````

No code uses ECMAScript modules! In **createQuestioner.js** on line 20 the function *createQuestioner* is exporting by using CommonJS `exports.greeting = greeting;`. The same in **greeting.js**: on line 7 you will see the code `exports.greeting = greeting;` which is CommonJS.

Okay, the exporting is sorted out. What about importing modules?

Let's look into the file **main.js**:

*The file is quite large so I cut the code that is not important for us right now*

````js
"use strict";
// some code here
exports.__esModule = true;
var createQuestioner_1 = require("./createQuestioner");
var greeting_1 = require("./greeting");
function main() {
  return __awaiter(this, void 0, void 0, function () {
    var questioner, firstName, username, e_1;
    return __generator(this, function (_a) {
      switch (_a.label) {
        case 0:
          _a.trys.push([0, 3, , 4]);
          questioner = createQuestioner_1.createQuestioner();
          return [4 /*yield*/, questioner.ask("Type your first name: ")];
        case 1:
          firstName = _a.sent();
          return [4 /*yield*/, questioner.ask("Type your username: ")];
        case 2:
          username = _a.sent();
          greeting_1.greeting(firstName, username);
          questioner.finishUp();
          return [3 /*break*/, 4];
        case 3:
          e_1 = _a.sent();
          console.error(e_1);
          return [3 /*break*/, 4];
        case 4:
          return [2 /*return*/];
      }
    });
  });
}
main();
````

On lines 4 and 5 (in the file - 39 and 40), you will see that modules **greeting** and **createQuestioner** is imported by CommonJS modules.

The great thing is that TypeScript is a very configurable tool and we can compile TypeScript to the JavaScript code that uses ECMAScript Modules!

All we have to do is to use option **--module** with value **ESNext**:

````
tsc --module ESNext main.ts
````

The value **ESNext** means that TypeScript will compile code into the latest version of ECMAScript standard. For the purpose to use ECMAScript modules in compiled code it works for us.

Let's look into compiled file **main.js** again:

````
// ...
import { createQuestioner } from "./createQuestioner";
import { greeting } from "./greeting";
function main() {
    // ...
}
main();
````

Great! We have the code with imports that we need. It's time to execute thins code by Node:

````
node main.js
````

And... It fails. Node tells us that we need to specify parameter **type** with value **module** in file **package.json**. First of all, we need to create package.json in our folder:

````
npm init -y
````

And then add the parameter in the file:

````json
{
  "devDependencies": {
    "@types/node": "^15.3.0"
  },
  "name": "tsc-intro",
  "version": "1.0.0",
  "main": "main.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "description": "",
  "type": "module"
}
````

Try to run main.js again:

````
node main.js
````

It fails again!

````
Error [ERR_MODULE_NOT_FOUND]: Cannot find module 'tsc-intro/createQuestioner' imported from /tsc-intro/main.js
Did you mean to import ../createQuestioner.js?
````

The new problem is that Node can't understand modules that are imported without file extensions **.js**. To solve this just use the special Node option for now:

````
node --es-module-specifier-resolution=node main.js
````

## Separating files into different folders

Everything works fine. But we have some mess in a folder. There are files that we wrote in TypeScript and also compiled JavaScript files. Let's clean the folder.

We can manage it by separating files into different folders. One is for source code that we write and the second one is for output code that will be executed by Node. We will use the TypeScript compiler for that purpose.

Create the folder **/src** and put all **.ts** files there:

````
mkdir src
mv *.ts src/
````

Also, remove all compiled **.js** files in the root folder:

````
rm *.js
````

All we have to do is run TypeScript compiled with special options **outDir** which is a path to the folder there should be compiled JavaScript files.

````
tsc --module ESNext --outDir "./dist" src/main.ts
````

## Watch mode

Most of the time, we need to quickly change something in the code and see the result of the changes right now. Use this whole **tsc** command whenever we need to re-compile our project is a bit uncomfortable. We can use option **--watch** that re-run TypeScript compiler every time when files changes in **/src** folder.

````
tsc --module ESNext --outDir "./dist" --watch src/main.ts
````

Using **watch mode** is not enough for developing Node.js application because we also need to re-run Node after changes in the code. Check out the post [How To Setup Simple Workflow To Write Node TypeScript Application In Live Reload](https://maxoid.io/blog/article/setup-simple-workflow-to-write-node-typeScript-application-in-live-reload).

## Checking code without compilation

Another aspect of using TypeScript in modern Front End or Back End development is that we do not always need to compile TypeScript code into JavaScript code by using **tsc**. We can also use [Babel](Babel.md) for that purpose.

Compiling TypeScript or JavaScript code can be a quite long process. If you need to just check your types and validate your code you can use TypeScript without compilation by this command:

````
tsc --noEmit ./src/main.ts
````

## Conclusions

In this post, we learned how to use TypeScript compiler with just several general options. We configured **tsc** by using command's flags but it also can be managed by using configuration file - **tsconfig.json**. In the next post, we will see how to configure **tsc** by **tsconfig.json**.
