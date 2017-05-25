# Webpack 2: The Complete Developer's Guide

|#|Content|
|:---:|:---|
|1.|[Why do we use building tools?](#why-do-we-use-building-tools)|
|2.|[Installation and configuration](#installation-and-configuration)|

### Why do we use building tools?

Modern web development, like SPA, needs a gigantic chunk of JavaScript code, which makes the developers hard to navigate the code. So to address the issue of large code bases, the idea of **JavaScript modules** was born.

|Module system|Common syntax|
|:---:|:---:|
|CommonJS|module.exports / require|
|AMD|define / require|
|ES6|export / import|

In the module base of world, rather than having a few very large files in JavaScript, we tend to have many small files.

:+1: *Benefits*: It's more clear where code locates in our project

:-1: *Problems*:
- Load order: We need to make sure the order of our code executed
- Performance: Having many JavaScript files and loading them over HTTP connection is considered a bad idea in performance standpoint

The purpose of webpack is to take a big collection of tiny JavaScript modules and merge them into one big JavaScript file. Also ensuring each module is executing in the correct order.

### Installation and Configuration

```bash
$ npm install webpack --save-dev
```

After installation, we need to create **webpack.config.js** inside the project folder. And there are two configuration that must be provided to webpack in order to run properly, which are **Entry property** and **Output property**

- Entry property defines the **entry point** of the application
- Output property defines where should we output the bundled file

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

Add the **build** script in package.json

```javascript
// package.json
{
    "scripts": {
    "build": "webpack"
  },
}
```

Run webpack

```bash
$ npm run build
```
