---
slug: what-are-type-declaration-files-in-typescript
aliases:
  - /blog/article/what-are-type-declaration-files-in-typescript
date: "2021-05-11 20:00:00"
image: images/what-are-type-declaration-files-in-typescript.png
images: ['images/what-are-type-declaration-files-in-typescript.png']
imageCopyright: Andreea Ch
imageCopyrightUrl: "https://www.pexels.com/@andreea-ch-371539"
title: What Are Type Declaration Files In TypeScript?
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

You probably have seen files with a file extension like *.d.ts* in some JavaScript or TypeScript projects (libraries or frameworks) before and you wondered what are these files and what they are for. In TypeScript they are called Type Declaration files. Let's find out what is that practically.

Let's create a simple example with TypeScript code. Create a folder with the name **typescript-type-defs** or whatever name you want and then let's create a file called **users-list.ts** there with code:

````ts
const users = [
  {
    id: "1",
    name: "John",
    username: "johnsmith11",
    age: 55,
    level: 23,
  },
  {
    id: "2",
    name: "C3PO",
    username: "iamnotrobot",
    age: 112,
    level: 1,
  },
];

export const NAME_FIELD_KEY: keyof User = "name";

interface User {
  id: string;
  name: string;
  username: string;
  age: number;
  level: number;
}

export function getEntityField<Entity>(
  entities: Entity[],
  fieldName: keyof Entity
): unknown[] {
  return entities.map((entity) => entity[fieldName]);
}

export function getUsersName(users: User[]): string[] {
  return getEntityField(users, NAME_FIELD_KEY) as string[];
}

const result = getUsersName(users);

console.log(result);
````

What if need to use Interface User somewhere else? Not a problem, just add `export` before interface:

````ts
export interface User {
  id: string;
  name: string;
  username: string;
  age: number;
  level: number;
}
````

You can import this interface from module **users-list** in another module. However, sometimes we need to use this interface as a common thing between several modules. So, *exporting* interface from one of these modules is not an option. We need to create a special file where we can specify Interface User and use it in modules.

{{< subscription >}}

Create a file with name **typings.d.ts** and moved interface User from file **users-list.ts** into this new file:

````ts
export interface User {
  id: string;
  name: string;
  username: string;
  age: number;
  level: number;
}
````

Now we need to use this interface in module **users-list**. You can just import this interface from **typings.d.ts**:

````ts
import { User } from "./typings";

// ...

export const NAME_FIELD_KEY: keyof User = "name";

// ...

export function getUsersName(users: User[]): string[] {
  return getEntityField(users, NAME_FIELD_KEY) as string[];
}

// ...
````

Let's look at file **typings.d.ts** more. In this file, you cannot write variables, functions, and other code of TypeScript/JavaScript. All you can write there is types or interfaces. You can only define types there and use them in any modules.

Usually, you don't write types in **.d.ts** but in **.ts** files. However, **.d.ts** files are used in projects that are packages or libraries and are originally written in JavaScript. If you have JavaScript library and you have to add an ability to use your library in TypeScript projects, so you need to create **.d.ts** files. Another case is when you write your library in TypeScript but you ship it in compiled JavaScript code. In that case, you can automatically generate Type Declarations based on your TypeScript source code by using the TypeScript compiler (tsc).

Here is an example based on **users-list.ts**. Let's use **tsc** to generate Type Declaration file:

````
tsc users-list.ts --declaration
````

After that you will see a new file called **users-list.d.ts** with the following code:

````ts
import { User } from "./typings";
export declare const NAME_FIELD_KEY: keyof User;
export declare function getEntityField<Entity>(
  entities: Entity[],
  fieldName: keyof Entity
): unknown[];
export declare function getUsersName(users: User[]): string[];
````

So with that Type Declaration file, you provide an ability to work with your library's API and types to someone who uses your library in their project.

There is a huge repository that contains type definitions for many libraries and packages - [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped). You probably have installed [npm](npm.md) packages with names like "@types/\*". The code of some of these packages is in this repository.
