<p align="center">
  <strong>**This project is no longer active. Please check out <a href="http://js.tensorflow.org">TensorFlow.js</a>.**<br/>The <a href="https://transcranial.github.io/keras-js">Keras.js demos</a> still work but is no longer updated.</strong>
</p>

<p align="center">
  <a href="https://transcranial.github.io/keras-js">
    <img src="https://cdn.rawgit.com/transcranial/keras-js/73aa4cca/assets/logo.svg" width="300px" />
  </a>
</p>

<p align="center">
  <strong>Run Keras models in the browser, with GPU support using WebGL</strong>
</p>

<br/>

## Introduction

Run [Keras](https://github.com/fchollet/keras) models in the browser, with GPU support provided by WebGL 2. Models can be run in Node.js as well, but only in CPU mode. Because Keras abstracts away a number of frameworks as backends, the models can be trained in any backend, including TensorFlow, CNTK, etc.

Library version compatibility: Keras 2.1.2

## Features

In GPU mode, computation is performed by WebGL shaders. All layers implement a GPU version of its computation call, so CPU <-> GPU data transfer only occurs at the start and at the end of each predict call. This results in 1-2 orders of magnitude faster performance over CPU mode.

There are a myriad of benefits to running neural networks client-side in the browser, including but not limited to: reduced data transfer and latency of server-client communication, the ability to offload computation to end-user clients, privacy and security.

There are always tradeoffs to consider, of course. For example, eliminating the need to upload mode input data repeatedly comes at the cost of an initial model file download. Depending on the size of input data and number of uses per model download, this can be a worthwhile tradeoff. Additionally, there are [caching](caching) and [quantization](conversion/#quantization) strategies to address this.

## Limitations

**WebGL 2**

To run GPU mode in the browser, WebGL 2 (which uses the OpenGL ES 3.0 specification) is required. Unfortunately, this is not supported in all browsers, yet. We use extensively [features exclusive to WebGL 2](https://webgl2fundamentals.org/webgl/lessons/webgl2-whats-new.html), so this unfortunately is a hard requirement. The capabilities which WebGL 2 affords us greatly outweigh the c`urrent browser restrictions. To date, [not all browsers support WebGL 2](https://caniuse.com/webgl2), but support by all major browsers should happen at some point in the future.

**MAX_TEXTURE_SIZE**

In GPU mode, tensors are encoded as WebGL textures. The size of these tensors are limited by the parameter `MAX_TEXTURE_SIZE`, which differs by browser/platform/hardware. See [here](http://webglstats.com/) for typical expected values. For operations involving tensors where this value is exceeded along any dimension, we break up the tensor into an array of texture fragments, and perform computation on these fragments. This process naturally incurs some overhead.

**WebWorkers**

Keras.js can be run in a WebWorker separate from the main thread. Because Keras.js performs a lot of synchronous computations, this can prevent the DOM from being blocked. However, one of the biggest limitations of WebWorkers is the lack of `<canvas>` (and thus WebGL) access, so it can only be run in CPU mode for now. In other words, Keras.js in GPU mode can only be run in the main thread. This will not be the case forever: [OffscreenCanvas is in development](https://caniuse.com/offscreencanvas).

**Lambda layers**

Currently, there is no way to port custom Lambda layers, as these will need to be re-implemented in JavaScript. In the future, there will be a means to do so.

## Implemented Layers

**Advanced Activations**

- ELU
- LeakyReLU
- PReLU
- ThresholdedReLU

**Convolutional**

- Conv1D
- Conv2D
- Conv2DTranspose
- Conv3D
- Cropping1D
- Cropping2D
- Cropping3D
- SeparableConv2D
- UpSampling1D
- UpSampling2D
- UpSampling3D
- ZeroPadding1D
- ZeroPadding2D
- ZeroPadding3D

**Core**

- Activation
- Dense
- Dropout
- Flatten
- Permute
- RepeatVector
- Reshape
- SpatialDropout1D
- SpatialDropout2D
- SpatialDropout3D

**embeddings**

- Embedding

**Merge**

- Add
- Average
- Concatenate
- Dot
- Maximum
- Minimum
- Multiply
- Subtract

**Noise**

- GaussianDropout
- GaussianNoise

**Normalization**

- BatchNormalization

**Pooling**

- AveragePooling1D
- AveragePooling2D
- AveragePooling3D
- GlobalAveragePooling1D
- GlobalAveragePooling2D
- GlobalAveragePooling3D
- GlobalMaxPooling1D
- GlobalMaxPooling2D
- GlobalMaxPooling3D
- MaxPooling1D
- MaxPooling2D
- MaxPooling3D

**Recurrent**

- GRU
- LSTM
- SimpleRNN

**Wrappers**

- Bidirectional
- TimeDistributed

## License

[MIT](https://github.com/transcranial/keras-js/blob/master/LICENSE)
