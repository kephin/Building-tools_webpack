# Webpack 2: The Complete Developer's Guide

|#|Content|
|:---:|:---|
|1.|[Why do we use building tools?](#why-do-we-use-building-tools)|
|2.|[Installation and configuration](#installation-and-configuration)|
|3.|[Handling project assets](#handling-project-assets)|

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
      test: /\.(jpe?g|png|gif|svg)$/,
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
