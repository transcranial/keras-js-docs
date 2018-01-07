Installation in Node.js is straightforward. The only limitation at this time is that models will be set to CPU mode, even if `gpu: true` is set in the options for `Model`.

```sh
yarn add keras-js
# or
npm install keras-js --save
```

```js
import KerasJS from 'keras-js'
// or
const KerasJS = require('keras-js')
```
