---
slug: typescript-what-is-all-about-and-why-should-you-use-it
aliases:
  - /blog/article/typescript-what-is-all-about-and-why-should-you-use-it
date: "2021-05-08 19:50:00"
image: images/typescript-what-is-all-about-and-why-should-you-use-it.png
images: ['images/typescript-what-is-all-about-and-why-should-you-use-it.png']
imageCopyright: eberhard grossgasteiger
imageCopyrightUrl: "https://www.pexels.com/@eberhardgross"
title: TypeScript - What Is All About and Why Should You Use It?
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

## What is TypeScript?

**TypeScript** is a [JavaScript](JavaScript.md) Superset. It means that [TypeScript](TypeScript.md) is a language that is built upon JavaScript. It is based on JavaScript syntax, constructions, advantages (and disadvantages) but it also brings new features, syntax, and capabilities.

What's the purpose of this language? TypeScript brings to developers some features that help to write code on [JavaScript](JavaScript.md) easier and safer. One of the most important features that TypeScript provides is [Static typing](static%20typing.md). Basically, static typing allows us to make fewer mistakes with data types. For example, you can't put some value as an argument of the function if this value doesn't have the same type as the parameter. This is very basic stuff but [TypeScript](TypeScript.md) has also more powerful features that we will find out in the next posts of this series.

**TypeScript** has the ability to work with the same API and environments as JavaScript has, like Browser API or [Node](Node.md). However, [Web browser](Web%20browser.md) and Node can't *execute* TypeScript because they work only with JavaScript. So, how can we use [TypeScript](TypeScript.md) if we can't execute the code in [JavaScript](JavaScript.md) environments like a Web browser or Node?

The thing is that [TypeScript](TypeScript.md) is not just a language but a powerful tool, compiler, that can compile (*transform*) code that is written in TypeScript to [JavaScript](JavaScript.md) code. And *that* compiled JavaScript code we can execute in the browser or [Node](Node.md). TypeScript compiler *transforms* code with features that available only in TypeScript to JavaScript general code. And there is a thing. All types and other TypeScript constructions won't be in the code that you will execute on the environment because they don't exist in [JavaScript](JavaScript.md).

## Why TypeScript?

Before answer this question, let's look at this simple code example:

````js
const countries = [
  {
    name: "The Netherlands",
    flag: "🇳🇱",
    currency: "EUR",
    capital: "Amsterdam",
  },
  {
    name: "Germany",
    flag: "🇩🇪",
    currency: "EUR",
    capital: "Berlin",
  },
  {
    name: "The Czech Republic",
    flag: "🇨🇿",
    currency: "CZK",
    capital: "Prague",
  },
];

function getLabel(country) {
  return `${country.flag} ${country.name}, ${country.captal}, ${country.currency}`;
}

function print(str) {
  console.log(str + "\n");
}

function printCountries(countries) {
  countries.forEach((country) => {
    print(getLabel(country));
  });
}

printCountries(countries);
````

Very simple code, right? Did you see any mistakes? We just have list of objects each containing information about some country. The result of code execution is that information about every country will be *printed* in the terminal. Let's run this by [Node](Node.md).

That's what we will see in the terminal:

````
🇳🇱 The Netherlands, undefined, EUR

🇩🇪 Germany, undefined, EUR

🇨🇿 The Czech Republic, undefined,
````

Wait.. What? Surely, the result won't surprise you if you have a phenomenal attention span. But we are all human and we can make mistakes sometimes.

The error here is that we wrote name of the field that does not exist - **captal**:

````diff
function getLabel(country) {
  - return `${country.flag} ${country.name}, ${country.captal}, ${country.currency}`;
  + return `${country.flag} ${country.name}, ${country.capital}, ${country.currency}`;
}
````

And this is just a very simple synthetic example. What if we make mistake in a project which has hundreds of lines of code? Thousands?

You could say "but we found the error after all when we executed our code". Yes, we did. But this is just one file. If you have a large project you will waste a lot of time to find the error. [TypeScript](TypeScript.md) provides us an ability to find this kind of errors **before** executing the code.

{{< subscription >}}

