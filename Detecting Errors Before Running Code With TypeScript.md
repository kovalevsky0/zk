---
slug: detecting-errors-before-running-code-with-typescript
aliases:
  - /blog/article/detecting-errors-before-running-code-with-typescript
date: "2021-05-10 21:10:00"
image: images/detecting-errors-before-running-code-with-typescript.png
images: ["images/detecting-errors-before-running-code-with-typescript.png"]
imageCopyright: Pixabay
imageCopyrightUrl: "https://www.pexels.com/@pixabay"
title: Detecting Errors Before Running Code With TypeScript
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

## The problem

In the [previous post](https://mkvl.me/blog/article/typescript-what-is-all-about-and-why-should-you-use-it) we talked about what is [TypeScript](TypeScript.md) and why should we use that. Now it's time to go to practice.

We need to know how to start using [TypeScript](TypeScript.md) in our [JavaScript](JavaScript.md) project. In examples of this post series, I will use mostly code that written in [Node](Node.md) environment. It won't be any specific code that is understandable for only developers who worked with [Node](Node.md) before. Because this material is about TypeScript I want to specify more on TypeScript itself.

Okay, let's start with an introduction to our first example. Here we have a very simple command-line application that works on [Node](Node.md). This example consists of one file. Let name it **sum.js**. When we execute this file by Node it will ask two questions in the Terminal - a value of argument X and Y. After typing these values the app will print the result of _X + Y_.

> An example of this code is written with [Node](Node.md). If Node is not installed on your machine, check out the post about [How To Install or Update Node by Using nvm (Node Version Manager)](https://mkvl.me/blog/article/how-to-install-or-update-node-by-using-nvm).

Look at the code:

```js
const readline = require("readline");

const rlInterface = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

// there is a simplified version of util.promisify method
const question = (message) => {
  return new Promise((resolve) => {
    rlInterface.question(message, (data) => {
      resolve(data);
    });
  });
};

function sum(a, b) {
  return a + b;
}

async function main() {
  try {
    const argX = await question("Type value of X: ");
    const argY = await question("Type value of Y: ");
    const result = sum(argX, argY);

    console.log(`Result: ${result}`);

    rlInterface.close();
  } catch (e) {
    console.error(e);
  }
}

main();
```

Don't focus on module _readline_, methods _createInterface_ and _question_. It's just a [Node](Node.md) specific code that allows us to take data that the user types in the Terminal. Let's focus on functions _sum_ and _main_. The function _main_ is just an entry point of our small app.

Alright. Now let's test our app that it works correctly. To run the app use this command (if you already in the same folder as file sum.js there):

```
node sum.js
```

The app is asking you to type the value of parameters **X** and **Y**. Let it be 7 and 2.

We expected that result will be 9 but the result is disappointed. The app prints:

```
72
```

There is a **bug**. The thing is that the value that function _question_ returns have type **string**, not **number** as it expected in function _sum_.

It is a typical problem with [JavaScript](JavaScript.md) type system. I would say, it is a JavaScript trademark. Probably, you could see memes and jokes about this problem.

It's all perfectly fine, but how can we avoid this problem? Of course, you can change function _sum_ and do something like this (unary add operator):

```js
function sum(a, b) {
  return +a + +b;
}
```

But don't you think that this looks like a bad solution? It seems like that we try to use a patch to hide a hole in the tearing jacket. Instead of this, we can put on a new jacket that won't have holes (or maybe less than the previous one) - [TypeScript](TypeScript.md).

## The solution

### Installing TypeScript

To install [TypeScript](TypeScript.md) globally on your machine let's use [npm](npm.md):

```
npm install -g typescript
```

Alright. Now we need to check that TypeScript was installed. Type this command in the Terminal:

```
tsc --version
```

It should print you something like this:

```
Version 4.2.4
```

It means that [TypeScript](TypeScript.md) was successfully installed on our machine. What is **tsc** command? It is a **TypeScript compiler**. As mentioned in the [previous post](https://mkvl.me/blog/article/how-to-install-or-update-node-by-using-nvm), TypeScript compiler is a tool, or program, that turns the TypeScript code into JavaScript code. We need this feature because we will execute this compiled JavaScript code by [Node](Node.md).

{{< subscription >}}

### From JavaScript to TypeScript

Alright. To solve the problem we need to write the same code as before but in TypeScript. Let's change extension of the JavaScript file **sum.js** to TypeScript file extension - **.ts**. Just rename the file from **sum.js** to **sum.ts** and let's see that we will have it in the editor.

We just renamed our file but there are already some changes in the editor (I use [Visual Studio Code](Visual%20Studio%20Code.md)):

![TypeScript code in VSCode](/images/typescript-first-using-screen-1.png)

We have several lines with red underlining which means that there are TypeScript errors. There are also two dashed borders on line 11 - TypeScript warnings. But, why don't we just ignore all this stuff and execute our code? Let's try it.

For executing this file now we must first compile it by TypeScript compiler.

Run this command in the Terminal to compile TypeScript file **sum.ts**:

```
tsc sum.ts
```

Oops! After running this command we will see that our code cannot be compiled because of the errors that were marked in the editor.

![TypeScript Compiler shows errors](/images/typescript-first-using-screen-2.png)

And **there is a thing**. TypeScript won't allow you to compile the code that contains errors.

### Fixing the code

To compile and execute this file we need to fix the code in the file. Let's see what the errors we have there.

The first four problems are about the same thing.

```
error TS2468: Cannot find global value 'Promise'.

sum.ts:3:18 - error TS2580: Cannot find name 'require'. Do you need to install type definitions for node? Try `npm i --save-dev @types/node`.

3 const readline = require("readline");
```

TypeScript tries to understand the types of modules that we use in the code - **readline**. To help TypeScript know the types of the modules we need to install **type definitions**. You will learn about it more in the next posts. For now, let's just say that **type definitions** is a special notation that helps TypeScript to know types of code that were originally written in JavaScript.

Let's install it as TypeScript tells us:

```
npm install --sade-dev @types/node
```

Then, try to compile file **sum.ts** again:

```
tsc sum.ts
```

Great! We don't have any errors and successfully compiled our TypeScript file into JavaScript. You should see that there is a new file called **sum.js** in the same folder as **sum.ts**. No, this is not the file that we created before. This file contains compiled JavaScript code of **sum.ts** file.

If you open this file, well... You may be afraid. There is no our code at all! Don't jump to conclusions. It still the same code as we wrote in **sum.ts** but it transformed into a form that is more understandable for runtime environment (in our case - [Node](Node.md), also it might be [Web browser](Web%20browser.md)).

Okay, let's execute our code again. But note that we have to execute _compiled code_, i.e. **sum.js**, not **sum.ts**:

```
node sum.js
```

Let's type new values: 13 and 7. We will see the wrong result, again.

```
137
```

But you said that we will solve this problem by using TypeScript and we will catch the errors before executing the file! Well, there is another thing of TypeScript that you need to remember. _You wanna help? Help yourself!_. In our case, it means that we have to say to TypeScript where the problem can be.

### Use types to prevent bug

Let's describe our problem in the code. The function _question_ returns a value that has a type string. But we don't know about it before executing the file. Because we don't know it we bravely put the values that function _question_ returns into a parameter of function **sum**. The function **sum** expected that values will have type _number_ and it worked with them as if they were numbers.

So, firstly, we need to say to TypeScript that function _question_ returns string type. Let's do it!

To specify what type of value function returns we should write this code:

```ts
const question = (message): string => {
  return new Promise((resolve) => {
    rlInterface.question(message, (data) => {
      resolve(data);
    });
  });
};
```

Hmm. We specified the type of returned value, but TypeScript shows that there is an error:

![](/images/typescript-first-using-screen-3.png)

An error sounds like this:

```
Type 'Promise<unknown>' is not assignable to type 'string'.ts(2322)
```

It means that we cannot just specify the type **string** as a type of returned value of function _question_ because the function _question_ is an asynchronous function and returns Promise.

Alright. To specify the type in this kind of function we just need to specify it like `Promise<your_type>` as TypeScript writes to us in the text of the error.

Let's fix that:

```ts
const question = (message): Promise<string> => {
  return new Promise((resolve) => {
    rlInterface.question(message, (data) => {
      resolve(data);
    });
  });
};
```

Okay. Did we tell TypeScript there might be a problem? Not yet. The next step is to specify the types of parameters of the function _sum_.

To specify types of function's parameters we should write this code:

```ts
function sum(a: number, b: number) {
  return a + b;
}
```

Let's look at the function _main_ where functions _question_ and _sum_ are calling:

![An error in the code](/images/typescript-first-using-screen-4.png)

**This is it!**. This is the error that helps us to fix the bug with the wrong result that prints in the Terminal. Now, if we would try to compile file **sum.ts** we will see the error.

To run our program in one file use this command:

```
tsc sum.ts && node sum.js
```

We will see:

![The error in executing the program](/images/typescript-first-using-screen-5.png)

All we have to do is to write a code that _converts_ values from string type to number:

```diff
async function main() {
  try {
    const argX = await question("Type value of X: ");
    const argY = await question("Type value of Y: ");
    + const x = Number(argX);
    + const y = Number(argY);
    - const result = sum(argX, argY);
    + const result = sum(x, y);

    console.log(`Result: ${result}`);

    rlInterface.close();
  } catch (e) {
    console.error(e);
  }
}
```

Let's see the result of executing our program:

![](/images/typescript-first-using-screen-6.png)

**Congratulations!** You solve the problem and prevent the bug by using TypeScript!

TypeScript compiler is a very configurable tool. In the next post of the series we deep dive into configuring TypeScript.
