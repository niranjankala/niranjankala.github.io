---
title: "Building Your First React App: Understanding Index.js and App Component"
published: true
description: "Building Your First React App: Understanding Index.js and App Component"
tags: react
cover_image: 
canonical_url: https://niranjankala.in/post/building-your-first-react-app-understanding-index-js-and-app-component
layout: post
---

**Introduction:**
Congratulations on making it through the setup phase! Now that we've established support for ES6 and React syntax, it's time to delve into the core of our React application. In this section, we'll focus on two crucial files: `index.js` and `App.js`. These files play pivotal roles in rendering our React components and defining the root structure of our application.

**Creating the Files:**
To kick things off, we'll create three essential files within our `src` folder: `index.js`, `App.js`, and `App.css`. In this step, we'll concentrate on `index.js`, which serves as the entry point for inserting our React app into the `index.html` page.

```javascript
// index.js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App.js';

ReactDOM.render(<App />, document.getElementById('root'));
```

This code snippet sets the stage for rendering our `App` component into the HTML element with the ID of 'root'.

**Writing the App Component:**
Moving on to `App.js`, we'll define our root component. This is where we import React, link our CSS file, and create a simple functional component.

```javascript
// App.js
import React from 'react';
import './App.css';

const App = () => {
  return (
    <div className="App">
      <h1>Hello, World!</h1>
    </div>
  );
};

export default App;
```

Here, we're introducing a basic structure with styling that we'll later customize.

**Basic Styling with App.css:**
Our styling needs are modest for now. In `App.css`, we add some basic styles to make our application visually appealing.

```css
/* App.css */
.App {
  margin: 1rem;
  font-family: 'Arial', 'Helvetica', sans-serif;
  color: #222222;
}
```

These styles provide a clean and straightforward look to our React app.

**Preparing for Development:**
Before we witness our app in action, we need to install React and ReactDOM. Run the following command in your terminal:

```bash
npm install react react-dom
```

**Conclusion:**
We've successfully set the foundation for our React app. The `index.js` file orchestrates the rendering process, and the `App.js` file defines the root component with basic styling. In the upcoming sections, we'll explore how to leverage Webpack to build and serve our project, bringing our React application to life. Stay tuned for an exciting journey into the heart of React development!