# Async / Await [by Codecademy](https://www.codecademy.com/learn/introduction-to-javascript)

Originally, JavaScript used callback functions to handle asynchronous actions. The problem with callbacks is that they encourage complexly nested code which quickly becomes difficult to read, debug, and scale. With ES6, JavaScript integrated native [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) which allow us to write significantly more readable code. JavaScript is continually improving, and ES8 provides a new syntax for handling our asynchronous action, `async...await`. The `async...await` syntax allows us to write asynchronous code that reads similarly to traditional synchronous, imperative programs.  

The `async...await` syntax is syntactic sugar — it doesn't introduce new functionality into the language, but rather introduces a new syntax for using promises and generators. Both of these were already built in to the language.  

```js
async function readFiles() {
  let firstSentence = await promisifiedReadfile('./file.txt', 'utf-8');
  let secondSentence = await promisifiedReadfile('./file2.txt', 'utf-8');
  console.log(firstSentence, secondSentence);
}
readFiles();


// We can also create async function expressions:
const myFunc = async () => {
  // Function body here
};

myFunc();
```

## The await Operator

The `await` keyword can only be used inside an `async` function. `await` is an operator: it returns the resolved value of a promise. Since promises resolve in an indeterminate amount of time, `await` halts, or pauses, the execution of our `async` function until a given promise is resolved.

We've seen that the `await` keyword halts the execution of an `async` function until a promise is no longer pending. Don't forget the `await` keyword! It may seem obvious, but this can be a tricky mistake to catch because our function will still run — it just won't have the desired results: 

```js
let myPromise = () => {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve('Yay, I resolved!')
    }, 1000);
  });
}

async function noAwait() {
 let value = myPromise();
 console.log(value);
}

async function yesAwait() {
 let value = await myPromise();
 console.log(value);
}

noAwait(); // Prints: Promise { <pending> }
yesAwait(); // Prints: Yay, I resolved!
```

Without the `await` keyword, the function execution wasn't paused. The `console.log()` on the following line was executed before the promise had resolved.  

## Handling Errors

With `async...await`, we use `try...catch` statements for error handling. By using this syntax, not only are we able to handle errors in the same way we do with synchronous code, but we can also catch both synchronous and asynchronous errors. This makes for easier debugging!

```js
async function usingTryCatch() {
 try {
   let resolveValue = await asyncFunction('thing that will fail');
   let secondValue = await secondAsyncFunction(resolveValue);
 } catch (err) {
   // Catches any errors in the try block
   console.log(err);
 }
}

usingTryCatch();
```