# Javascript Built-in Methods

[Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)

* 陣列轉成字串  
  若沒放參數，則用 `,` 串成一個字串。
```js
arr.join([separator])
```

* `.map()` 對舊陣列中每一個元素做指定的事，創造新陣列  
  原陣列不變。
```js
var new_array = arr.map(function callback(currentValue[, index[, array]]) {
    // Return element for new_array
}[, thisArg])

var numbers = [1, 2, 3];
var bigNumbers = numbers.map(number => number * 10);
// bigNumbers is now [10, 20, 30]
// numbers is still [1, 2, 3]
```

* `.filter()` 過濾舊陣列，使通過指定條件者成為新陣列  
  原陣列不變。
```js
var newArray = arr.filter(callback(element[, index[, array]])[, thisArg])

var words = ['spray', 'limit', 'elite', 'exuberant', 'destruction', 'present'];

const result = words.filter(word => word.length > 6);

console.log(result);
// expected output: Array ["exuberant", "destruction", "present"]
```
* `.forEach()` 把陣列中每個元素都跑一遍
```js
arr.forEach(function callback(currentValue[, index[, array]]) {
    //your iterator
}[, thisArg]);

var groceries = ['brown sugar', 'salt', 'ginger'];
groceries.forEach(groceryItem => console.log('- ' + groceryItem));
// expected output:
// - brown sugar
// - salt
// - ginger
```
* 陣列切片  
  原陣列不變。
```js
arr.slice([begin[, end]]) // 不包含 end
```

* 刪除或新增元素到陣列中指定位置  
  會改變陣列。  
  [用法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
```js
array.splice(start[, deleteCount[, item1[, item2[, ...]]]])
```
* 排序陣列內元素
```js
arr.sort([compareFunction]) // 按照字串大小比較

// 若要按照數字大小比較，則：
var numbers = [4, 2, 5, 1, 3];

numbers.sort(function(a, b) {
  return a - b; //由小到大。大到小就改成 b - a
});

console.log(numbers);
// [1, 2, 3, 4, 5]
```

* `.reduce()` 累積器
```js
arr.reduce(callback[, initialValue])

const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```