### Create new model

On instantiation, data is loaded using XHR (same-domain or CORS required), and layers are initialized as a directed acyclic graph:

```js
// in the browser, URLs can be relative or absolute
const model = new KerasJS.Model({
  filepath: 'path/to/model.bin',
  gpu: true
})

// in Node.js, gpu flag will always be off
// paths can be filesystem paths or absolute URLs
// must be specified if filesystem path
const model = new KerasJS.Model({
  filepath: 'path/to/model.bin',
  filesystem: true
})
```

Additional configuration options (shown below are default values):

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
