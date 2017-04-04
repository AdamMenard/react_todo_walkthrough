## Sprint 1: React Router

We're going to use React Router today to introduce it as a concept. However, it isn't strictly necessary for this application. We're really just going for exposure here. There's a lot to learn about react router and we'll just be scratching the surface. If you want to dive deeper, checkout [this tutorial](https://github.com/reactjs/react-router-tutorial)

We need React Router in the same way that we needed angular routers. We need a way to link to various urls to components in our application. Because our application will be a SPA, we still want to preserve different application-states via the url. This Todo app's application-states (not to be confused with component state) will just be the root url and a url to all todos(`/` and `/todos`)

### Creating Routes
It's great, Routes are just react Components as well! Let's start by installing the `react-router` dependency, making a `config` folder and a `routes.js` file that will contain our routes:

```bash
$ mkdir src/config
$ touch src/config/routes.js
```

Let's fill in the contents our `routes.js` file:

```js
import React from 'react'
import {Route} from 'react-router'
import App from '../App'

module.exports = (
  <Route path='/' component={App}/>
)
```

All we've done here is added some dependencies as well as added our App component to this file. Then we used the `Route` component, given to us by `react-router` to create a route for the root path(`'/'`). We also establish that the component that should be rendered here is the App component we defined earlier.

> Something that's weird is that we imported `React` from `'react'` but then we imported `{Route}` from `'react-router-dom'`. What's with the curly braces? In the latter case we're actually only importing a specific module of the `react-router-dom` and name spacing it within `Route` If we had omitted the curly's it would have grabbed all of `react-router-dom` functionality. Check out the [react router source code](https://github.com/reactjs/react-router-dom) and we can clearly see the Route is a module within react-router-dom

Great, we've defined out routes, but it's not going to do anything because nothing knows about this file yet. Let's update our `index.js` to use a Router now instead of just rendering the `App` Component. In `index.js`:


```js
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
import {Router, browserHistory} from 'react-router'
import routes from './config/routes.js'

ReactDOM.render(
  <Router routes={routes} history={browserHistory}/>,
  document.getElementById('root')
);
```

Great, we should now be able to see hello world show up!



### A Simple Component
Before we add another route, let's change the header to be more applicable and make it its own component.

In `src/App.js`:

```js
import React, { Component } from 'react';
import Header from './components/Header'

class App extends Component {
  render() {
    return (
      <div className="App">
      <h1>Hello, and welcome! I am a heading tag in App.js! Have a great day!</h1>
      <Header />
      </div>
    );
  }
}

export default App;
```

This will immediately error our code base out, why? (ST-WG)

That's right, we don't actually have that folder let alone the file within it. Let's create those things and define our component within it.

```bash
$ mkdir src/components
$ touch src/components/Header.js
```

In `src/components/Header.js`:

```js
import React, {Component} from 'react'
import {Link} from 'react-router'

class Header extends Component{
  render(){
    return (
      <header>
        <h1><Link to={'/todos'}>React Todos</Link></h1>
      </header>
    )
  }
}

export default Header
```

In this file, we've grabbed some dependencies and stored them in variables and then defined a component. The `Link` component is exactly what you think it is, a link to another route. You can think of it as `data-ui-sref` in angular or even an `href` in plain 'ol HTML

Awesome! We now have a header showing up! Let's click on the link.

```
Warning: [react-router] Location "/todos" did not match any routes
```

This warning makes sense, our `config/routes.js` only has a reference to `'/'` and nothing else. We'll fix that by adding the first parts of our app's main functionality. But before that... let's talk about containers.
