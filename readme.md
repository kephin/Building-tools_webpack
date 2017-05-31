# Webpack 2: The Complete Developer's Guide

|#|Content|
|:---:|:---|
|1.|[Why do we use building tools?](#why-do-we-use-building-tools)|
|2.|[Installation and configuration](#installation-and-configuration)|
|3.|[Handling project assets](#handling-project-assets)|
|4.|[Enhance Performance with Code splitting](#enhance-performance-with-code-splitting)|
|5.|[Webpack dev server](#webpack-dev-server)|

### Why do we use building tools?

Modern web development, like SPA, needs a gigantic chunk of JavaScript code, which makes the developers hard to navigate the code. So to address the issue of large code bases, the idea of **JavaScript modules** was born.

|Module system|Common syntax|
|:---:|:---:|
|CommonJS|module.exports / require|
|AMD|define / require|
|ES6|export / import|

In the module base of world, rather than having a few very large files in JavaScript, we tend to have many small files.

:+1: *Benefits*: It's more clear where code locates in our project.

:-1: *Problems*:
1. Load order: We need to make sure the order of our code executed
2. Performance: Having many JavaScript files and loading them over HTTP connection is considered a bad idea in performance standpoint

The purpose of webpack is to take a big collection of tiny JavaScript modules and merge them into one big JavaScript file. Also ensuring each module is executing in the correct order.

### Installation and Configuration

```bash
$ npm install webpack --save-dev
```

After installation, we need to create **webpack.config.js** inside the project folder. And there are two configuration that must be provided to webpack in order to run properly, which are **Entry property** and **Output property**.

- Entry property: Defines the **entry point** of the application.
- Output property: Defines where should we output the bundled file.

```javascript
// webpack.config.js
const path = require('path');

const config = {
  entry: './src/index.js',
  output: {
    // absolute path
    path: path.resolve(__dirname, 'build'),
    filename: 'bundle.js',
  },
};

module.exports = config;
```

Add the **build** script.

```javascript
// package.json
{
  "scripts": {
    "build": "webpack"
  },
}
```

Now we can run webpack.

```bash
$ npm run build
```

### Handling Project Assets

:floppy_disk: **Loaders**

Module loaders are doing some **pre-processing** on files before they are added to the bundle.js, such as transpiling to ES6 / 7 code, handling assets like .css, .jpg, etc.

Two configurations should be added in module.rules:

- Test property: Identify what files should be transformed by a certain loader.
- Use property: Transform that file so that it can be added to your dependency graph (and eventually your bundle).

:electric_plug: **Plugins**

In order to use a plugin, we need to require() it and add it to the plugins array. Most plugins are customizable via options.

Since you can use a plugin multiple times in a config for different purposes, you need to create an instance of it by calling it with new.

```javascript
// installed via npm
const ExtractTextPlugin = require('extract-text-webpack-plugin');
const HtmlWebpackPlugin = require('html-webpack-plugin');
// to access built-in plugins
const webpack = require('webpack');

const config = {
  module: {
    rules: [{
      test: /\.(jpe?g|png|gif|svg)$/i,
      use: [{ loader: 'url-loader', options: { limit: 40000 } }, 'image-webpack-loader']
    }]
  },
  plugins: [
    new ExtractTextPlugin('style.css'),
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({ template: './src/index.html' })
  ]
};
```

### Enhance Performance with Code splitting

Webpack allows us to separate our JavaScript code and only load up certain part of the code base while in the particular part of our application.

With code splitting, webpack splits bundle.js output into separate individual files and then programmatically decides when to load up different piece of bundle inside our code base. In other words, we can control exactly when to load up different modules to show different code inside our project.

|#|Content|
|:---:|:---|
|1.|[3rd party libraries](#code-splitting---3rd-party-libraries)|
|2.|[CSS](#code-splitting---css)|
|3.|[Asynchronous](#code-splitting---asynchronous)|

#### Code splitting - 3rd party libraries

#### Code splitting - CSS

#### Code splitting - Asynchronous

This allows you to serve a minimal bootstrap bundle first and to asynchronously load additional features later.

First of all, we need to install the *syntax-dynamic-import* plugin in order to use **import** syntax.

```bash
$ npm install babel-plugin-syntax-dynamic-import -D
```

```javascript
// webpack.config.js
const config = {
  module: {
    rules: [{
      test: /\.js$/,
      exclude: /(node_modules)/,
      use: [{
        loader: 'babel-loader',
        options: {
          presets: ['env'],
          // add plugins for import syntax
          plugins: ['syntax-dynamic-import'],
        },
      }],
    }],
  },
};

```

The following code shows that webpack do not load *image_viewer.js* at the beginning, and will only import it after the user click on the button.

```javascript
// index.js
const button = document.createElement('button');
button.innerText = 'Click me';
button.onclick() = () => {
  import('./image_viewer').then(module => module.default());
}
document.body.appendChild('button');
```

System.import('URL'):
 - When we execute this function call, the browser will reach out back to the server with the URL to fetch the module and send back to the client in order to be executed.
 - Asynchronous

### Webpack Dev Server

Installation via npm

```bash
$ npm install -D webpack-dev-server
```

Script configuration

```javascript
{
  "scripts": {
    "serve": "webpack-dev-server"
  },
}
```

No when we run

```bash
$ npm serve
```

**webpack-dev-server** will try to build the project and start the server on *http://localhost:8080*

**webpack-dev-server** will watch for any changes that we made to our project files, and when it sees changes, it will automatically rebuild the project for us. The key point here is: It doesn't rebuild the whole project, it only rebuild the changed file. The benefit is we can dramatically reduce the build time.

If you change the webpack.config.js file, you have to restart the **webpack-dev-server**.

When you run **webpack-dev-server**, it internally will execute **webpack** but it stops **webpack** from actually saving any built files in our project directory, instead, it saves all the files in memory. So we will not see the built files in the project folder.

**webpack-dev-server** is only for development and not for production use.
