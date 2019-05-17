# What’s new in JavaScript (Google I/O ’19)

![](https://i.imgur.com/q0AzGu4.jpg)
[完整影片](https://youtu.be/c0oy0vQKEZE)

TC39 is the committee that governs the evolution of ECMAScript, which is the standard behind the JavaScript language.

> TC39 means Technical Committee number 39. It’s part of ECMA, the institution which standardizes the JavaScript language under the “ECMAScript” specification.

WebAssembly is also implemented in V8.

> WebAssembly 或稱 wasm 是一個實驗性的低階程式語言，應用於瀏覽器內的客戶端。WebAssembly 是可攜式的抽象語法樹，被設計來提供比 JavaScript 更快速的編譯及執行。WebAssembly 將讓開發者能運用自己熟悉的程式語言（最初以 C/C++ 作為實作目標）編譯，再藉虛擬機器引擎在瀏覽器內執行。WebAssembly 的開發團隊分別來自 Mozilla、Google、Microsoft、Apple，代表著四大網路瀏覽器 Firefox、Chrome、Microsoft Edge、Safari。2017 年 11 月，以上四個瀏覽器都開始實驗性的支援 WebAssembly。

Code for benchmarking purposes dose not reflect real-world usage.

### Lead real-world performance for modern JavaScript & WebAssembly, and enable developers to build a faster future web.

Performance

- parse speed
- runtime performance
- memory consumption

These are fully supported across modern browsers and Node.js:

- [async {it,gen}erators](https://javascript.info/async-iterators-generators)
- Promise
- [optional catch binding](https://github.com/tc39/proposal-optional-catch-binding)
- String#trim{[Start](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimStart),[End](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/trimEnd)}
- [object rest & spread](https://developers.google.com/web/updates/2017/06/object-rest-spread)

These features have received updates since last year:

## Class fields

- 支援：Chrome, Node.js

evolving class syntax

舊的 class：

```js
class IncreasingCounter {
  constructor() {
    this._count = 0;
  }

  get value() {
    console.log("Getting the current value!");
    return this._count;
  }

  increment() {
    this._count++;
  }
}

// 創造一個 instance：
const counter = new IncreasingCounter();

counter.value;
// logs 'Getting the current value!'
// 0

counter.increment();
counter.value;
// logs 'Getting the current value!'
// 1
```

The class installs the `value` getter and an `increment` method on the prototype.

The class has a constructor that creates an instance property `_count` and set its default value to 0.

We currently use the underscope prefix to denote that `_count` should not be used directly by the consumers of the class, but that's just a convention - the privacy is not enforced by the language.

新的 class fields：

```js
class IncreasingCounter {
  #count = 0;
  // 不需要 constructor 了
  // private fields 可用 # 符號來表示

  get value() {
    console.log("Getting the current value!");
    return this.#count;
  }

  increment() {
    this.#count++;
  }
}
```

The private fields syntax is similar to public fields, except you mark the fields as being private by using the `#` symbol.

`#`: part of the field name.

Private fields 無法在 class body 之外被讀取：

```js
const counter = new IncreasingCounter();

counter.#count;
// SyntaxError
//
counter.#count = 42;
// SyntaxError
```

Class field syntax 在處理 subclasses 時特別方便，例如這裡有一個 `Animal` 的 class 和他的 subclass `Cat`：

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  constructor(name) {
    super(name);
    this.likesBaths = false;
  }

  meow() {
    console.log("Meow!");
  }
}
```

整個 constructor 都不需要了，包括奇怪的 `super` call。

```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Cat extends Animal {
  likesBaths = false;
  // 只需要一行

  meow() {
    console.log("Meow!");
  }
}
```

之後會有 private methods, private getters and setters。

## String#matchAll

- 支援：Chrome, Firefox, Node.js

在處理 regular expressions 時很方便。  
假如用 String 的 method `match`：

```js
const string = "Magic hex numbers: DEADBEEF CAFE";
const regex = /\b\p{ASCII_Hex_Digit}+\b/gu;

for (const match of string.match(regex)) {
  console.log(match);
}
// Output:
//
// 'DEADBEFF'
// 'CAFE'
//
// 只會給你 match 的 substrings
```

假設用 `matchAll`：

```js
const string = "Magic hex numbers: DEADBEEF CAFE";
const regex = /\b\p{ASCII_Hex_Digit}+\b/gu;

for (const match of string.matchAll(regex)) {
  console.log(match);
}
// Output:
//
// ['DEADBEFF', index: 19, input: 'Magic hex numbers: DEADBEEF CAFE']
// ['CAFE', index: 28, input: 'Magic hex numbers: DEADBEEF CAFE']
```

This is especially useful for regular expressions with capture groups.  
寫一個簡單的 for-of loop 就能得到全部的資訊。

```js
const string = "Favorite GitHub repos: tc39/ecma262 v8/v8.dev";
const regex = /\b(?<owner>[a-z0-9]+)\/(?<repo>[a-z0-9\.]+)\b/g;

for (const match of string.matchAll(regex)) {
  console.log(`${match[0]} at ${match.index} with '${match.input}'`);
  console.log(`- owner: ${match.groups.owner}`);
  console.log(`- repo: ${match.groups.repo}`);
}
// Output:
//
// tc39/ecma262 at 23 with 'Favorite GitHub repos: tc39/ecma262 v8/v8.dev'
// - owner: tc39
// - repo: ecma262
// v8/v8.dev at 36 with 'Favorite GitHub repos: tc39/ecma262 v8/v8.dev'
// - owner: v8
// - repo: v8.dev
```

## Improved numeric literals

- 支援：Chrome 75

improving readability & maintainability

Using underscores as separators in numeric literals.  
數字用底線隔開。

```
1000000000000
1019436871.42

// 現在可以寫成這樣：
1_000_000_000_000
1_019_436_871.42
```

## BigInt

- 支援：Chrome, Firefox Nightly, Node.js，Safari 也快了
- There's now a transpiler and a polyfill story available for BigInts - JSBI

```js
1234567890123456789 * 123;
// 151851850485185200000
// 錯誤的結果

1234567890123456789n * 123n;
// 151851850485185185047n
// 正確的結果

1234567890123456789n.toLocaleString("en");
// '12,345,678,901,234,567,890'
1234567890123456789n.toLocaleString("de");
// '12.345.678.901.234.567.890'

const nf = new Intl.NumberFormat("fr");
nf.format(1234567890123456789n);
// '12 345 678 901 234 567 890'
// 新的 Intl.NumberFormat API
// 以上這些大數也都可用底線隔開
```

## Array\${flat, flatMap}

- 支援：Chrome, Firefox, Safari, Node.js

```js
// Flatten one level
const array = [1, [2, [3]]];
array.flat();
// [1, 2, [3]]

// Flatten 2 level
array.flat(2);
// [1, 2, 3]

// Flatten recursively until the array contains no more nested arrays:
array.flat(Infinity);
// [1, 2, 3]
```

flatMap 示範：

```js
const duplicate = x => [x, x];

[2, 3, 4].map(duplicate);
// [[2, 2], [3, 3], [4, 4]]

[2, 3, 4].map(duplicate).flat();
// [2, 2, 3, 3, 4, 4]
// 🐌

[2, 3, 4].flatMap(duplicate);
// [2, 2, 3, 3, 4, 4]
// 🚀
```

## Object.fromEntries

- 支援：Chrome, Firefox, Safari, Node.js

filling the API gap

```js
const object = { x: 42, y: 50 };
const entries = Object.entries(object);
// [['x', 42], ['y', 50]]
// 把 object 裡面的每個 key-value pair 變成 array，
// 第一個 element 是 key 第二個是 value

for (const [key, value] of entries) {
  console.log(`The value of ${key} is ${value}.`);
}
// Logs:
// The value of x is 42.
// The value of y is 50.
```

新的 API 提供從 entries 回去 object 的簡單方法：

```js
const object = { x: 42, y: 50 };
const entries = Object.entries(object);
// [['x', 42], ['y', 50]]

const result = Object.fromEntries(entries);
// { x: 42, y: 50 }

const object = { x: 42, y: 50, abc: 9001 };
const result = Object.fromEntries(
  Object.entries(object)
    .filter(([key, value]) => key.length === 1)
    .map(([key, value]) => [key, value * 2])
);
// { x: 84, y: 100 }
```

在 API 格式轉換時很好用。

```js
const object = { language: "JavaScript", coolness: 9001 };

// Convert the object into a map:
const map = new Map(Object.entries(object));

// Convert the map back into an object:
const objectCopy = Object.fromEntries(map);
// { language: 'JavaScript', coolness: 9001 }
```

## GlobalThis

- 支援：Chrome, Firefox, Safari, Node.js

輕鬆取得 globalThis：

```js
const theGlobalThis = globalThis;
```

## Stable sort

- 支援：Chrome, Firefox, Safari, Node.js

consistent & reliable results

```js
// Note how the array is pro-sorted alphabetically by `name`
const doggos = [
  { name: "Abby", rating: 12 },
  { name: "Bandit", rating: 13 },
  { name: "Choco", rating: 14 },
  { name: "Daisy", rating: 12 },
  { name: "Elmo", rating: 12 },
  { name: "Falco", rating: 13 },
  { name: "Ghost", rating: 14 }
];

doggos.sort((a, b) => b.rating - a.rating);

// 現在 sort 的結果更穩定了，相同 rating 者會按照字母排列
[
  { name: "Choco", rating: 14 },
  { name: "Ghost", rating: 14 },
  { name: "Bandit", rating: 13 },
  { name: "Falco", rating: 13 },
  { name: "Abby", rating: 12 },
  { name: "Daisy", rating: 12 },
  { name: "Elmo", rating: 12 }
];
```

## Intl

internationalization

### Intl.RelativeTimeFormat

- 支援：Chrome, Firefox, Node.js

locale-aware relative time formatting

```js
const rtf = new Intl.RelativeTimeFormat("en", { numeric: "auto" });

rtf.format(-1, "day");
// 'yesterday'
rtf.format(0, "day");
// 'today'
rtf.format(1, "day");
// 'tomorrow'
rtf.format(-1, "week");
// 'last week'
rtf.format(0, "week");
// 'this week'
rtf.format(1, "week");
// 'next week'
```

### Intl.ListFormat

- 支援：Chrome, Node.js

locale-aware list formatting

```js
const lfEnglish = new Intl.ListFormat("en");

lfEnglish.format(["Ada", "Grace", "Ida"]);
// 'Ada, Grace, and Ida'

const lfHindi = new Intl.ListFormat("hi-in");

lfHindi.format(["आनया", "आद्रिका", "अक्षत"]);
// "आनया, आद्रिका, और अक्षत"

// MDN 上的示範：
const vehicles = ["Motorcycle", "Bus", "Car"];

const formatter = new Intl.ListFormat("en", {
  style: "long",
  type: "conjunction"
});
console.log(formatter.format(vehicles));
// expected output: "Motorcycle, Bus, and Car"

const formatter2 = new Intl.ListFormat("de", {
  style: "short",
  type: "disjunction"
});
console.log(formatter2.format(vehicles));
// expected output: "Motorcycle, Bus oder Car"

const formatter3 = new Intl.ListFormat("en", { style: "narrow", type: "unit" });
console.log(formatter3.format(vehicles));
// expected output: "Motorcycle Bus Car"
```

### Intl.DateTimeFormat#formatRange

- 支援：behind a flag in Chrome，即將 ship it

locale-aware time span formatting

```js
// 現有的 API
const start = new Date(startTimestamp);
// 'May 7, 2019'
const endn = new Date(endTimestamp);
// 'May 9, 2019'
const fmt = new Intl.DateTimeFormat("en", {
  year: "nuremic",
  month: "long",
  dat: "numeric"
});
// 若想得到一個 range
// 'May 7, 2019 - May 9, 2019'
const output = `${fmt.format(start)} - ${fmt.format(end)}`;

// 新的 formatRange
const output = fmt.formatRange(start, end);
```

### Intl.Locale

- 支援：Chrome, Node.js

internationalization

```js
const locale = new Intl.Locale("es-419-u-hc-h12", {
  calender: "gregory"
});
locale.language;
// 'es'
locale.calender;
// 'gregory'
locale.hourCycle;
// 'h12'
locale.region;
// '419'
locale.toString();
// 'es-419-u-ca-gregory-hc-h12'
```

## top-level await

- 還沒支援

以前 `await` 只能和 async 一起出現。

```js
async function main() {
  const result = await doSomethingAsync();
  doSomethingElse();
}

main();

// 或者這樣
(async function() {
  const result = await doSomethingAsync();
  doSomethingElse();
})();
```

新的提案：

```js
const result = await doSomethingAsync();
doSomethingElse();
```

## Promise combinators

- Promise.any 還沒支援
- Promise.allSettled 支援：Chrome 76, Firefox Nightly

filling more API gaps

| Promise              | Use case                                        |
| -------------------- | ----------------------------------------------- |
| `Promise.allSettled` | does not short-circuits                         |
| `Promise.all`        | short-circuits when an input value is rejected  |
| `Promise.race`       | short-circuits when an input value is settled   |
| `Promise.any`        | short-circuits when an input value is fulfilled |

```js
// Promise.all
const promises = [
  fetch("/component-a.css"),
  fetch("/component-b.css"),
  fetch("/component-c.css")
];

try {
  const styleResponse = await Promise.all(promise);
  enableStyle(styleResponse);
  renderNewUi();
} catch (reason) {
  displayError(reason);
}

// Promise.race
try {
  const result = await Promise.race([
    performHeavyComputation(),
    rejectAfterTimeout(2000)
  ]);
  renderResult(result);
} catch (error) {
  renderError(error);
}

// Promise.allSettled
const promises = [
  fetch("/api-call-1"),
  fetch("/api-call-2"),
  fetch("/api-call-3")
];
// Imagine some of these request fail, and some succeed.

await Promise.allSettled(promise);
// All API calls have finished (either failed or succeed).
removeLoadingIndicator();
// 使用時機：
// 當你不在乎 promise 被 fulfilled 或 rejected，
// 只想知道何時做完。

// Promise.any
const promises = [
  fetch("/endpoint-a").then(() => "a"),
  fetch("/endpoint-b").then(() => "b"),
  fetch("/endpoint-c").then(() => "c")
];
try {
  const first = await Promise.any(promise);
  // Any of the promises was fulfilled.
  console.log(first);
  // e.g. 'b'
} catch (error) {
  // All of the promises were rejected.
  console.log(error);
}
```

## WeakRef

- 還在通過標準化的流程

As long as you have a reference to an object, that object won't be garbage-collected.  
Currently WeakMaps and WeakSets are the only way to weakly reference an object. Adding an object to a WeakMap or a WeakSet does not prevent it from being garbage-collected.
Weak references are a more advanced API that provide a window into the lifetime of an object.

```js
const cache = new Map();

function getImage(name) {
  if (cache.has(name)) return cache.get(name);

  const image = performExpensiveOperation(name);
  cache.set(name, image);
  return image;
}
```

But there's a problem: maps hold on to their keys and values strongly, so the image names and data can never be garbage-collected. So this steadily increase memory and causes a memory leak.

WeakMap 不支援 strings as keys 所以幫不上忙。

```js
const cache = new Map();

function getImage(name) {
  let ref = cache.get(name);
  if (ref !== undefined) {
    const deref = ref.deref();
    if (deref !== undefined) return deref;
  }

  const image = performExpensiveOperation(name);
  ref = new WeakRef(image);
  cache.set(name, ref);
  return image;
}
```

把 weak reference 存進 cache，而不是存 image itself。  
當 garbage collector 發現記憶體不足時，可以清掉一些 images。

But there's still a problem here: the map still holds on to the name strings forever, because those are the keys in the cache.  
最理想是這些 name strings 也一起被清掉：

```js
const cache = new Map();

const finalizationGroup = new FinalizationGroup(itertor => {
  for (const name of iterator) {
    const ref = cache.get(name);
    if (ref !== undefined && red.deref() === undefined) {
      cache.delete(name);
    }
  }
});
```

最終 implementation 像這樣：

```js
const cache = new Map();

function getImage(name) {
  let ref = cache.get(name);
  if (ref !== undefined) {
    const deref = ref.deref();
    if (deref !== undefined) return deref;
  }

  const image = performExpensiveOperation(name);
  ref = new WeakRef(image);
  cache.set(name, ref);
  finalizationGroup.register(image, name);
  return image;
}
```
