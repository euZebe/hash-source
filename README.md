# Hash Source

This is an addon for [Reach Router](https://reach.tech/router) to allow you to
use hash history.

e.g.

```
example.com/#/about
example.com/#/contact
```

## Install

```sh
npm i hash-source
# or
yarn add hash-source
```

## Usage

```jsx
import { createHistory, Link, LocationProvider, Router } from '@reach/router'
import createHashSource from 'hash-source'
import React from 'react'
import ReactDOM from 'react-dom'
import About from './routes/About'
import Contact from './routes/Contact'
import Home from './routes/Home'

let source = createHashSource()
let history = createHistory(source)

const App = () => {
  return (
    <LocationProvider history={history}>
      <header>
        <nav>
          <Link to="/">Home</Link>
          <Link to="about">About</Link>
          <Link to="contact">Contact</Link>
        </nav>
      </header>

      <hr />

      <Router>
        <Home path="/" />
        <About path="about" />
        <Contact path="contact" />
      </Router>
    </LocationProvider>
  )
}
```

## navigate programmatically
When you need to navigate programmatically (after a form submit for instance), *do not import* `navigate` from reach-router, but rather use `props.navigate`at a route component level:
```
import { createHistory, Link, LocationProvider, Router } from "@reach/router";
import React from "react";
import ReactDOM from "react-dom";
import createHashSource from "hash-source";

const Home = () => <div>Home</div>;
const About = () => <div>About</div>;
const Contact = (
  { navigate } // navigate is accessible here because Contact is a SubComponent of Router, in the VDom
) => (
  <div>
    <div>Contact</div>
    <button onClick={() => navigate("/about")}>
      programmatically navigate to about
    </button>
  </div>
);

const source = createHashSource();
const history = createHistory(source);

function App() {
  return (
    <LocationProvider history={history}>
      <header>
        <nav>
          <Link to="/">Home</Link>
          <Link to="about">About</Link>
          <Link to="contact">Contact</Link>
        </nav>
      </header>

      <hr />

      <Router>
        <Home path="/" />
        <About path="about" />
        <Contact path="contact" />
      </Router>
    </LocationProvider>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

or use `<Loocation>` and its Children as a function pattern:
```
import React from "react";
import { Location } from "@reach/router";

// to prevent `navigate` prop drilling, we can also use the Location component
const NonRoutedComponent = () => (
  <Location>
    {({ navigate }) => (
      <button onClick={() => navigate("/")}>Submit then Back home</button>
    )}
  </Location>
);

const SubComponent = () => (
  <div>
    <p>This is a sub component.</p>
    <NonRoutedComponent />
  </div>
);

const HugeComponent = () => (
  <div>
    <p>
      Imagine this is a big component with a bunch of subcomponents. You may not
      want to pass drill `navigate` till the very last component requiring it.
    </p>
    <SubComponent />
  </div>
);

export default HugeComponent;
```

## Demo
https://codesandbox.io/s/reach-router-hash-source-npzq8
