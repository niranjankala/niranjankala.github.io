---
title: "Effortless Development: Hot Reloading in React with react-hot-loader"
published: true
description: "Effortless Development: Hot Reloading in React with react-hot-loader"
tags: react
cover_image: 
canonical_url: https://niranjankala.in/post/effortless-development-hot-reloading-in-react-with-react-hot-loader
layout: post
---

**Introduction:**    

As we delve into the intricate world of React development, we encounter a common inconvenience – the need to manually refresh our browsers every time a code change is made. While this may seem like a minor hiccup, it does disrupt the flow of development. Fear not, for there's a simple solution: introducing `react-hot-loader`. This tool revolutionizes the development experience, allowing us to witness real-time changes without the hassle of constant browser refreshes.

**Installing react-hot-loader:**    

To embark on this journey of seamless development, let's begin by installing `react-hot-loader`. Open your terminal in the project directory and run:

```bash
npm install --save-dev react-hot-loader
```

This quick installation paves the way for a more fluid development process.

**Implementing Hot Reloading:**   

Now, let's integrate `react-hot-loader` into our React app. Open the `app.js` file and follow these simple steps. At the top, import the `hot` function from `react-hot-loader`:

```javascript
import { hot } from 'react-hot-loader';
```

Then, modify the export statement at the bottom of the file:

```javascript
export default hot(module)(App);
```

This elegant addition ensures that any changes made to our app reflect instantly without the need for manual browser refresh.

**Streamlining the Development Workflow:**   

To further enhance our workflow, let's create an npm script for running the development server effortlessly. Open your `package.json` file and add the following script:

```json
"scripts": {
  "dev": "npx webpack-dev-server --mode development",
  ...
}
```

Now, running `npm run dev` in the terminal initiates the webpack dev server, building and serving our app dynamically.

**Understanding the Magic Behind the Scenes:**   

While the webpack dev server is running, you may notice the absence of the expected `dist` folder. This is by design – webpack dev server holds the `dist` folder in memory, serving it dynamically and discarding it upon shutdown.

**Building the React App:**
For scenarios where you want to generate the `dist` folder physically, we can create a build script. Update the `scripts` section in `package.json`:

```json
"scripts": {
  "build": "npx webpack --mode development",
  ...
}
```

Executing `npm run build` compiles our React app, producing the `dist` folder with all the transpiled code.

**Conclusion:**

With `react-hot-loader` seamlessly integrated into our React development environment, the days of manual browser refreshes are behind us. Real-time updates and a streamlined workflow empower us to focus on what truly matters – crafting exceptional React applications. Embrace the efficiency, embrace the future of React development!