# [React Router](https://reacttraining.com/react-router/)

## How does React Router work?

React Router is a collection of **navigational components**.  

`<Router>` is a component that you can use to bundle your navigations.   

And you use `<Route>` to assign which component to be render.  
Here is an example:  

```js
import React from "react";
import { BrowserRouter as Router, Route, Link } from "react-router-dom";

const Header = () => (
  <ul>
    <li>
      <Link to="/">Home</Link>
    </li>
    <li>
      <Link to="/about">About</Link>
    </li>
    <li>
      <Link to="/topics">Topics</Link>
    </li>
  </ul>
);

const AppRouter = () => (
  <Router>
    <div>
      <Header />

      <Route path="/" exact component={Index} />
      <Route path="/about/" component={About} />
      <Route path="/users/" component={Users} />
    </div>
  </Router>
);

export default AppRouter;
```