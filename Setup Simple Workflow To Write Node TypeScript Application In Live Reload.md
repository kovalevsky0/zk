---
slug: setup-simple-workflow-to-write-node-typeScript-application-in-live-reload
aliases:
  - /blog/article/setup-simple-workflow-to-write-node-typeScript-application-in-live-reload
date: "2021-05-12 22:10:00"
image: images/setup-simple-workflow-to-write-node-typeScript-application-in-live-reload.png
images:
  [
    "images/setup-simple-workflow-to-write-node-typeScript-application-in-live-reload.png",
  ]
imageCopyright: Negative Space
imageCopyrightUrl: "https://www.pexels.com/@negativespace"
title: "Setup Simple Workflow To Write Node TypeScript Application In Live Reload (Nodemon, ts-node)"
tags:
  - javascript
  - typescript
  - node
  - typescriptbook
keywords:
  - javascript
  - typescript
  - node
type: blog
public: true
---

In this post, we will learn how to set up a Node project with TypeScript. It is not based on any framework or library like Fastify, Express, Nest, etc. Let's say you wanna build just a command-line application by using TypeScript and Node.

First of all, you need to install TypeScript on your computer. Install it by [npm](npm.md) or [Yarn](Yarn.md) globally.

```
npm i -g typescript
```

I'm sure you already installed [Node](Node.md) on your computer but maybe you need to update your version. If so, check out the post about [How To Install or Update Node by Using nvm (Node Version Manager)](https://mkvl.me/blog/article/how-to-install-or-update-node-by-using-nvm).

Okay, now let's create a project's folder with name whatever you want (I name it as **node-ts-setup-example**). Open this folder in Terminal and your editor (I use [Visual Studio Code](Visual%20Studio%20Code.md)).

Initialize the project by npm command:

```
npm init -y
```

Our project as an example will be simple - it is a command-line app that asks users to type their name in the Terminal and then prints greetings with this name.

Let's create a first file of the project - **main.ts**. Just put there very basic [TypeScript](TypeScript.md) code like this:

```ts
import { createInterface } from "readline";
import { promisify } from "util";

const rlInterface = createInterface({
  input: process.stdin,
  output: process.stdout,
});

const question = promisify(rlInterface.question).bind(rlInterface);

function greeting(name: unknown) {
  console.log(`Hello, ${name}!`);
}

async function main() {
  try {
    const name = await question("Type your name: ");

    greeting(name);

    rlInterface.close();
  } catch (e) {
    console.error(e);
  }
}

main();
```

Now let's try to compile this file by using the TypeScript compiler:

```
tsc main.ts
```

{{< subscription >}}

As you may have noticed TypeScript tells us that we need to install Type Declaration for modules of [Node](Node.md) that we use - **readline** and **util**. If you are not familiar with Type Declarations check out the post [What Are Type Declaration Files In TypeScript?](https://mkvl.me/blog/article/what-are-type-declaration-files-in-typescript). For now, let's just install these Type Declarations by [npm](npm.md):

```
npm install --save-dev @types/node
```

Try to compile **main.ts** again:

```
tsc main.ts
```

Great! The file was successfully compiled. Let's run it by [Node](Node.md) and type our name to see greetings:

```
node main.js
```

Awesome. But what if we need to change our code a bit? When we change that we need to compile this file again and run it by Node. It would be great if our code will be automatically compiled and executed after changing. We can automate the process by running TypeScript compiler in **watch mode**:

```
tsc main.ts -w
```

So now TypeScript compiler automatically compiles **main.ts** into JavaScript code. But what about executing this? Well, TypeScript can't execute the code, only compile it.

We can set up the project to automate our development process. Let's start with TypeScript configuration. We need to create TypeScript configuration file in our project. We can use a special command that generates a configuration file with default settings:

```
tsc --init
```

It generated the file **tsconfig.json**. If you open this file you will see there are many options and parameters. I will write about it more in the next posts. All we need to do is focus on parameters **outDir** and **rootDir**. Uncomment these options in the **tsconfig.json**.

**outDir** is the path to folder where will be compiled from TypeScript to JavaScript code.

**rootDir** is the path to folder where are our TypeScript source code of the app. In our case - file **main.ts**.

Specify the options with values:

```json
{
	...
	"outDir": "./dist",
    "rootDir": "./src",
	...
}
```

Also we need to uncomment parameter **moduleResolution** with value **node**:

```json
"moduleResolution": "node", /* Specify module resolution strategy: 'node' (Node.js) or 'classic' (TypeScript pre-1.6). */
```

Create folder **/src** and move **main.ts** there.

Alright. We configured TypeScript for our project. Now we have to configure [Node](Node.md) to execute our code in **watch** mode.

We need to install a few dev dependencies - [ts-node](ts-node.md) and [nodemon](nodemon.md):

```
npm i -D ts-node nodemon
```

**ts-node** is a tool that executes code that is written in TypeScript as if it is written in JavaScript. I mean, you can perceive this as running [Node](Node.md) but for TypeScript files. You can also use **ts-node** as a REPL to execute the code without files.

**nodemon** is a tool that restarts your Node application when some file changes. It really helps in developing because you don't need to re-run [Node](Node.md) if you change code in your application.

Now let's specify section **scripts** in **package.json** file:

```json
{
	...
	"scripts": {
		"dev": "nodemon -w src src/main.ts",
		"start": "node dist/main.js",
		"build": "tsc -p ."
	},
	...
}
```

To run dev server use this command:

```
npm run dev
```

Now if we change our code in **main.ts** it automatically re-compiles and re-run Node to execute the code.
