# The `this` Keyword

[Explanation from Mosh](https://youtu.be/gvicrj31JOM)  

`this` is refer to:
> The **object** that is executing the current function.

### 如果這個 function 只是普通的 function：`this` 指的是 global object

- `Window` in browser
- `Global` in node

```js
const example = {
  title: 'Hello',
  tags: ['a', 'b','c'],
  showTags() {
    this.tags.forEach(function(tag) {
      console.log(this.title, tag); // undefined 'a' ...
      console.log(this);  // the global object
    })
  }
}

// 上例中，如果想在非 method 的 function 中取得 example.title，其中一個解法：
const example = {
  title: 'Hello',
  tags: ['a', 'b','c'],
  showTags() {
    this.tags.forEach(function(tag) {
      console.log(this.title, tag); // Hello a ...
    }, this);  // 把 this 傳下去給這個 function
  }
}
```

### 如果這個 function 在 object 之中（object 的 method）：`this` 指的是 object 本身。  

```js
const goat = {
  dietType: 'herbivore',
  makeSound() {
    console.log('baaa');
  },
  diet() {
    console.log(this.dietType);
  }
};

goat.diet(); 
// Output: herbivore
```

#### Arrow Functions and `this`

Arrow function 中的 `this`，會綁定這個 arrow function，不會指向 object。  

Arrow functions inherently *bind*, or tie, an already defined `this` value to the function itself that is NOT the calling object. 

```js
'use strict';

var obj = {
  i: 10,
  b: () => console.log(this.i, this),
  c: function() {
    console.log(this.i, this);
  }
}

obj.b(); // prints undefined, Window {...} (or the global object)
obj.c(); // prints 10, Object {...}
```

### 如果在 constructor 裡：在以此 constructor 製造的新 object 中，`this` 指的是這個新 object

```js
function Video(title) {
  this.title = title;
}

const v = new Video('The new video');
console.log(v.title);  // 'The new video'
```

在 constructor 中的普通 function，如果用 arrow function 的形式出現，則 `this` 會綁定 object。

```js
// 在 React component 之中

handleClick = () => {
  // 這裡面用 this 指向的就是這個 component，不用 bind()
};
```