Let's just write a few lines of code in an example with countries and prevent the error before executing the code:

````ts
type Currency = "EUR" | "CZK";

interface Country {
  name: string;
  flag: string;
  currency: Currency;
  capital: string;
}

const countries: Country[] = [
  {
    name: "The Netherlands",
    flag: "🇳🇱",
    currency: "EUR",
    capital: "Amsterdam",
  },
  {
    name: "Germany",
    flag: "🇩🇪",
    currency: "EUR",
    capital: "Berlin",
  },
  {
    name: "The Czech Republic",
    flag: "🇨🇿",
    currency: "CZK",
    capital: "Prague",
  },
];

function getLabel(country: Country) {
  return `${country.flag} ${country.name}, ${country.captal}, ${country.currency}`;
}

function print(str: string) {
  console.log(str + "\n");
}

function printCountries(countries: Country[]) {
  countries.forEach((country) => {
    print(getLabel(country));
  });
}

printCountries(countries);
````

The error is still in the code but I see it in my editor (VSCode):

![](/images/example-countries-ts.png)

We added a few new constructions that help us to find mistakes before running the code. The most important thing here is on line 3 - **interface**. Let's just say that it is something like an object that contains information about the types of each country object's fields. We will get to that later in the next posts of this series.

## TypeScript is already here

TypeScript doesn't become popular in [JavaScript](JavaScript.md) development ecosystem. It's already popular. There are many technologies that provide an ability to write code in one programming language and compile this code into JavaScript to execute in the browser. But they are less popular or don't have general purposes like [TypeScript](TypeScript.md).

There are many projects and libraries that are written in [TypeScript](TypeScript.md). In fact, you probably use one tool to write a code, which is written in TypeScript - [Visual Studio Code](Visual%20Studio%20Code.md). And even if you write JavaScript code, TypeScript is already using to inspect and analyze this code in Visual Studio Code.

Remember our code example with countries? Let's go back to it. We changed this code that was written in [JavaScript](JavaScript.md). Somehow, we made a mistake, some little typo in the code:

````js
const countries = [
  // the same countries as before
];

function getLabel(country) {
  return `${country.flag} ${country.name}, ${country.capital}, ${country.currency}`;
}

function print(str) {
  console.log(str + "\n");
}

function printCountries(countries) {
  countries.forEach((country) => {
    print(getLabel(country));
  });
}

printCountries(contries);
````

If you open this [JavaScript](JavaScript.md) code in [Visual Studio Code](Visual%20Studio%20Code.md) you won't see any errors. Okay, now let's add a special comment line on top of the file:

````diff
+ // @ts-check

const countries = [
  // the same countries as before
];

// .. the same code as before

printCountries(contries);
````

And now we see the error in JavaScript file that doesn't have any types or other constructions that are specific for [TypeScript](TypeScript.md):

![](/images/example-countries-ts-check.png)

## TypeScript Advantages

We've got an idea of what is TypeScript in general and why should we use it. Now, let's see that features and advantages [TypeScript](TypeScript.md) can provide to developers.

### Types

As I mention before, [TypeScript](TypeScript.md) adds [Static typing](static%20typing.md) to JavaScript code. It helps us avoid some errors and typos in the code. We also can use modern IDEs or editors that have features like autocompletion, refactoring, go to definition. Types and type definitions add support to analyze code in the IDE.

### Support modern JavaScript features

[TypeScript](TypeScript.md) supports ES6+ features of [JavaScript](JavaScript.md). It means that we can write modern JavaScript features in TypeScript code. We can compile that code into JavaScript code that will be executed by even an old version of some [Web browser](Web%20browser.md) that doesn't support modern JavaScript features.

### TypeScript-specific features

TypeScript also adds features that are specific to it. It is about Interfaces, Generics, Decorators, and others. That new constructions don't exist in JavaScript. I will write about it more in the next post of this series.

## To be continued

In this post, we learned that TypeScript is a Superset of JavaScript and how TypeScript can help us write more stable and safe code. This is an introduction post to TypeScript series on my blog. In the next post, we will discover how to configure our TypeScript compiler for the project and deep dive into the features that TypeScript brings to developers.

Be in touch! ✨
