# Angular 2 with Webpack Project Setup


### [Repository](https://github.com/mirkonasato/angular2-course-webpack-starter)

## Structure Build

* Create **index.html**

* Create */src/app/app.component.ts* and set: 

```
import { Component } from '@angular/core';
```

* Create **package.json**, set name,version, dependencies: *https://gist.github.com/mirkonasato/5822686059d4ce4bdac83697ce7c89dc*

* Run `npm install` now files are downloaded to 'node_modules'

* Create `main.js` at root to bootstrap the app.

## Install Typscript

* `npm install --save-dev typescript@1.8.10` its now added to **package.json** devDependencies

* At root create **tsconfig.json**  :

```
{  
  "compilerOptions": {
    "target": "es5",
    "experimentalDecorators": true,  
    "emitDecoratorMetadata": true  
  },  
  "compileOnSave": false  
}
``` 

* `npm bin` will show location of folder: 8angular2-starter\node_modules\.bin*

* `node_modules/.bin/tsc -v` to show Type Script Compiler version: **Version 1.8.10**

* `node_modules/.bin/tsc` to run Type Script Compiler

* `node_modules/.bin/tsc --rootDir src --outDir dist` Will set the files origin and the destination of compiled files.
running it now will create the *dist* folder, yet with many errors.

* The errors  "Promise", "Set" or "Map" which are global objects of the ES6 specification. When we set `"target": "es5"`, those objects won't be available.
core.js library provides this type definitions but Type Script Compiler can read it since it is plane javascript so it doesn't declare types or interfaces.

* We need to provide a **DECLARATION FILE**, first install Typings:

```
 npm install --save-dev typings@1.4.0  
```

* Typings folder created under .bin commands folder, `node_modules/.bin/typings -v`

* Find type definitions for core-js
```
node_modules/.bin/typings search core-js
```
*Prints out*

```
NAME    SOURCE HOMEPAGE                             VER UPDATED
core-js dt     https://github.com/zloirock/core-js/ 2   2016-11-30T13:37:42.000Z
```
*So we generate install as follows(DT: Definitly Type Repository)*
```
node_modules/.bin/typings install --global --save dt~core-js
```
* *Got message*: Update available 1.4.0 → 2.1.0      │
   │            Run npm i -g typings to update

   It created a "typings" folder and "typings.json" file at the root

* Now we can run compilation without errors
```
node_modules/.bin/tsc --rootDir src --outDir dist
```

## NPM Scripts

* Create `npm run build` tsc alias:
```
  "scripts": {
    "build": "tsc --rootDir src --outDir dist"
  },
  ```
* Set "Typings" to be installed right after npm install:
```
    "scripts": {
        "build": "tsc --rootDir src --outDir dist",
        "postinstall": "typings install"
    },
```
* Define .gitignore file on root
```
/dist/
/node_modules/
/typings/
```
At this stage we can open *index.html* in the browser and connect the generated */dist/main.js* and the app won't run since the `import` statement is translated into `require` which is not supported in ES5, thus we need another package like **sytemjs** or **webpack**.

## Install Webpack

```
npm install --save-dev webpack@1.13.2
```
* Webpack supports by default only javascript files, so to use typescript we need and additional package, loaders.
```
npm install --save-dev ts-loader@0.8.2
```
* To run webpack, we need a configuration file named `webpack.config.js`
```
var webpack = require('webpack');

module.exports = {
    entry: './src/main.ts',
    output: {
        path: './dist',
        filename: '.app.bundle.js'
    },
    module: {
        loaders: [{test: /\.ts$/,loader: 'ts'}]
    },
    extensions: ['', '.js', '.ts']
}
```
* Now we can rebuild the app
```
node_modules/.bin/webpack --progress
```
Yet we have errors: *Uncaught reflect-metadata shim is required when using class decorators*

Even thou `"reflect-metadata": "0.1.8",` is present in the package.json, it's a prerequisite and not connected to webpack.

To fix this, we have to import those files into *main.ts*
```
import 'core-js';
import 'reflect-metadata';
import 'zone.js/dist/zone'
```



