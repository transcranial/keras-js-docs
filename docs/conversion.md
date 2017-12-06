## Format

Keras.js uses a custom protocol buffer format binary file that is a serialization of the HDF5-format Keras model and weights file. The `python/encoder.py` script performs this necessary conversion.

The HDF5-format Keras model file must include _both_ the model architecture and the weights. This is the default behavior for Keras model saving:

```py
model.save('example.h5')
```

Note that when using the [ModelCheckpoint](https://keras.io/callbacks/#modelcheckpoint) callback, `save_weights_only` must not be set to `True` (default is `False`).

Works for both Keras `Model` and `Sequential` classes:

```py
model = Sequential()
model.add(...)
...
```

```py
...
model = Model(inputs=..., outputs=...)
```

## Requirements

* NumPy
* h5py
* protobuf 3.4+

## Running the script

From the `python/` directory:

```sh
./encoder.py --help
```

```text
usage: encoder.py [-h] [-n NAME] [-q] hdf5_model_filepath

positional arguments:
  hdf5_model_filepath

optional arguments:
  -h, --help            show this help message and exit
  -n NAME, --name NAME  model name (defaults to filename without extension if
                        not provided)
  -q, --quantize        quantize weights to 8-bit unsigned int
```

Example:

```sh
./encoder.py -q /path/to/model.h5
```

## Quantization

The quantize flag enables weights-wise 8-bit uint quantization from 32-bit float, using a simple linear min/max scale calculated for every layer weights matrix. This will result in a roughly 4x reduction in the model file size. For example, the model file for Inception-V3 is reduced from 92 MB to 23 MB. Client-side, Keras.js then restores uint8-quantized weights back to float32 during model initialization.

The tradeoff, of course, is slightly reduced performance, which may or may not be perceptible depending on the model type and end application. For a study on the performance effects of quantization, [this](https://github.com/aaron-xichen/pytorch-playground) is an excellent resource.
