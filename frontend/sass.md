# Sass Basics

* [程式導師實驗計畫第一期](https://github.com/Lidemy/mentor-program)：Lesson 9-1
* [Sass](https://sass-lang.com/)
* [PostCSS](https://postcss.org/)
* [PostCSS 介紹](http://huli.logdown.com/posts/262723-experiences-what-is-postcss)

***

### Install

用 npm 安裝：

```
npm install -g sass
```

後來要更新版本時，遇到這個錯誤：  
`npm WARN checkPermissions Missing write access to /usr/local/lib/node_modules/sass`  
`npm ERR! errno -13`  
改用這個指令即可：

```
sudo npm update -g
```

### Preprocessing

手動把 `input.scss` 轉換成 `output.css`：

```
sass input.scss output.css
```

若不想每次都手動轉換，  
可用以下指令讓 `input.scss` 每次一按存檔鍵，都自動轉換成 `output.css`：

```
sass --watch input.scss output.css
```

同上，  
若 `input.scss` 和 `output.css` 在不同資料夾，  
可用 `:` 區別不同資料夾：

```
sass --watch app/sass:public/stylesheets
```
Sass 會監視 `app/sass` 資料夾內所有改變，  
轉換到 `public/stylesheets` 資料夾。

### Variables: $

SCSS 的語法長得比較像 CSS，使用 `;` 和 `{}`，  
Sass 則省略這兩個，比較沒標點符號，語法也有些許不同。  
以下皆為 SCSS 語法。

宣告和呼叫變數使用 `$` 符號，如：

```scss
$font-stack:    Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```

### Nesting

撰寫 Sass 可以像 HTML 一樣擁有清楚的巢狀結構，  
但勿過度使用，以免難維護。  
範例：

```scss
nav {
  display: none;

  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { 
    display: inline-block; 
  }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```

### Referencing Parent Selectors: &

在巢狀結構底下，可以用 `&` 來取代上一層 parent 的元素，  
例如在 `a` 標籤底下提到 `a:hover` ，  
可寫成 `&:hover`。

```scss
#main {
  color: black;

  a {
    font-weight: bold;

    &:hover {
      color: red;
    }
  }
}
```

class 名稱很長時，  `&` 也很好用，  
例如 `.card__list--disabled` 這個名稱：

```scss
.card {
  &__list {
    &--disabled {
      display: none;
    }
  }
}
```

### Partials/Import

用底線為開頭命名的 SCSS 檔案如 `_partial.scss` ，  
不會被轉換成 CSS 檔案，  
  
它們是片段的程式碼，可用 `@import` 引入其他 SCSS 檔案中，  
便於模組化 CSS 程式碼。
  
假設有個檔案 `_reset.scss`：

```scss
// _reset.scss

html,
body,
ul,
ol {
  margin:  0;
  padding: 0;
}
```

在 `base.scss` 檔案中用 `@import` 引入（像是複製貼上）：

```scss
// base.scss

@import 'reset';

body {
  font: 100% Helvetica, sans-serif;
  background-color: #efefef;
}
```

### Mixins (they're like functions)

有些想重複利用的程式碼可用 `@mixin` 做成像是 function 的元件，  
使用時帶入想運算的值，用 `@include` 引入：

```scss
@mixin border-radius($radius) {
  -webkit-border-radius: $radius * 2;
     -moz-border-radius: $radius * 2;
      -ms-border-radius: $radius;
          border-radius: $radius;
}

.box { 
  @include border-radius(10px); 
}
```

經過轉換後，在 CSS 檔案中變這樣：

```css
.box { 
  -webkit-border-radius: 20px;
     -moz-border-radius: 20px;
      -ms-border-radius: 10px;
          border-radius: 10px;
}
```

### Extend/Inheritance

若沒有值需要帶入，不需要 function，但想重複使用同一組 properties，  
可使用 `%` 定義該 properties，需要時用 `@extend` 引入：

```scss
// This CSS won't print because %equal-heights is never extended.
%equal-heights {
  display: flex;
  flex-wrap: wrap;
}

// This CSS will print because %message-shared is extended.
%message-shared {
  border: 1px solid #ccc;
  padding: 10px;
  color: #333;
}

.message {
  @extend %message-shared;
}

.success {
  @extend %message-shared;
  border-color: green;
}

.error {
  @extend %message-shared;
  border-color: red;
}

.warning {
  @extend %message-shared;
  border-color: yellow;
}
```

以上 `.message`、`.success`、`.error`、`.warning` 都用了 `%message-shared` 定義的 properties，  
而 `%equal-heights` 沒用到，所以不會顯示。  
上面的程式碼轉換成 CSS 檔案後是這樣：

```css
.message, .success, .error, .warning {
  border: 1px solid #cccccc;
  padding: 10px;
  color: #333;
}

.success {
  border-color: green;
}

.error {
  border-color: red;
}

.warning {
  border-color: yellow;
}
```

### Comments: /* */ and //

使用 `/* */` 會出現在 CSS 檔案上，而 `//` 不會。

```scss
/* This comment is
 * several lines long.
 * since it uses the CSS comment syntax,
 * it will appear in the CSS output. */

body {
  color: black;
}

// These comments are only one line long each.
// They won't appear in the CSS output,
// since they use the single-line comment syntax.

a {
  color: green;
}
```