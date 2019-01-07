# Built-in Object Methods

[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)  

- 用 array 回傳 object 的 keys
```js
Object.keys(obj)
```

- 用 array 回傳 object 的內容，形式：`[key, value]`
```js
Object.entries(obj)
```

- `Object.assign()`：用已有的 object 資訊建立一個新 object，可加入新的 keys and values

```js
const obj = {
  a: 1,
  b: 2,
  c: 3
};

const newObj = Object.assign({c: 4, d: 5}, obj);
console.log(object2.c, object2.d);
// 3 5
```

- 定義 object 的 property

```js
// 定義一個
Object.defineProperty(obj, prop, descriptor)

const obj = {};
Object.defineProperty(obj, 'property1', {
  value: 42,
  writable: false
});

obj.property1 = 77;  // throws an error in strict mode
console.log(obj.property1);  // 42


// 定義多個
Object.defineProperties(obj, {
  property1: {
    value: 42,
    writable: true
  },
  property2: {
    value: 42,
    writable: false
  }
});
```
