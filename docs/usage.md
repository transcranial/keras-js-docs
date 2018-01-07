See the Setup section ([Browser](setup/browser), [Webpack](setup/webpack), or [Node.js](setup/node)) for instructions on importing Keras.js as a library.

### Create new model

On instantiation, data is loaded using XHR (same-domain or CORS required), and layers are initialized as a directed acyclic graph.

In the browser, URLs can be relative or absolute:

```js
const model = new KerasJS.Model({
  filepath: 'path/to/model.bin',
  gpu: true
})
```

In Node.js, the `gpu` flag will always be off, as there is no WebGL interface available. `filepath` can be a file system path or an absolute URL. If `filepath` is a file system path, the additional `filesystem` flag must be specified:

```js
const model = new KerasJS.Model({
  filepath: 'path/to/model.bin',
  filesystem: true
})
```

Additional configuration options are available (shown below are default values):

```js
{
  filepath: 'path/to/model.bin',
  headers: {},
  filesystem: false,
  gpu: false,
  transferLayerOutputs: false,
  pauseAfterLayerCalls: false,
  visualizations: []
}
```

* `headers` - Pass in additional headers during file request, authorization credentials or cache control settings, for example.

* `transferLayerOutputs` - In GPU mode, this setting will transfer the outputs of each layer from GPU to CPU so that they can be accessed (e.g., for visualization purposes -- see the [MNIST CNN](https://github.com/transcranial/keras-js/blob/master/demos/src/components/models/MnistCnn.vue) or [MNIST VAE](https://github.com/transcranial/keras-js/blob/master/demos/src/components/models/MnistVae.vue) demos). Otherwise, they will remain as WebGL texture objects. Turning this on will significantly slow down performance.

* `pauseAfterLayerCalls` - Turning this on will chunk synchronous calls on `Model.predict()` to per-layer instead of per-model. This allows DOM updates and other main thread operations to proceed after each layer. Otherwise, `Model.predict()` will block the event loop during the duration of the call.

* `visualizations` - Array of visualizations to activate. See the [visualizations section](https://transcranial.github.io/keras-js-docs/visualizations/overview) for further details.

### Predict

The class method `ready()` returns a Promise which resolves when initialization steps are complete. Then, use `predict()` to run a forward pass with the input data (also returns a Promise).

For Keras `Model` models, the input data object has keys corresponding to the names of the input layers. For `Sequential` models, the single key should be `input`. Values are the flattened Float32Array representations of the input data. Input tensor shapes are specified in the model config.

Similarly, the output data object has keys corresponding to the names of the output layers for Keras `Model` models. For `Sequential` models, the single key will be `output`.

```js
model
  .ready()
  .then(() => {
    // input data object keyed by names of the input layers
    // or `input` for Sequential models
    // values are the flattened Float32Array data
    // (input tensor shapes are specified in the model config)
    const inputData = {
      input_1: new Float32Array(data)
    }

    // make predictions
    return model.predict(inputData)
  })
  .then(outputData => {
    // outputData is an object keyed by names of the output layers
    // or `output` for Sequential models
    // e.g.,
    // outputData['fc1000']
  })
  .catch(err => {
    // handle error
  })
```

Alternatively, we could also use async/await:

```js
try {
  await model.ready()
  const inputData = {
    input_1: new Float32Array(data)
  }
  const outputData = await model.predict(inputData)
} catch (err) {
  // handle error
}
```
