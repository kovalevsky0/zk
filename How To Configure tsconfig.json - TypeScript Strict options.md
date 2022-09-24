---
slug: how-to-configure-tsconfigjson-typescript-strict-options
aliases:
  - /blog/article/how-to-configure-tsconfigjson-typescript-strict-options
date: "2021-07-29 17:00:00"
image: images/how-to-configure-tsconfigjson-typescript-strict-options.jpeg
images: ['images/how-to-configure-tsconfigjson-typescript-strict-options.jpeg']
title: "How To Configure tsconfig.json: TypeScript Strict options"
tags:
  - typescript
  - typescriptbook
keywords:
  - typescript
  - typescript strict
  - typescript use strict
  - tsconfig.json
  - how to tsconfig
  - useUnknownInCatchVariables
  - noImplicitAny
  - strictNullChecks
  - strictFunctionTypes
  - strictBindCallApply
  - strictPropertyInitialization
  - noImplicitThis
  - alwaysStrict
type: blog
public: true
twitterTags: ["TypeScript", "SoftwareEngineer", "softwaredevelopment", "DEVCommunity", "development", "javascript", "web", "nodejs", "webdevelopment", "howto"]
linkedinTags: ["TypeScript", "SoftwareEngineer", "softwaredevelopment", "DEVCommunity", "development", "javascript", "web", "nodejs", "webdevelopment", "howto"]
---

