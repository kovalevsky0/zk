---
title: "Using \"table\" in method it.each of Jest"
public: true
tags:
  - testing
  - jest
  - javascript
date: 2021-04-14
---

# Using "table" in method it.each of Jest

[test.each\`table\`(name, fn, timeout)](https://jestjs.io/docs/api#2-testeachtablename-fn-timeout)

````js
  it.each`
    a    | b    | result
    ${1} | ${2} | ${3}
    ${0} | ${4} | ${4}
  `("should return $result when a = $a, b = $b", ({ a, b, result }) => {
    expect(sum(a, b)).toBe(result);
  });
````
