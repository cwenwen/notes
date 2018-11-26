# [React Crash Course 2018](https://youtu.be/Ke90Tje7VS0)

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).

## Session 1: getting started

```sh
npx create-react-app react-app
cd react-app
npm start
```

因為有引入 [Simple React Snippets](https://marketplace.visualstudio.com/items?itemName=burkeholland.simple-react-snippets) 這個 extension，可以用這些快速鍵：

```jsx
imrc
// import React, { Component } from "react";
```

```jsx
cc
/* 
class [輸入 component] extends Component {
  state = {  }
  render() { 
    return (  );
  }
}
 
export default [輸入 component];
*/
```

這邊用預設的格式，  
但 `export default` 也可以加在 class Counter 宣告那一行的開頭，就不用多這一行：

```jsx
export default Counter;
```

JSX 中，要寫 JavaScript 語法，要加上 `{}`。  
JSX 中，`<React.Fragment>` 可以取代 `<div>` 來當一個父元素，而不會被 render 出來。  
（因為 React 只能 render 單一的元素。）

在標籤裡加上 `style={this.styles}` 來使用以下 style：

```jsx
styles = {
  // 這裡要用駝峰式取代 CSS 中的 dash
  // 數字 10 會自動變成 10px
  fontSize: 10,
  fontWeight: "bold"
};
```

在 JSX 的標籤中直接寫 CSS，記得要兩個 `{}`，  
裡面那個 `{}` 代表 JavaScript object。  
像這樣：

```jsx
style={{ fontSize: 30 }}
```

Destructuring 解構，  
簡化 this.state.count：

```jsx
const { count } = this.state;
```

array 中每個元素都要加上 key，以避免這個 warning：  
`Warning: Each child in an array or iterator should have a unique "key" prop.`  
加上 key 後，在必須改變 DOM 時，React 會使用這個 key 找出那個被改變的元素。

```jsx
<ul>
  {this.state.tags.map(tag => (
    <li key="">{tag}</li>
  ))}
</ul>
```

想依照不同條件 render 不同東西時，  
把 `render()` 中的 `return` 改為自定的一個 function `{ this.renderTags() }`，  
在元素中宣告這個 function：

```jsx
renderTags() {
  // 狀況 1：
  if (this.state.tags.length === 0) return <p>There are no tags!</p>;
  // 狀況 2：
  return (
    <ul>
      {this.state.tags.map(tag => (
        <li key={tag}>{tag}</li>
      ))}
    </ul>
  );
}
```

另一個想依照條件 render 元素的方法，用 `&&`：  
(在 `this.state.tags.length === 0` 時，render `"Please create a new tag!"`)

```jsx
render() {
  return (
    <div>
      {this.state.tags.length === 0 && "Please create a new tag!"}
      {this.renderTags()}
    </div>
  );
}
```

要讓標籤的 attribute 如 `onClick={handleClick}` 的 `handleClick` 有作用，必須加上 constructor：

```jsx
constructor() {
  super(); // 有了這行才能繼承父元素的 this
  this.handleClick = this.handleClick.bind(this); // 使用 bind() 來讓 handleClick 可以用 this
}
```

或是用 arrow function 來宣告 `handleClick`，就不用 `bind()`  
（arrow function 也太讚了吧！）

```jsx
handleClick = () => {
  // 這裡面用 this 不會有問題，不用 bind()
};
```

改變 state 用 `setState()`：

```jsx
this.setState({ count: this.state.count + 1 });
```

Passing Event Arguments  
目前 `handleIncrement` 這個 method 沒有 take any parameter（就只有按了加 1），  
但真正的 app 中我們會需要 pass argumants with our events。
例如我們在購物網站，按 buttun 啟動 `handleIncrement` 時，我們想同時得到商品的 ID，知道是哪個商品加 1。  

方法 1，寫另一個 method 給 ID（程式碼較雜亂）：  
```jsx
// 新的 method
doHandleIncrement = () => {
  this.handleIncrement({ id: 1 }); // product 就是 { id: 1 }
};

// 改變 btn 的 onClick
<button onClick={this.doHandleIncrement}>Increment</button>
// 本來是 onClick={this.handleIncrement}

// handleIncrement 加入參數（product 也就是平常用的 e）
handleIncrement = product => {
  console.log(product) // { id: 1 }
  this.setState({ count: this.state.count + 1 });
};
```

方法 2，btn 直接加上 inline function（較好）：
```jsx
<button onClick={ () => this.handleIncrement({ id: 1 })}>Increment</button>
// 把新的 method 直接寫在 btn 裡，讓按鈕按下時，順便紀錄商品 ID
```

## Session 2: composing components

- Pass data
- Raise and handle events
- Multiple components in sync
- Functional components
- Lifecycle hooks

React 是由 components tree 組成的。  
要在一個 `<Counters />` 底下 render 多個 `<Counter />` 時，  
這樣比較笨：

```jsx
// Counters 元素內
render() { 
  return (
    <div>
      <Counter />
      <Counter />
      <Counter />
      <Counter />
    </div>
  );
}
```

這樣比較好：

```jsx
// <Counters /> 元素內
state = { 
  counters: [
    { id: 1, value: 0},
    { id: 2, value: 0},
    { id: 3, value: 0},
    { id: 4, value: 0}
  ]
}

render() { 
  return (
    <div>
      {this.state.counters.map(counter => <Counter key={counter.id} id={counter.id} />)}
    </div>
  );
}
```

`this.props` 是 component 的 attributes（不包含 `key`）：

```jsx
// <Counters /> 元素內
render() { 
  return (
    <div>
      {this.state.counters.map(counter =>
        <Counter key={counter.id} value={counter.value} selected={true} />)}
    </div>
  );
}
// <Counter /> 的 this.props 為 {value: 0, selected: true}
// `key` is not a prop
```

改變 `<Counter />` 的預設 value：

```jsx
// <Counter /> 元素內
state = {
  count: this.props.value
};
```

若要 render 的 component 不是 self-closing 如 `<Counter />`，而是有 children：

```jsx
<Counter key={counter.id} value={counter.value}>
  <h4>Counter #{counter.id}</h4>
</Counter>
```

則 `<Counter />` 的 props 會多一個 `children`，  
這例子中， `children` 是一個 React.element，`type: 'h4'`。  

但到這邊為止，這個 `<h4>` 還不會被 render 出來，  
要 render 的話，到 `<Counter />` render 的 return 中，加上 `{this.props.children}`：

```jsx
render() {
  return (
    <div>
      {this.props.children}
      <span className={this.getBadgeClasses()}>{this.formatValue()}</span>
      <button
        onClick={ () => this.handleIncrement}
        className="btn btn-secondary btn-sm"
      >
        Increment
      </button>
    </div>
  );
}
/* 
{this.props.children} 會 render 出 <h4> 元素，內容為：
Counter #1
Counter #2
Counter #3
Counter #4
*/
```

在以上例子中，不用到 `children` 也可以，  
直接給一個 attribute 叫 `id`，可達到一樣效果：

```jsx
// <Counters /> 元素內
render() { 
  return (
    <div>
      {this.state.counters.map(counter => <Counter key={counter.id} value={counter.value} id={counter.id} />)}
    </div>
  );
}

// <Counter /> 元素內
render() {
  return (
    <div>
      <h4>{this.props.id}</h4>
      <span className={this.getBadgeClasses()}>{this.formatValue()}</span>
      <button
        onClick={ () => this.handleIncrement}
        className="btn btn-secondary btn-sm"
      >
        Increment
      </button>
    </div>
  );
}
```

React Developer Tools (Chrome extension)  
安裝後打開 dev tool，會多一個 React 可以選，  
可以清楚看到每個 element 的 state、props、key 等等各種資訊，  
還可以 search 直接找到你要的 component。  
  
`== $r`：  
在 dev tool 的 React 那一欄點選了 `<Counter />`，後面會出現 `== $r`，  
在瀏覽器的 console 打 `$r`，會得到 `<Counter />` 的第一個 instance。  
用這個 `$r` 就可以在 DOM 上找到所有 instance of components of the page。  
在瀏覽器的 console 打 `$r.render()` 可以看到那個 component 會 render 的東西。  
  
### State
The state is local and internal to the component.  
The locally state is completely invisible to other components.  
State is used so that a component can keep track of information in between any renders that it does. When you `setState()` it updates the state object and then re-renders the component.  
Sometimes a component may not have a state. It may get all the data it needs via props.

### Props
Props is read-only. We cannot change the input to this component inside of this component.  
Props are how components talk to each other. Props flow downwards from the parent component. Components receive data from the parent.   
There is also the case that you can have default props so that props are set even if a parent component doesn’t pass props down.  
  
新增元素的快捷鍵：

```jsx
buttun.btn.btn-danger.btn-sm.m-2
// 按下 enter 後，自動生成：
// <button className="btn btn-danger btn-sm m-2"></button>
```

要改變 state 必須由擁有那個 state 的 component 來執行，像 `<Counter />` 就不能改變 `<Counters />` 的 state。  
該怎麼做？  

- Raise and handle events 

假設要用 `<Counter />` 中的 delete 按鈕來操作父元素 `<Counters />` 的 state，  
先把父元素 `<Counters />` 加一個 `handleDelete` 的 method（不是加在子元素），  
父元素 render 之中也加這個 prop `onDelete={this.handleDelete}`，  
子元素就可透過這個 prop 來用父元素的 `handleDelete`：

```jsx
// <Counters /> 元素內
render() { 
  return (
    <div>
      {this.state.counters.map(counter => 
      <Counter key={counter.id} onDelete={this.handleDelete} value={counter.value} id={counter.id} />)}
    </div>
  );
}

// <Counter /> 元素內
<button onClick={this.props.onDelete} className="btn btn-danger btn-sm m-2">Delete</button>
```

實作 delete 內容，首先要拿到欲刪除的 id：

```jsx
<button onClick={() => this.props.onDelete(this.props.id)} className="btn btn-danger btn-sm m-2">Delete</button>
```

 有了 id 就可以更新 state。做法是用 `filter()` 給一個新的 array，再 `setState()`：

```jsx
handleDelete = (counterId) => {
  const newCounters = this.state.counters.filter(item => item.id !== counterId);
  this.setState({ counters:  newCounters });
}

// key 跟 value 一樣，可簡化成：
handleDelete = (counterId) => {
  const counters = this.state.counters.filter(item => item.id !== counterId);
  this.setState({ counters });
}
```

簡化 props：

```jsx
// <Counters /> 元素內
render() { 
  return (
    <div>
      {this.state.counters.map(counter => (
        <Counter 
          key={counter.id} 
          onDelete={this.handleDelete} 
          value={counter.value} 
          id={counter.id} 
        />
      ))}
    </div>
  );
}
```

我們可以把 `value={counter.value}`、`id={counter.id}` 和所有 counter object 有關的 props，  
簡化成 `counter={counter}`：

```jsx
// <Counters /> 元素內
render() { 
  return (
    <div>
      {this.state.counters.map(counter => (
        <Counter 
          key={counter.id} 
          onDelete={this.handleDelete} 
          counter={counter}
        />
      ))}
    </div>
  );
}
```

`counter={counter}` 這個 prop 包含了所有我們需要的 counter props。  
之後記得改一下路徑，把用到 `this.props.id` 的地方改為 `this.props.counter.id`，  
用到 `this.props.value` 的地方改為 `this.props.counter.value`。  
  
添加 reset 按鈕，讓一切回到初始狀態，但以下程式碼不行（點了 reset btn 沒用）：  

```jsx
// <Counters /> 元素內
handleReset = () => {
  const counters = this.state.counters.map(item => {
    item.value = 0;
    return item;
  });
  this.setState({ counters });
}

render() { 
  return (
    <div>
      <button 
        onClick={this.handleReset} 
        className="btn btn-primary btn-sm m-2">
        Reset
      </button>
      {this.state.counters.map(counter => (
        <Counter 
          key={counter.id} 
          onDelete={this.handleDelete} 
          counter={counter}
        />
      ))}
    </div>
  );
}
```

為什麼？ 因為 local state 沒被更新（跟 single source of truth有關）。  
多看幾次[影片講解](https://www.youtube.com/watch?v=Ke90Tje7VS0&t=6475s)！

移除 local state：  
移除 `<Counter />` 裡的 state，並把跟 local state 相關的程式碼改寫：  

1. `this.state.value` 改成 `this.props.counter.value`  
2. 去除 `handleIncrement` method，按鈕上改成 `onClick={() => this.props.onIncrement(this.props.counter)}`

用父元素 `<Counters />` 直接給子元素資訊，和本來 local state 相關的操作（`handleIncrement`）都上升給父元素來做：

```jsx
// <Counters /> 元素內
handleIncrement = counter => {
  const counters = [...this.state.counters];  // 因為 object 和本來的都一樣，所以 clone 一個一樣的 array
  const index = counters.indexOf(counter);    // 找到「增加的元素」的 index
  counters[index] = { ...counter };           // 先 clone 一個和原本一樣的 counter object
  counters[index].value++;                    // 再增加 value
  this.setState({ counters });                // 最後 setState()
}
```

不是父子關係的 component 之間要怎麼同步？把 state 移到他們共同的父元素上。  
多看幾次[影片講解](https://www.youtube.com/watch?v=Ke90Tje7VS0&t=7239s)！  

假如加上一個 navbar，顯示 total number of counters（購物車有幾種商品的概念），  
就必須把 `<Counters />` 的 state 相關的所有東西移到父元素 `<App />`，  
才能也讓 `<NavBar />` 用：

```jsx
// <App /> 元素內，所有本來 <Counters /> 的 state 相關程式碼都移過來以後
// 在 render 中的 <Counters /> 加入 props
// <NavBar /> 不用 methods，所以加上一個 prop 來計算總數即可

render() {
  return (
    <React.Fragment>
      <NavBar />
      <main className='container'>
        <Counters 
          counters={this.state.counters} 
          onReset={this.handleReset} 
          onIncrement={this.handleIncrement} 
          onDelete={this.handleDelete}
        />
      </main>
    </React.Fragment>
  );
  }
```

`<NavBar />` 是 stateless functional component，其實不用給他一個 class，可以把他變成 function。

```jsx
const NavBar = props => {
  return ( 
    <nav className="navbar navbar-light bg-light">
      <a className="navbar-brand" href="#">
        Navbar 
        <span className="badge badge-pill badge-secondary">
          {props.totalCounters}
        </span>
      </a>
    </nav> 
  );
}
```

製作 stateless functional component 的快速鍵：  

```jsx
sfc
/* 
const [輸入 component] = (這裡要放 props 做為參數) => {
  return ( 
    所有的 this.props 都要去除 this，只留 props
   );
}
 
export default [輸入 component];
*/
```

Destructuring 解構，  
簡化 `this.props`：

```jsx
const { onReset, counters, onDelete, onIncrement } = this.props;

// 接下來都可以只用 onReset, counters, onDelete, onIncrement
// 不用 this.props
```

## Session 3: lifecycle hooks

### Mount 
1. constructor
2. render
3. componentDidMount

### Update
1. render
2. componentDidUpdate

### Unmount
1. componentWillUnmount
  
常用就以上幾種。  


## Mounting phase

發生在 component 放上 DOM 時（mount），  
從 constructor 開始跑：  

```jsx
constructor(props) {
  super(props);
  this.state = this.props.something;
}
```
- 在這階段可以直接指定 `this.state`，不用 setState()

記得放上 props 當參數。constructor 第一行一定是 `super(props)`。  
再來跑 render，所有的子元素也會 render 到 DOM 上。  
再來當 component 放上 DOM 後，會跑 `componentDidMount()`：  

```jsx
componentDidMount() {
  // Ajax call
  this.setState({ callback resp })
}
```

- 在這階段很適合用 Ajax 來向 server 拿資料
- 拿完後更新 state

Component mounted 就是元素在 DOM 上了。  

## Updating phase

發生在 component 的 state 或 props 更新時。  
例如按下 Increment 的按鈕後，所有相關的 component 都會被 render：

```jsx
// browser console
App - rendered
NavBar - rendered
Counters - rendered
(4) Counter - rendered
```

注意：被 render 不代表 DOM 有被更新，只有被改變的元素會更新 DOM。  
接著會跑 `componentDidUpdate()`：  

```jsx
// <Counter /> 元素內
componentDidUpdate(prevProps, prevState) {
  console.log('prevProps', prevProps);
  console.log('prevState', prevState);
  if (prevProps.counter.value !== this.props.value) {
  // Ajax call and get new data from the server
  }
}
// 在第一個 counter 按了 increment，全部 render 後會 console.log 舊的 props 和 state
// 第一個 counter 舊的 prop：  counter: {id: 1, value: 4}
// 第一個 counter 新的 prop：  counter: {id: 1, value: 5}
```

- 這階段會有新的 state 或 props，可以比較新的和舊的
- 如果有改變，可以在這階段發 Ajax 向 server 拿資料，沒改變就不用浪費 request 了。

## Unmounting phase

發生在 component 被移除 DOM 的前一刻。按下 Delete 的按鈕後：  

```jsx
// browser console
App - rendered
NavBar - rendered
Counters - rendered
(3) Counter - rendered  // counter 剩三個
Counter - unmount       // 把要 delete 的 counter 從 DOM 移除前，會執行 componentWillUnmount()
```

- 這階段適合做一些清理。
- 像是有設定 timer，或是各種 listener，隨著元素從 DOM 上移除，也可以把它們拿掉了。