TypeScript is not just a superset of JavaScript with static types. It is also a quite configurable tool that can be used for different types of projects. One parameter or group of parameters that can be configured is **strict**. If you are not familiar with strict mode and why you should use it for a new project then check out the post [What Is Strict Mode In TypeScript, Why And When You Should Use It?](https://maxoid.io/blog/article/what-is-strict-mode-in-typescript-and-why-and-when-you-should-use-it). In this post I focus more on a practical side of this topic.

TypeScript's strict mode parameter can be configurated as several individual parameters for each specific case of type checking. So, basically, if you set the parameter **strict** to *true* in **tsconfig.json** it means that all these strict options are set to *true*.

List of strict options:

* useUnknownInCatchVariables (new)
* noImplicitAny
* strictNullChecks
* strictFunctionTypes
* strictBindCallApply
* strictPropertyInitialization
* noImplicitThis
* alwaysStrict

Let's explore each strict option in practice.

## TypeScript Strict options in tsconfig.json: useUnknownInCatchVariables

This option was introduced in [TypeScript 4.4](https://devblogs.microsoft.com/typescript/announcing-typescript-4-4-beta/#use-unknown-catch-variables).

The problem is that when we use construction **try catch** the type of variable **error** in *catch* is **any**:

![](/images/strict-typescript-screen-1.png)

It increases the potential risk of errors and application malfunction. The option **useUnknownInCatchVariables** solves this problem.

If you set option **useUnknownInCatchVariables** to `true` then variable **error** in every **try catch** in your code base will have type `unknown`:

````json
{
  "compilerOptions": {
    // ...
    "useUnknownInCatchVariables": true
  }
}
````

![](/images/strict-typescript-screen-2.png)

You can also use type **Error** for error variable:

````ts
try {
  // some code
} catch (e) {
  if (e instanceof Error) {
    console.error(e.message);
  }
}
````

## TypeScript Strict options in tsconfig.json: noImplicitAny

Let's start with option **noImplicitAny**.

In the **main.ts** file (or whatever file you want) let's create a simple function:

````ts
function printData(data) {
  console.log(data);
}
````

If you run `tsc` command you will see that TypeScript successfully compiles the code because there is no error.

Now, set the options in configuration file **tsconfig.json** in your project:

````json
{
  "compilerOptions": {
    "noImplicitAny": true
  }
}
````

If you're writing your code in an editor like [Visual Studio Code](Visual%20Studio%20Code.md) or some IDE you probably already see that something is wrong with parameter **data** in the function. Let's run TypeScript compiler `tsc` and see what it will tell us.

TypeScript compiler will print something like this:

````
error TS7006: Parameter 'data' implicitly has an 'any' type.

4 function printData(data) {
                     ~~~~
Found 1 error.
````

So, if you set the option **noImplicitAny** to `true`, TypeScript won't allow us to write functions with parameters without types of parameters. The thing is that TypeScript doesn't know what type of the parameter **data** is and it doesn't *infer* because there is no information in the code about that value should be there.

You need to set some type to avoid this TypeScript error. For example, I'll specify type **string** for the data:

````ts
function printData(data: string) {
  console.log(data);
}
````

Also, if your parameter is not required, you can specify the default value of the parameter. And there is the thing: if you set the default value of the parameter then you won't need to specify the type. In that case, TypeScript will understand what type of the parameter is by *Type inference*.

An example. The default value of the parameter is empty **string** so type of the parameter is **string**:

````ts
function printData(data = "") {
  console.log(data);
}
````

### TypeScript Strict options in tsconfig.json: Why Should noImplicitAny Be Enabled?

By setting the option **noImplicitAny** to `true`, TypeScript forces you to write safer code. How? The problem with ignorance of the type of the parameter is that you can manipulate the value in the code by methods that can't work with this value. For example, inside the function **printData** you can use method **.toLowerCase** that works with type **string**. Your colleague (or even you!) can use the function **printData** somewhere in the future. Because you don't know what the type of the parameter **data** is, you probably can put the number value to this parameter.

````ts
function printData(data) {
  console.log(data.toLowerCase());
}

async function main() {
  printData(10);
}

main();
````

The code above will successfully be compiled by `tsc` because there are no errors from the TypeScript perspective. But when you will run the program in the [Web browser](Web%20browser.md) or by [Node](Node.md) as in our case, you will see that program falls:

````
node dist/main.js
/ts-node-sample/dist/main.js:13
    console.log(data.toLowerCase());
                     ^
TypeError: data.toLowerCase is not a function
````

You can avoid this error before executing the code by specifying the type of the parameter. The TypeScript's option **noImplicitAny** won't allow you to escape from specifying the type in the new code.

## TypeScript Strict options in tsconfig.json: strictNullChecks

 > 
 > Source code of this example is available on [GitHub](https://github.com/maxoidIO/ts-node-sample/tree/strict-options-strict-null-checks)

This parameter obligates us to make a check of the variable existing. For example, let's say we have an array of some object. This data is available in a code of app from JSON file:

**src/inventory.json**

````json
[
  {
    "id": "1",
    "name": "sword",
    "level": "10",
    "icon": "🗡"
  },
  {
    "id": "2",
    "name": "bow",
    "level": "7",
    "icon": "🏹"
  },
  {
    "id": "3",
    "name": "shield",
    "level": "5",
    "icon": "🛡"
  }
]
````

In some modules, we have a code where this JSON file is imported and used as a database. The app is simple: it asks the user to type the name of the item from inventory and then if this item exists the program will print information about it.

**src/main.ts**

````ts
import { createQuestioner } from "./createQuestioner";
import { greeting } from "./greeting";

import inventory from "./inventory.json";

async function main() {
  try {
    const questioner = createQuestioner();
    const username = await questioner.ask("Type your username: ");

    greeting(username);

    const itemName = await questioner.ask(
      "Type the name of the inventory item: "
    );

    const foundItem = inventory.find((item) => item.name === itemName);

    console.log(
      `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
    );

    questioner.finishUp();
  } catch (e) {
    console.error(e);
  }
}

main();
````

If you run this program by `npm run dev`, type any name and one of three item's names (sword, bow, shield) the program will run as it should. The problems begin when you type the name of the item that *does not exist* in the inventory. If you try this, you'll see something like this:

````
❯ npm run dev

> tsc-intro@1.0.0 dev
> tsc && node dist/main.js

Type your username: byteski
Hello, @byteski!
Type the name of the inventory item: spear
TypeError: Cannot read property 'icon' of undefine
````

All we need to do to fix this problem is to add the code that checks the variable existing before using it for printing the result. But the point is that [TypeScript](TypeScript.md) should highlight that we need to fix the potential problem. To do it just set option **strictNullChecks** to *true*:

**tsconfig.json**

````json
{
  "compilerOptions": {
    // ...
    "strictNullChecks": true
  }
}
````

Now, let's run `npm run dev` and see that happens:

````bash
src/main.ts:20:33 - error TS2532: Object is possibly 'undefined'.

20       `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
                                   ~~~~~~~~~

src/main.ts:20:51 - error TS2532: Object is possibly 'undefined'.

20       `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
                                                     ~~~~~~~~~

src/main.ts:20:74 - error TS2532: Object is possibly 'undefined'.

20       `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
                                                                            ~~~~~~~~~

Found 3 errors
````

Great! Now we have information about where the problem is. Just add checking the variable **foundItem**:

````ts
async function main() {
  try {
    const questioner = createQuestioner();
    const username = await questioner.ask("Type your username: ");

    greeting(username);

    const itemName = await questioner.ask(
      "Type the name of the inventory item: "
    );

    const foundItem = inventory.find((item) => item.name === itemName);

    if (!foundItem) {
      console.log(`There is no item with name '${itemName}' in the inventory.`);
      questioner.finishUp();
      return;
    }

    console.log(
      `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
    );

    questioner.finishUp();
  } catch (e) {
    console.error(e);
  }
}
````

### TypeScript Strict options in tsconfig.json: strictNullChecks and Exclamation mark

{{< subscription >}}

You can also use "!" in such a case when **you are sure** that found item or element exist. Let's see an example:

````ts
async function main() {
  try {
    const questioner = createQuestioner();
    const username = await questioner.ask("Type your username: ");

    greeting(username);

    const listOfItems = inventory
      .map(
        (item) => `${item.id}) ${item.icon} ${item.name} (lvl ${item.level})`
      )
      .join("\n");

    const option = await questioner.ask(
      `\n${listOfItems}\n\nChoose the item (type the number): `
    );

    const itemsIds = inventory.map((item) => item.id);

    if (!itemsIds.includes(option)) {
      console.log(`There is no item with option number ${option}.`);
      questioner.finishUp();
      return;
    }

    const foundItem = inventory.find((item) => item.id === option);

    console.log(
      `You've chosen an item: ${foundItem.icon} ${foundItem.name} (lvl ${foundItem.level})`
    );

    questioner.finishUp();
  } catch (e) {
    console.error(e);
  }
}
````

In this case, a user is not typing the name of the inventory item but type an option number offered by the app. Because the code checks that the user typed option number that surely exists (the line `if (!itemsIds.includes(option)) {`) we don't need to manually check that variable **foundItem** has data inside. But TypeScript will tell us that we need to check this variable because *Object is possibly 'undefined'*. To avoid this highlight we can use **exclamation mark**:

````ts
console.log(
  `You've chosen an item: ${foundItem!.icon} ${foundItem!.name} (lvl ${
    foundItem!.level
  })`
);
````

It tells TypeScript that we are totally sure that **foundItem** is not undefined or null. After that you can run the app it will work correctly.

*I recommend not use **exclamation mark** very often because it can expand the count of potential mistakes in the future. Use it only in case when **you are totally sure** that some data exists.*

## TypeScript Strict options in tsconfig.json: strictBindCallApply

 > 
 > Source code of this example is available on [GitHub](https://github.com/maxoidIO/ts-node-sample/tree/strict-options-strict-bind-call-apply)

The next option is not so useful nowadays since we don't need to use **bind()** and related methods much often in modern JavaScript. But anyway, if you need to use bind(), call(), or apply() then this option might be useful for you.

The example is unusual but you may come across this in existing projects with an old version of ECMAScript (where arrow functions are not available or their support is disabled for some reason). This function creates an object of a non-player character. You can start the dialog with this character (in our example it starts automatically after running the app) but the character is busy right now so it answers later (after 2 sec):

````ts
import { Questioner } from "./createQuestioner";

export function createMerchant(name: string, questioner: Questioner) {
  async function greeting(caller: { name: string; level: number }) {
    console.log("\nDid you complete the quest? \n 1) yes \n 2) no");
    const answer = await questioner.ask("\nYour answer: ");

    if (answer === "1") {
      console.log(`\nExcellent! Now your level is: ${caller.level + 1}`);
    } else {
      console.log("\nSee ya later");
    }

    questioner.finishUp();
  }

  const character = {
    name,
    startDialog: function (caller: { name: string; level: string }) {
      console.log("[This character is busy now]");
      setTimeout(greeting.bind(this, caller), 2000);
    },
  };

  return character;
}
````

Let's create a merchant in **main** module:

````ts
import { createQuestioner } from "./createQuestioner";
import { greeting } from "./greeting";
import { createMerchant } from "./merchant";

async function main() {
  try {
    const questioner = createQuestioner();
    const username = await questioner.ask("Type your username: ");
    const level = await questioner.ask("Type your level: ");

    greeting(username);

    const merchant = createMerchant("Trader", questioner);

    merchant.startDialog({ name: username, level });
  } catch (e) {
    console.error(e);
  }
}

main();
````

Now, if you run the program and type your name and level (for example, 10) and then answer "yes" in dialog (type "1") when you see something goes wrong with your level:

````bash
Excellent! Now your level is: 10
````

Typical problem with `string` and `number` values in [JavaScript](JavaScript.md). Notice that in **createMerchant** in method **startDialog** a parameter *level* has type `string` but in function **greeting** the parameter *caller* has field *level* with type `number`. But we don't have any type checking errors after running **tsc**. We should tell TypeScript to check parameters of function that called by **bind()** (call(), apply()). This is what option **strictBindCallApply** is for.

**tsconfig.json**

````json
{
  "compilerOptions": {
    // ...
    "strictBindCallApply": true
  }
}
````

Now, if you run the program you will see that TypeScript highlights the problem with different types of field *level* in function **createMerchant**:

````bash
src/merchant.ts:21:38 - error TS2769: No overload matches this call.
...
21       setTimeout(greeting.bind(this, caller), 2000);
                                        ~~~~~~
Found 1 error.
````

## TypeScript Strict options in tsconfig.json: strictFunctionTypes

This option is intended for quite specific cases. If this option was set to *true* then TypeScript will not allow you to use a function in a case when types of parameters of this function are not the same as parameter's types in specified *type*.

An example:

````ts
type loggerFn = (id: number | string) => void;

const logTransaction: loggerFn = (id: string) => {
  console.log(`[${new Date().toDateString()}] ${id.trim()}`);
};

logTransaction(transactionId);
````

In this case, if options are enabled, **tsc** will return an error message after running:

````bash
src/main.ts:11:11 - error TS2322: Type '(id: string) => void' is not assignable to type 'loggerFn'.
  Types of parameters 'id' and 'id' are incompatible.
    Type 'string | number' is not assignable to type 'string'.
      Type 'number' is not assignable to type 'string'.

11     const logTransaction: loggerFn = (id: string) => {
             ~~~~~~~~~~~~~~~
Found 1 error
````

Theoretically, in this case you could specify the parameter **id** as a number and call function **logTransaction** like that: `logTransaction(parseInt(transactionId))`. But still, you will have a type-checking error because you cannot use method **trim()** for a number value.

Anyway, is good to know what specific options are needed if you enabled **strict mode** in your project.

## TypeScript Strict options in tsconfig.json: noImplicitThis

You might know that [JavaScript](JavaScript.md) has a quite important nuance with the variable "this". Let's say, you have a method that prints a value of an object's field. If you wrote this method as *function declaration* then will lose "this" of an object where the method is specified. I would say that it's one of the famous "features" of [JavaScript](JavaScript.md) and Internet has tons of materials about this.

Here is an example:

````ts
const createCharacter = (name: string, level: number) => {
  return {
    label: `[${level} lvl] ${name}`,
    info(prefix: string) {
      return function () {
        console.log(`${prefix}: ${this.label}`);
      };
    },
  };
};

const ranger = createCharacter("Ranger", 77);
const printRangerInfo = ranger.info("Neutral");

printRangerInfo();
````

After running `npm run dev` you will see that it throws an error:

````bash
TypeError: Cannot read property 'label' of undefined
````

Now, let's set option **noImplicitThis** in configuration file:

````json
{
  "compilerOptions": {
    // ...
    "noImplicitThis": true
  }
}
````

After that [TypeScript](TypeScript.md) will highlight an error in the code:

````bash
error TS2683: 'this' implicitly has type 'any' because it does not have a type annotation.
14             console.log(`${prefix}: ${this.label}`);
                                         ~~~~
````

````bash
13           return function () {
					~~~~~~~~
An outer value of 'this' is shadowed by this container.
Found 1 error
````

By doing so we can fix the problem before running an application. One of a solution, in this case, is using an arrow function:

````ts
const createCharacter = (name: string, level: number) => {
  return {
    label: `[${level} lvl] ${name}`,
    info(prefix: string) {
      return () => {
        console.log(`${prefix}: ${this.label}`);
      };
    },
  };
};
````

When you change the nested function to arrow one [TypeScript](TypeScript.md) will stop highlight this line as an error. After running `npm run dev` you will see that the program works correctly.

## TypeScript Strict options in tsconfig.json: strictPropertyInitialization

The next option is directly related to classes in [JavaScript](JavaScript.md) and [TypeScript](TypeScript.md). In TypeScript, you can specify the properties of the class and also their types. Here is an example.

Let's say we have a special class for game characters:

````ts
export class Character {
  name: string;
  level: string;

  constructor() {}

  greeting(callerName: string) {
    console.log(`[${this.level}] ${this.name}: Hello, ${callerName}!`);
  }
}
````

Now, in the **main** module we create a character's object. The character should greetings the player:

````ts
async function main() {
  try {
    const questioner = createQuestioner();
    const name = await questioner.ask("Type your first name: ");

    const traveler = new Character();

    traveler.greeting(name);

    questioner.finishUp();
  } catch (e) {
    console.error(e);
  }
}
````

If you run this small example, you will see:

````bash
Type your first name: Max
[undefined] undefined: Hello, Max!
````

I guess we didn't give a name to the traveler! Okay, we made a mistake in the code. It's not a big deal. The real problem is that [TypeScript](TypeScript.md) didn't say anything about it! Notice that `constructor` of class **Character** is empty. But also there is no highlighted error or warning. We don't have a specific syntax like `required name: string` in TypeScript to declare that properties *name* and *level* are required for initialization in the class **Character**. However, we can enable option **strictPropertyInitialization** and after that [TypeScript](TypeScript.md) compiler will tell us that we didn't initialize properties name and level in the constructor method of class Character.

An option **strictPropertyInitialization** can be enabled only if option **strictNullChecks** is enabled too.

````ts
{
  "compilerOptions": {
    // ...
	"strictNullChecks": true,
    "strictPropertyInitialization": true
  }
}
````

And after that we run `tsc` and see:

````bash
error TS2564: Property 'name' has no initializer and is not definitely assigned in the constructor.

2   name: string;
    ~~~~
````

````bash
src/Character.ts:3:3 - error TS2564: Property 'level' has no initializer and is not definitely assigned in the constructor.

3   level: string;
    ~~~~~

Found 2 errors.
````

This is exactly what we need. Now, let's fix the problem:

````ts
export class Character {
  // Class Property Inference from Constructors
  // since version 4.0 TypeScript can “take" types of properties from a constructor
  // so we don't need to specify types of properties 'name' and 'level' here
  name;
  level;

  constructor(name: string, level: number) {
    this.name = name;
    this.level = level;
  }

  greeting(callerName: string) {
    console.log(`[${this.level}] ${this.name}: Hello, ${callerName}!`);
  }
}
````

And don't forget to give a name for the traveler in **main** module!

## TypeScript Strict options in tsconfig.json: alwaysStrict

If you set the option **alwaysStrict** to `true` then [TypeScript](TypeScript.md) will parse your code in [ECMAScript](ECMAScript.md) Strict mode and put "use strict" in each source file. If you are not familiar with [ECMAScript](ECMAScript.md) Strict mode then check out [article on MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Strict_mode) about it.

## Conclusions

When you have already learned what errors can be prevented by TypeScript's strict options you may exclaim "It can be fixed by a few lines of code. Just add a checking of variable's existing before print it. What's the big deal?" and you'll be right. But, it's just a synthetic example to demonstrate the problem that can be solved by strict options. In reality, it could be one small part of a huge project with hundreds of files and thousands of lines of code. You cannot keep track of everything and you shouldn't. You also can make a typo or forget about doing a check because you can't concentrate after last night's party. It can also happen to your new colleague who hasn't completely figured out the codebase yet.

The point is to delegate solving errors that are related to types of variables to tools like [TypeScript](TypeScript.md).
