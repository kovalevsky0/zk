---
slug: configuring-typescript-options-rootdir-outdir-module-target
aliases:
  - /blog/article/configuring-typescript-options-rootdir-outdir-module-target
date: "2021-05-18 20:00:00"
image: images/configuring-typescript-options-rootdir-outdir-module-target.png
images:
  ["images/configuring-typescript-options-rootdir-outdir-module-target.png"]
imageCopyright: Pixabay
imageCopyrightUrl: "https://www.pexels.com/@pixabay"
title: "Configuring TypeScript: options rootDir, outDir, module, target"
tags:
  - javascript
  - typescript
  - typescriptbook
keywords:
  - javascript
  - typescript
  - typescript config
type: blog
public: true
---

In the post [Getting Started With TypeScript Compiler (tsc)](https://mkvl.me/blog/article/getting-started-using-and-configuring-typescript-compiler) we started to use TypeScript compiler with a few options in a simple Node project. We used **tsc** command to compile our app with specific rules and in watch mode. It works but we can use much more options of TypeScript compiler. However, use it just like a command in the Terminal with a bunch of flags is not so comfortable. We can improve our workflow by using TypeScript configuration file - **tsconfig.json**. In this post, we will learn how to create this file and configure it.

The project example will be the same as in the post [Getting Started With TypeScript Compiler (tsc)](https://mkvl.me/blog/article/getting-started-using-and-configuring-typescript-compiler). It is also available on GitHub as repository [maxoidIO/ts-node-sample](https://github.com/mkvl0/ts-node-sample). If you didn't make the project from previous post from scratch - just download the repository from GitHub or clone the repository by this git command:

```
git clone https://github.com/mkvl0/ts-node-sample.git
```

## Creating configuration file

Alright, let's start with creating the TypeScript configuration file. In the root folder of the project just use **tsc** with a special flag:

```
tsc --init
```

You will see the message from **tsc** that the file was successfully created. Now we have a new file in the folder called **tsconfig.json**. Let's look into this file. You will see a JSON with a bunch of different options. Most of them are commented but few options are already enabled by default. You can check out the whole commands with commentaries in **tsconfig.json** or check out [documentation](https://aka.ms/tsconfig.json) by yourself. In this guide, we will configure TypeScript for our small project from scratch so you need to delete all these options. Your **tsconfig.json** should look like this:

```json
{
  "compilerOptions": {}
}
```

Okay. We already have the command from the previous post that compiles TypeScript with specific rules and in watch mode:

```
tsc --module ESNext --outDir "./dist" --watch src/main.ts
```

Just a reminder. It compiles file **main.ts** that is in folder **/src**. Compiled JavaScript files will be in the folder **/dist**. The option **--module ESNext** means that **tsc** will compile files in JavaScript code with [ES Modules](ES%20Modules.md).

Now, let's configure the TypeScript compiler in **tsconfig.json**.

## rootDir and outDir

First of all, we need to specify the folders for source code and output code. We already have folders **/src** and **/dist** for it. We just need to tell TypeScript to look at **/src** as a folder that contains TypeScript files with source code and to compile all files into the folder **/dist**. For this purpose we can use options **rootDir** and **outDir**.

- **rootDir** is the path to the folder with the source code of the app (in our case it is **/src**)
- **outDir** is the path to the folder with compiled JavaScript files that will be executed by Node or [Web browser](Web%20browser.md) (in our case it is **/dist**)

Change the **tsconfig.json**:

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist"
  }
}
```

Delete the folder **/dist** just to make sure that the TypeScript compiler will create it after compilation based on our configuration file:

```
rm -r dist
```

Because we use configuration file we don't need to use any options or specify file entry point (src/main.ts). Just use in the root folder of the project:

```
tsc
```

You will see that **tsc** successfully created folder **/dist** with compiled JavaScript code.

Run the app just to make sure that everything works as before:

```
node dist/main.js
```

{{< subscription >}}

## module

We already know from the post [Getting Started With TypeScript Compiler (tsc)](https://mkvl.me/blog/article/getting-started-using-and-configuring-typescript-compiler) that we can tell TypeScript to compile the code into JavaScript that uses [ES Modules](ES%20Modules.md) instead of [CommonJS Modules](CommonJS%20Modules.md). For that purpose we used the special option of **tsc**:

```
tsc --module ESNext src/main.ts
```

Now we can specify it in **tsconfig.json**:

```
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
    "module": "ESNext"
  }
}
```

It works the same as the flag **--module** of **tsc**. You can compile the code again and see that now it uses ES Modules in compiled JavaScript code:

**dist/main.js**

```js
// ...
import { createQuestioner } from "./createQuestioner";
import { greeting } from "./greeting";
// ...
```

## target

The next important option of the TypeScript compiler is called **target**. You may notice that when we created **tsconfig.json** by command `tsc --init` the option **target** has already been set with value **es5** in the configuration file.

It means that TypeScript will compile the code to JavaScript code of version ES5. In other words, this compiled code can be executed by the browser or Node with a version that supports a version of JavaScript (ECMAScript) that is no more than ES5. So, if your environment where you need to run your application (some specific version of web browser or Node) doesn't support modern features of JavaScript, you should set option **target** with the version of JavaScript that is supported by this environment.

In practice if your environment is a Web browser, i.e. you work on the Front End project, you probably will use value **es2015** of option **target**. Of course, if you don't have some specific web browser and you need to run JavaScript with version **ES3**.

For the Node, there is information [on GitHub](https://github.com/microsoft/TypeScript/wiki/Node-Target-Mapping) with recommendations on what **tsconfig.json** settings to use.

A table with information about which **target** to use for a specific Node version:

| version | target |
| ------- | :----: |
| 16      | ES2021 |
| 14      | ES2020 |
| 12      | ES2019 |
| 10      | ES2018 |
| 8       | ES2017 |

Also, check out the project [node.green](https://node.green) that contains information about Node.js ECMAScript compatibility.

### Example

In our code example in file **main.ts** we use async/await construction to manage asynchronous code. async/await construction have been available since the ES2017 version of [ECMAScript](ECMAScript.md).

How it looks like in **main.ts**:

```ts
const firstName = await questioner.ask("Type your first name: ");
const username = await questioner.ask("Type your username: ");
```

Set the option **target** to **ES2015**:

```json
{
  "compilerOptions": {
    "rootDir": "./src",
    "outDir": "./dist",
    "target": "ES2015"
  }
}
```

And compile the code:

```
tsc
```

Now, open the file **dist/main.js** in the editor. You will see that where in our code was async/await construction, there is now something else:

```js
// ...
function main() {
  return __awaiter(this, void 0, void 0, function* () {
    try {
      const questioner = createQuestioner();
      const firstName = yield questioner.ask("Type your first name: "); // async await??
      const username = yield questioner.ask("Type your username: ");
      greeting(firstName, username);
      questioner.finishUp();
    } catch (e) {
      console.error(e);
    }
  });
}
// ...
```

The reason why compiled code doesn't have async/await that we used in **main.ts** is that the code was compiled into the version of JavaScript that can be executed by Web browser or Node that doesn't support async/await construction.

Now, set the option **target** to value **ES2017** (or any version that is more than ES2017) and run `tsc`. Open file **dist/main.js** again. You will see:

```js
// ...
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
// ...
```
