To directly use the UMD build of Keras.js, include the library in your index.html or html template file. The [unpkg](https://unpkg.com/) CDN is a good way to go here:

```html
<script src="https://unpkg.com/keras-js"></script>
```

This will always point to the latest release on NPM. You can also use a specific version/tag via URLs like `https://unpkg.com/keras-js@1.0.2`.

`KerasJS` will now be in the global namespace and can be used directly:

```js
const model = new KerasJS.Model({
  // ...
})
```
