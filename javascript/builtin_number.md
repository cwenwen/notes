# Built-in Number Methods

[Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number) and [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math)

* 字串轉換成數字
```js
Number(value)
```

* 字串轉換成整數
```js
parseInt(string, radix);
```

* 字串轉換成小數
```js
parseFloat(value)
```

* 數字轉換成字串
```js
numObj.toString([radix])
numObj + ''
```

* 小數點後取幾位（會四捨五入，不加參數即取整數）
```js
numObj.toFixed([digits])
```

## Math

* 圓周率（常數 constant）
```js
Math.PI
```

* 四捨五入取整數
```js
Math.round(x)
```

* 無條件進位、捨去
```js
Math.ceil(x)
Math.floor(x)
```

* 開根號
```js
Math.sqrt(x)
```

* 取 array 中的最大、最小值
```js
Math.max([value1[, value2[, ...]]])
Math.min([value1[, value2[, ...]]])

var array1 = [1, 3, 2];
console.log(Math.max(...array1)); // 展開 array1
// expected output: 3
```

* 取次方
```js
Math.pow(base, exponent)
```

* 取隨機數（包含 0，不含 1）
```js
Math.random()

function getRandomInt(max) {
  return Math.floor(Math.random() * Math.floor(max));
}
//The maximum is exclusive and the minimum is 0

function getRandomInt(min, max) {
  min = Math.ceil(min);
  max = Math.floor(max);
  return Math.floor(Math.random() * (max - min)) + min; 
  //The maximum is exclusive and the minimum is inclusive
}
```