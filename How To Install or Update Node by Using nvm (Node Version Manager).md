---
slug: how-to-install-or-update-node-by-using-nvm
aliases:
  - /blog/article/how-to-install-or-update-node-by-using-nvm
date: "2021-05-09 21:00:00"
image: images/how-to-install-or-update-node-by-using-nvm.png
images: ['images/how-to-install-or-update-node-by-using-nvm.png']
imageCopyright: Lucky
imageCopyrightUrl: "https://www.pexels.com/sv-se/@lucky-3592812"
title: How To Install or Update Node by Using nvm (Node Version Manager)
tags:
  - javascript
  - node
keywords:
  - javascript
  - node
  - nvm
type: blog
public: true
---

## Intro

There are few ways to install [Node](Node.md) on your local machine. The most popular way is to install it following [official website instructions](https://nodejs.org/en). But if you use this way you will install just one specific (latest) version of Node. What if you need to install a specific version of Node? Or you need to upgrade from one version to another but only for a short while.

For that purpose, you can use a tool called **nvm** ([Node Version Manager](Node%20Version%20Manager.md)).

## Installation

Because I use **macOS** on my local machine these instructions are specific to that operating system. You can find instructions that are specific to an operating system that you use on [official documentation on GitHub](https://github.com/nvm-sh/nvm).

To install **nvm** on your local machine let's use this command:

````
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
````

Now let's type the command which tells us that nvm is installed and available on our local machine:

````
nvm --version
````

You should see the current version of [Node Version Manager](Node%20Version%20Manager.md) that is installed on your local machine.

### Shell Troubleshooting

If you use some shell as [Z shell](Z%20shell.md) or [Fish Shell](Fish%20Shell.md) you probably will have something like this in the Terminal:

````
fish: Unknown command: nvm
````

Currently, I use [Fish Shell](Fish%20Shell.md). If you use something else you should check [detail information about troubleshooting on GitHub](https://github.com/nvm-sh/nvm#troubleshooting-on-macos).

Unfortunately, **nvm doesn't support Fish**. However, there is a solution called [fish-nvm](fish-nvm.md). It is a wrapper for [Fish Shell](Fish%20Shell.md). You can install this by using [Fisher (Fish Shell Plugin Manager)](Fisher%20%28Fish%20Shell%20Plugin%20Manager%29.md):

````
fisher install FabioAntunes/fish-nvm edc/bass
````

Now, if you type `nvm --version` you should see a version of [Node Version Manager](Node%20Version%20Manager.md) that is installed on your local machine.

## Usage

### Node Installation

Now it's time to start using [Node Version Manager](Node%20Version%20Manager.md) to install [Node](Node.md) on the local machine.

{{< subscription >}}

To install **latest** version of Node you can use the command:

````
nvm install node
````

To see all versions of Node that are installed on your machine use the command:

````
nvm ls
````

It should print in the Terminal something like this:

````
->      v16.1.0
         system
default -> node (-> v16.1.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v16.1.0) (default)
stable -> 16.1 (-> v16.1.0) (default)

...
````

Pay attention to the symbol "->". It shows us which version of Node is current on the local machine. So, basically, when you type `node -v` you should see the same version as with "->" before (in that case this version is 16.1.0).

Now let's install another version of [Node](Node.md). Let's say, I want to use some older version of Node. For example, 14 version. To install it use the command:

````
nvm install 14
````

It should install Node v.14.16.1. Let's look at the list of installed Node versions (***nvm ls***):

````
->     v14.16.1
        v16.1.0
         system
default -> node (-> v16.1.0)
iojs -> N/A (default)
unstable -> N/A (default)
node -> stable (-> v16.1.0) (default)
stable -> 16.1 (-> v16.1.0) (default)

...
````

Now we have two versions: 14.16.1 and 16.1.0. The current is 14 (the symbol "->" before).

To **switch to** version 16 use this command:

````
nvm use 16
````

We should see the answer of nvm:

````
Now using node v16.1.0 (npm v7.11.2)
````

And if we use the command ***nvm ls*** again we will see that the current version is 16.1.0.

### Uninstall Node

To uninstall **latest** version of [Node](Node.md) use command:

````
nvm uninstall node
````

To uninstall **specific** version of Node (for example - 14) use command:

````
nvm uninstall 14
````

## Global npm packages

If you have some installed globally [npm](npm.md) packages you should notice one thing. When you installed this npm package on one version of [Node](Node.md) and then you switch to another version of Node, the installed npm package won't be available for you. It's because npm packages that were installed on different versions of Node located in different places.

An example. Now I am on version 16 of [Node](Node.md). I want to install [Prettier](Prettier.md) globally on my computer by [npm](npm.md):

````
npm i -g prettier
````

And then we switch to version 14:

````
nvm use 14
prettier --version
````

You will see something like this:

````
prettier: command not found
````

Don't worry! If you want to use that package on version 14 you just need to install it again.

[npm](npm.md) packages are located in specific folders for each version of [Node](Node.md) that is installed by **nvm**.

Packages are located here (on macOS):

````
~/.nvm/versions/node/<version>/lib/node_modules
````
