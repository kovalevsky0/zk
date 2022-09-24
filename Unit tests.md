---
title: Unit tests
aliases:
  - Unit testing
tags:
  - testing
public: true
date: 2021-04-06
---

# Unit tests

* Unit tests are tests of modularity
* They test specific part/piece of the system (called **module**) independently from other modules of the system
* For example, test of module (component in Front End) that shows red error text:
  * module has input data (error status - existing or not)
  * output data" is
    * null (if error does not exist)
    * red text (if error exists)
  * unit test should check that module/component returns specific output data based on input data
  * in the system this module get input data (error status) from other module
    * we don't care about that because unit test checks module independently from other modules
  * for checking interaction between several modules we should use [Integration tests](Integration%20tests.md)
* Unit tests are documenting behavior of specific module

### What is unit?

* Basically, it's module
  * In languages like [JavaScript](JavaScript.md) modules are mostly files
    * But if file contains few classes then
      * classes are modules
      * file isn't module
* An example in [React](React.md) development
  * React app consists of components
  * Each component could contain:
    * state
    * methods (in class component) or functions (in functional component)
  * It doesn't make sense to test each function/method of the component because:
    * a function/method doesn't make sense outside of the component
    * a function/method *most likely* updates component's **state**
  * A **unit** here is the **component** itself
* Unit is a stateless function
  * It could be classic helper function
