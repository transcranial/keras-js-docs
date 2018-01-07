If you use webpack for building your app, you can easily add and import Keras.js as a dependency:

```sh
yarn add keras-js
# or
npm install keras-js --save
```

```js
import KerasJS from 'keras-js'
```

**Webpack Config**

The following must be included in your webpack configuration file:

```js
node: {
  fs: 'empty'
}
```

Otherwise when building your app's webpack bundle, you may encounter the following error:

```
ERROR in ./node_modules/keras-js/lib/Model.js
Module not found: Error: Can't resolve 'fs' in '/usr/src/app/node_modules/keras-js/lib'
```

**UglifyJS**

If you use UglifyJS when building your bundle, it is important that the option `compress` be set to `false`. The default is `true`, so UglifyJS will drop unreferenced functions and variables (simple direct variable assignments do not count as references).

_This is important, as a bundle built using the default options will not work!_

For [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin) 1.0+ (which uses [UglifyJS v3](https://github.com/mishoo/UglifyJS2/tree/harmony)):

```js
new UglifyJsPlugin({
  // ...
  uglifyOptions: { compress: { unused: false } }
})
```

For [uglifyjs-webpack-plugin](https://github.com/webpack-contrib/uglifyjs-webpack-plugin) < 1.0:

```js
new UglifyJsPlugin({
  // ...
  compress: { unused: false }
})
```
