# [Redux](https://redux.js.org/)

## Why we need Redux?

When the app continues to increase in complexity, every new feature makes it more challenging to think about how our Views and Models interact. Redux is a predictable state container for JavaScript apps.  

**[You Might Not Need Redux](https://medium.com/@dan_abramov/you-might-not-need-redux-be46360cf367)**  
 An article written by Dan Abramov - the author of Redux

## What is Redux?

Redux makes it easy to manage the state of an application.  

Redux lets developers to
- Describe application state as plain objects and arrays
- Describe changes in the system as plain objects
- Describe the logic for handling changes as pure functions

## How does Redux deal with asynchronous actions such as making API calls?

Without middleware, Redux store only supports synchronous data flow.  

Asynchronous middleware like [redux-thunk](https://github.com/reduxjs/redux-thunk) or [redux-promise](https://github.com/redux-utilities/redux-promise) wraps the store's `dispatch()` method and allows you to dispatch something other than actions, for example, functions or Promises.  

Read more: [Async Actions](https://redux.js.org/advanced/asyncactions)