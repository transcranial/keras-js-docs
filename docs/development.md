**Library**

Run `yarn` or `npm install` to install all dependencies. To run all tests, start `npm run server` and simply go to [http://localhost:3000/test/](http://localhost:3000/test/). All tests will automatically run. Open up browser devtools for additional test data info.

For development of Keras.js, start:

```sh
npm run watch
```

Editing of any file in `src/` will trigger webpack to update `dist/keras.min.js`. The development server explicitly has caching turned off, so simply refresh to load the updated build.

Running `npm run build` will build both a UMD bundle, located in `dist/`, as well as babel-transpiled source, located in `lib/`.

**Tests**

There are extensive tests for each implemented layer. See `notebooks/` for the jupyter notebooks creating the structure and data for these tests. These tests are by no means exhaustive, and many more (especially various graph configurations) will be added.

**Demos**

Data files for the demos are located at `demos/data/`. These are located in the [keras-js-demos-data](https://github.com/transcranial/keras-js-demos-data) repo--simply clone and copy the contents to `demos/data/`.

See `notebooks/demos/` for the jupyter notebooks creating the HDF5-format Keras model files, which are then used to generate Keras.js binary files.

For development of Keras.js demos, start:

```sh
npm run demos:watch
```
