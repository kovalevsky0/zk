---
title: ES Modules
tags:
  - javascript
  - typescript
  - ecmascript
public: true
date: 2021-05-16
---

# ES Modules

Module system in ECMAScript standard [JavaScript](JavaScript.md). 

An example:

````js
import { foo } from 'first-module';
import * as all from 'second-module';

export const SPECIAL_CHAR = "*";

export function anotherFoo() {
	// ...
}
````

### Links

* [Node.js Docs](https://nodejs.org/api/esm.html)
* [ES modules: A cartoon deep-dive](https://hacks.mozilla.org/2018/03/es-modules-a-cartoon-deep-dive)
