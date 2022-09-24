---
title: Monorepo
date: "2021-07-09 01:23"
tags:
  - git
  - monorepo
public: true
---

# Monorepo

* Monorepo is a way or strategy of managing a large project that consists of many packages or modules in one repository
* You have one repository with bunch of packages/modules instead of system "one package = one repository"
* An example of the project that is monorepo - [Babel](Babel.md)
  * If you open [Babel on GitHub](https://github.com/babel/babel) you will see a folder called **packages** that contains all packages of Babel project
    * In not monorepo system each of these packages would be in a separate repository
* Pros:
  * You can provide cross-changes in a several packages faster and easily
    * It is possible to create a one pull/merge request with changes in a several packages
  * Managing different versions of packages and dependencies
  * Deploying all packages at the same time
  * Using the same build system for many packages
* Cons:
  * Most of the tools for managing monorepo are hard to implementing or they bring a lot of unnecessary abstractions

### Technologies and tools

* [Yarn](Yarn.md) Workspaces
* [Lerna.js](Lerna.js.md)
* [Rush.js](Rush.js.md)
