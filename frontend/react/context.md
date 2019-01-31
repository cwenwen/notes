# React Context

Context 用來紀錄 React components 共享（global）的內容，資訊不用一個一個 components 的傳下去。  

如果只是要避免這件事，另一簡單解：component composition。  
然而，有些資訊必須在不同的巢狀結構中共用。Context lets you “broadcast” such data, and changes to it, to all components below.  

使用時機： managing the current locale, theme, or a data cache。

```js
// Context lets us pass a value deep into the component tree
// without explicitly threading it through every component.
// Create a context for the current theme (with "light" as the default).
const ThemeContext = React.createContext('light');

class App extends React.Component {
  render() {
    // Use a Provider to pass the current theme to the tree below.
    // Any component can read it, no matter how deep it is.
    // In this example, we're passing "dark" as the current value.
    return (
      <ThemeContext.Provider value="dark">
        <Toolbar />
      </ThemeContext.Provider>
    );
  }
}

// A component in the middle doesn't have to
// pass the theme down explicitly anymore.
function Toolbar(props) {
  return (
    <div>
      <ThemedButton />
    </div>
  );
}

class ThemedButton extends React.Component {
  // Assign a contextType to read the current theme context.
  // React will find the closest theme Provider above and use its value.
  // In this example, the current theme is "dark".
  static contextType = ThemeContext;
  render() {
    return <Button theme={this.context} />;
  }
}
```

## React.createContext

```js
const MyContext = React.createContext(defaultValue);
```

Creates a Context object.   

When React renders a component that subscribes to this Context object it will read the current context value from the closest matching `Provider` above it in the tree.  

The `defaultValue` argument is **only** used when a component does not have a matching Provider above it in the tree. This can be helpful for testing components in isolation without wrapping them. Note: passing `undefined` as a Provider value does not cause consuming components to use `defaultValue`.

## Context.Provider

```js
<MyContext.Provider value={/* some value */}>
```
