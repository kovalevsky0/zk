---
title: "Don't use arrow function inside mockImplementation with this (Jest)"
public: true
tags:
  - testing
  - jest
  - javascript
date: 2021-04-15
---

# Don't use arrow function inside mockImplementation with this (Jest)

When using the **arrow function**:

````js
// ...
      someAPI.JWT = jest.fn().mockImplementation(() => {
        this.authorize = jest.fn();
        this.credentials = { access_token: "fake_access_token" };
      });
// ...  
const jwtInstance = someAPI.JWT.mock.instances[0];
console.log(jwtInstance); // mockConstructor
````

When using **function**:

````js
// ...
      someAPI.JWT = jest.fn().mockImplementation(function () {
        this.authorize = jest.fn();
        this.credentials = { access_token: "fake_access_token" };
      });
// ...
const jwtInstance = someAPI.JWT.mock.instances[0];
console.log(jwtInstance);
/*
    mockConstructor {
      authorize: [Function: mockConstructor] {
        _isMockFunction: true,
        getMockImplementation: [Function (anonymous)],
        mock: [Getter/Setter],
        mockClear: [Function (anonymous)],
        mockReset: [Function (anonymous)],
        mockRestore: [Function (anonymous)],
        mockReturnValueOnce: [Function (anonymous)],
        mockResolvedValueOnce: [Function (anonymous)],
        mockRejectedValueOnce: [Function (anonymous)],
        mockReturnValue: [Function (anonymous)],
        mockResolvedValue: [Function (anonymous)],
        mockRejectedValue: [Function (anonymous)],
        mockImplementationOnce: [Function (anonymous)],
        mockImplementation: [Function (anonymous)],
        mockReturnThis: [Function (anonymous)],
        mockName: [Function (anonymous)],
        getMockName: [Function (anonymous)]
      },
      credentials: { access_token: 'fake_access_token' }
    }
*/
````
