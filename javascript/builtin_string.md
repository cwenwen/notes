# Javascript Built-in Methods

[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)

* 大小寫轉換
```js
str.toUpperCase()
str.toLowerCase()
```

* 回傳 UTF-16 編碼的數字
```js
str.charCodeAt(index)

'ABC'.charCodeAt(2); // returns 67

// uppercase A: 65
// uppercase Z: 90
// lowercase a: 97
// lowercase z: 122
```
* 把 charCode 變回它代表的字
```js
String.fromCharCode(num1[, ...[, numN]])

String.fromCharCode(65, 66, 67);  // returns "ABC"
```

* 尋找某字是否存在字串中  
  若存在，回傳第一個字元的 index；若不存在，回傳 -1。
```js
str.indexOf(searchValue[, fromIndex])

'Blue Whale'.indexOf('Whale'); // returns  5
'Blue Whale'.indexOf('Whale', 7); // returns -1
```

* 取代字串  
  用字串只會換掉第一個吻合的字串，要換所有相符者用 [RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp)。
```js
str.replace(regexp|substr, newSubstr|function)

var p = 'The quick brown fox jumped over the lazy dog. If the dog reacted, was it really lazy?';

var regex = /dog/gi;
// g: global match; find all matches rather than stopping after the first match

console.log(p.replace(regex, 'ferret'));
// expected output: "The quick brown fox jumped over the lazy ferret. If the ferret reacted, was it really lazy?"
```

* 把字串切成陣列
```js
str.split([separator[, limit]])
```

* 重複字串
```js
str.repeat(count);
```
