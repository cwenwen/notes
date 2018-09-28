# [Tutorial: Intro to React](https://reactjs.org/tutorial/tutorial.html)

This project was bootstrapped with [Create React App](https://github.com/facebookincubator/create-react-app).  

要更新 React 的 state 時，永遠不直接操作現有的 state。  
我們要 const 一個新的 state，做好變動後，再把新的 state 用 `setState()` 來放上去。官方給的[原因](https://reactjs.org/tutorial/tutorial.html#why-immutability-is-important)：  

1. Complex Features Become Simple
  Immutability makes complex features much easier to implement. Later in this tutorial, we will implement a “time travel” feature that allows us to review the tic-tac-toe game’s history and “jump back” to previous moves. This functionality isn’t specific to games — an ability to undo and redo certain actions is a common requirement in applications. Avoiding direct data mutation lets us keep previous versions of the game’s history intact, and reuse them later.

2. Detecting Changes
  Detecting changes in mutable objects is difficult because they are modified directly. This detection requires the mutable object to be compared to previous copies of itself and the entire object tree to be traversed.  
  Detecting changes in immutable objects is considerably easier. If the immutable object that is being referenced is different than the previous one, then the object has changed.

3. Determining When to Re-render in React
  The main benefit of immutability is that it helps you build *pure components* in React. Immutable data can easily determine if changes have been made which helps to determine when a component requires re-rendering.  
  You can learn more about `shouldComponentUpdate()` and how you can build pure components by reading [Optimizing Performance](https://reactjs.org/docs/optimizing-performance.html#examples).

## 有關 arrow function 在 `onClick` 的注意事項

- `onClick` 後面要跟 arrow function： 

```jsx
class Square extends React.Component {
 render() {
   return (
     <button className="square" onClick={() => alert('click')}>
       {this.props.value}
     </button>
   );
 }
}
```

Notice how with `onClick={() => alert('click')}`, we’re passing *a function* as the `onClick` prop.  
**It only fires after a click.**  
Forgetting `() =>` and writing `onClick={alert('click')}` is a common mistake, and would fire the alert every time the component re-renders.  


- 改寫 functional component 變成一個 function：  

```jsx
function Square(props) {
  return (
    <button className="square" onClick={props.onClick}>
      {props.value}
    </button>
  );
}
```

When we modified the Square to be a functional component, we also changed `onClick={() => this.props.onClick()}`   
to a shorter `onClick={props.onClick}`.  

（注意箭頭兩邊的括弧都移除了）  

In a class, we used an arrow function to access the correct `this` value, but in a functional component we don’t need to worry about `this`.  

## [Adding Time Travel](https://reactjs.org/tutorial/tutorial.html#adding-time-travel)

把所有 state 移到更上層的 `<Game />` 來保存。  
並把 state 改成紀錄 history 的形式：  

```jsx
// <Game /> 元素中
constructor(props) {
  super(props);
  this.state = {
    history: [{
      squares: Array(9).fill(null),
    }],
    xIsNext: true,
  };
}

render() {
  const history = this.state.history;
  const current = history[history.length - 1];
  const winner = calculateWinner(current.squares);

let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (this.state.xIsNext ? 'X' : 'O');
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board
          squares={current.squares}
          onClick={(i) => this.handleClick(i)}
        />
      </div>
      <div className="game-info">
        <div>{status}</div>
        <ol>{/* TODO */}</ol>
      </div>
    </div>
  );
}
```
把 `handleClick` 也移到 `<Game />`（用到 [`concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat) 把新的 array 加到最後面）：

```jsx
handleClick = (i) => {
  const history = this.state.history;
  const current = history[history.length - 1];
  const squares = [...current.squares];
  if (calculateWinner(squares) || squares[i]) {
    return;
  }
  squares[i] = this.state.xIsNext ? 'X' : 'O';
  this.setState({
    history: history.concat([{
      squares,
    }]),
      xIsNext: !this.state.xIsNext,
  });
}
  ```

Unlike the array `push()` method you might be more familiar with, the `concat()` method doesn’t mutate the original array, so we prefer it.  

最後，把歷史紀錄按鈕 render 出來（用 `map()`）：  

```jsx
// <Game /> 元素中的 render 裡，多加以下這些：

const moves = history.map((step, move) => {
  const describe = move ?
    'Go to move #' + move :
    'Go to game start';
  return (
    <li key={move}>
      <button onClick={() => this.jumpTo(move)}>{describe}</button>
    </li>
  );
});

// <li> 記得給 key 
```
