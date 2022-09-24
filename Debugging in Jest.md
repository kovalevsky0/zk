---
title: Debugging in Jest
public: true
tags:
  - testing
  - jest
  - javascript
date: 2021-04-14
---

# Debugging in Jest

Add breakpoint in test:

````js
  it.each`
    a    | b    | result
    ${1} | ${2} | ${3}
    ${0} | ${4} | ${4}
  `("should return $result when a = $a, b = $b", ({ a, b, result }) => {
    debugger; // breakpoint
    expect(sum(a, b)).toBe(result);
  });
````

Run this following command:

````
node --inspect-brk ./node_modules/.bin/jest ./<file_of_test>.test.js
````
