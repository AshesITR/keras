# keras (development version)

Breaking changes:
`set_vocabulary()` arguments `df_data` and `oov_df_value` are deprecated. They are superseded by the new argument `idf_weights`.

New Features:
- Default Tensorflow/Keras version is now 2.5
- The `keras` python module is exported
- Major changes to the underlying handeling of custom R6 layer classes.
  - A new `r_to_py` method is provided for `R6ClassGenerator` objects.
  - R6 custom layers can now inherit directly from python layer classes
    or other R6 custom layer classes
  - Custom R6 layers can now be instantiated directly after conversion of the class generator with `r_to_py`, without going through `create_layer`.
  - `KerasLayer` is deprecated (new classes should inherit directly from `keras$layers$Layer`)
  - `KerasWrapper` is deprecated (new classes should inherit directly from `keras$layers$Wrapper`)
  - `create_wrapper` is deprecated (no longer needed, use `create_layer` directly)
  - All layer class methods provided as R functions now have a `super` in scope that resolves to the python super class object
  - Methods of `super` can be accessed in the 3 common ways:
      (python3 style): `super()$"__init__"()`
      (python2 style): `super(ClassName, self)$"__init__"()`
      (R6 style): `super$initialize()`
  - User defined custom classes that inherit from a python type are responsible for calling `super()$__init__(...)` if appropriate.
  - Custom layers can now properly handle masks (#1225)
    - `supports_masking = TRUE` attribute is now supported
    - `compute_mask()` user defined method is now supported
  - `call()` methods now support a `training` argument, as well as any additional arbritary user-defined arguments


- Many layers gained new arguments, coming to parity with the interface
  available in the latest python version:

  layer name | new argument
  ----------------------
  `layer_gru`  | `time_major`
  `layer_lstm` | `time_major`
  `layer_max_pooling_1d` | `data_format`
  `layer_conv_lstm_2d` | `return_state`
  `layer_depthwise_conv_2d` | `dilation_rate`
  `layer_conv_3d_transpose` | `dilation_rate`
  `layer_conv_1d` | `groups`
  `layer_conv_2d` | `groups`
  `layer_conv_3d` | `groups`
  `layer_locally_connected_1d` | `implementation`
  `layer_locally_connected_2d` | `implementation`
  `layer_text_vectorization` | `vocabulary`

- Added activation functions swish and gelu. (#1226)
- `set_vocabulary()` gains a `idf_weights` argument.

# keras 2.4.0

- Use compat module when using `set_session` and `get_session`. (#1046)
- Allows passing other arguments to `keras_model` eg `name`. (#1045)
- Fixed bug when serializing models with the plaidml backends.(#1084)
- Install keras no longer tries to install scipy because it's already installed by tensorflow (#1081)
- Fixed bug with `layer_text_vectorization` with TensorFlow >= 2.3 (#1131)
- Handle renamed argument `text` to `input_text` in `text_one_hot` (#1133)
- Added TensorFlow 2.3 to the CI (#1102)
- Fix C stack error when using Image Data Generators and Time Series generators with TensorFlow <= 2.0.1 (#1135)
- Fixed warning raised in the initial epoch (@gsteinbu #1130)
- Consistent result when using `text_hashing_trick` with missing values (@topepo #1048)
- Added a custom error message for `k_logsumexp` as it was removed from Keras (#1137)
- Fixed bug when printing models that are not built yet. (#1138)
- Fix drop_duplicates DeprecationWarning with tf 2.3 (@gsteinbu #1139 #1141)
- Fixed bug when plotting the model history if the model used an early stopping callback (#1140)
- `install_keras` now installs a fixed version of h5py, because newer versions are backward incompatible. (#1142)
- Simplify testing utilities by using a `helper-*` file. (#1173)
- Deprecated `hdf5_matrix` if using TF >= 2.4 (#1175)
- Fixed TensorFlow nightly installation on CI (#1176)
- Support for TensorFlow v2.4: just small fixes for custom classes. (#1177)
- Added `untar` argument to `get_file` as it seems to be slightly different from `extract` (#1179)
- Warn when not using the tensorflow implementation of Keras (#1181)
- Added `layer_layer_normalization` (#1183)
- Added `layer_multihead_attention` (#1184)
- Added `image_dataset_from_directory` (#1185)
- Fixed bug when using a custom layer with a time distributed adverb. (#1188)
- Added the `ragged` argument to `layer_input`. (#1193)
- Fixed `*_generator` deadlocks with recent versions of TensorFlow (#1197)

# Keras 2.2.3.0 (CRAN)

- Added `layer_attention` (#1000) by @atroiano.
- Fixed issue regarding the KerasMetricsCallback with TF v2.2 (#1020)

# Keras 2.2.5.0 (CRAN)

- Added `layer_dense_features`.

- Added `on_test_*`, `on_test_batch_*`, `on_predict_*` and `on_predict_*` to callback options.

- Search for the right optimizers and initializers on TensorFlow 2.0

- Fixed bug in function generators when using models with multiple inputs. (#740)

- Added `export_savedmodel` support for TensorFlow 2.0 (#773)

- Fixed bug when using `metric_` functions. (#804)

- Allow users to pass additional arguments to `install_keras` (#808)

- Enabled calling Keras models with R arrays. (#806)

- Allow passing `data.frames` as inputs to Keras models. (#822)

- Fixed bug when passing a fixed validation set to `fit_generator` (#837)

- Fixed bug when passing a TensorFlow dataset to `fit` within a `tf$distribute` scope. (#856)

- `install_keras` will now install Keras dependencies (#856). It won't re-install TensorFlow if it's already installed.

- Fixed deprecation messages showed with TensorFlow v1.14.

- Largely reduced tests verbosity.

## Keras 2.2.4.1 (CRAN)

- Use `tf.keras` as default implementation module.

- Added AppVeyor to test on Windows.

- Added `flow_images_from_dataframe` function (#658).

- Allow for unknown `input_shape` in `application_*` functions.

- Added `save_model_tf` and `load_model_tf` to save/load models in the TensorFlow's
SavedModel format.


# Keras 2.2.4 (CRAN)

- Improve handling of `timeseries_generator()` in calls to `fit_generator()`

- Add support for `input_shape` argument to `layer_dropout()`

- Improve error message for data frames passed to `fit()`, etc.

- Use 1-based axis indices for `k_gather()`

- Added `version` parameter to `install_keras()` for installing alternate/older versions

- Added `activation_exponential()` function.

- Added `threshold` parameter to `activation_relu()`

- Added `restore_best_weights` parameter to `callback_model_checkpoint()`

- Added `update_freq` parameter to `callback_tensorboard()`

- Added `negative_slope` and `threshold` parameters to `layer_activation_relu()`

- Added `output_padding` and `dilation_rate` parameters to `layer_conv_2d_transpose()`

- Added `output_padding` argument to `layer_conv_3d_transpose()`

- Added `data_format` argument to `layer_separable_conv_1d()`, `layer_average_pooling_1d()`,
  `layer_global_max_pooling_1d()`, and `layer_global_average_pooling_1d()`

- Added `interpolation` argument to `layer_upsampling_1d()` and `layer_upsampling_2d()`

- Added `dtype` argument to `to_categorical()`

- Added `layer_activation_selu()` function.

- Added `KerasWrapper` class and corresponding `create_wrapper` function.


# Keras 2.2.0

- Fix issue with serializing models that have constraint arguments

- Fix issue with `k_tile` that needs an integer vector instead of a list as the `n` argument.

- Fix issue with user-supplied `output_shape` in `layer_lambda()` not being supplied to tensorflow backends

- Filter out metrics that were created for callbacks (e.g. `lr`)

- Added `application_mobilenet_v2()` pre-trained model

- Added `sample_weight` parameter to `flow_images_from_data()`

- Use native Keras implementation (rather than SciPy) for `image_array_save()`

- Default `layer_flatten()` `data_format` argument to `NULL` (which defaults to global Keras config).

- Add `baseline` argument to `callback_early_stopping()` (stop training if a given baseline isn't reached).

- Add `data_format` argument to `layer_conv_1d()`.

- Add `layer_activation_relu()`, making the ReLU activation easier to configure
  while retaining easy serialization capabilities.

- Add `axis = -1` argument in backend crossentropy functions specifying the class prediction
  axis in the input tensor.

- Handle symbolic tensors and TF datasets in calls to `fit()`, `evaluate()`, and `predict()`

- Add `embeddings_data` argument to `callback_tensorboard()`

- Support for defining custom Keras models (i.e. custom `call()` logic for forward pass)

- Handle named list of model output names in `metrics` argument of `compile()`

- New `custom_metric()` function for defining custom metrics in R

- Provide typed wrapper for categorical custom metrics

- Provide access to Python layer within R custom layers

- Don't convert custom layer output shape to tuple when shape is a list
  or tuple of other shapes

- Re-export `shape()` function from tensorflow package

- Re-export `tuple()` function from reticulate package

- Indexes for `get_layer()` are now 1-based (for consistency w/ `freeze_weights()`)

- Accept named list for `sample_weight` argument to `fit()`


## Keras 2.1.6

- Fix issue with single-element vectors passed to text preprocessing functions

- Compatibility with TensorFlow v1.7 Keras implementation

- Support `workers` parameter for native Keras generators (e.g. `flow_images_from_directory()`)

- Accept tensor as argument to `k_pow()`

- In `callback_reduce_lr_on_plateau()`, rename `epsilon` argument to `min_delta`
  (backwards-compatible).

- Add `axis` parameter to `k_softmax()`

- Add `send_as_json` parameter to `callback_remote_monitor()`

- Add `data_format` method to `layer_flatten()`

- In `multi_gpu_model()`, add arguments `cpu_merge` and `cpu_relocation` (controlling whether
  to force the template model's weights to be on CPU, and whether to operate merge operations
  on CPU or GPU).

- Record correct loss name for tfruns when custom functions are provided for `loss`


## Keras 2.1.5

- Support for custom constraints from R

- Added `timeseries_generator()` utility function

- New layer `layer_depthwise_conv_2d()`

- Added `brightness_range` and `validation_split` arguments to
  [image_data_generator()].


## Keras 2.1.4

- Added support for `remove_learning_phase` in `export_savedmodel()` to avoid
  removing learning phase.

- Normalize validation data to Keras array in `fit()` and `fit_generator()`

- Ensure that custom layers return a tuple from `compute_output_shape()`

- Added Nasnet and Densenet pre-trained models

- New layers `layer_activation_softmax()` and `layer_separable_conv_1d()`

- Added `amsgrad` parameter to `optimizer_adam()`

- Fix incompatibility with Progbar.update() method in Keras 2.1.4


## Keras 2.1.3

- Models saved via `export_savedmodel()` that make use of learning phases can
  now be exported without having to manually reload the original model.

- Ensure that models saved via `export_savedmodel()` can be served from CloudML

- Run image data generators with R preprocessing functions on the main thread

- Return R list from `texts_to_sequences()`

- Various fixes for `use_implementation()` function


# Keras 2.1.2

- Added `theme_bw` option to plot method for training history

- Support TF Dataset objects as generators for `fit_generator()`, etc.

- Added `use_implementation()` and `use_backend()` functions as alternative to
  setting `KERAS_IMPLEMENATION` and `KERAS_BACKEND` environment variables.

- Added R wrappers for Keras backend functions (e.g. `k_variable()`,
  `k_dot()`, etc.)

- Use 1-based axis for `normalize` function.

- Fix issue with printing training history after early stopping.

- Experimental support for using the PlaidML backend.

- Correct handling for R functions specified in `custom_objects`

- Added `with_custom_object_scope()` function.

- Automatically provide name to loss function during compile
  (enables save/load of models with custom loss function)

- Provide global `keras.fit_verbose` option (defaults to 1)


# keras 2.0.9

- Added `multi_gpu_model()` function.

- Automatically call `keras_array()` on the results of generator functions.

- Ensure that `steps_per_epoch` is passed as an integer

- Import `evaluate()` generic from tensorflow package

- Handle `NULL` when converting R arrays to Keras friendly arrays

- Added `dataset_imbd_word_index()` function

- Ensure that `sample_weight` is passed to `fit()` as an array.

- Accept single function as `metrics` argument to `compile()`

- Automatically cast `input_shape` argument to applications to integer

- Allow Keras models to be composable within model pipelines

- Added `freeze_weights()` and `unfreeze_weights()` functions.

- Implement `export_savedmodel()` generic from TensorFlow package

- Convert R arrays to row-major before image preprocessing

- Use `tensorflow.keras` for tensorflow implementation (TF v1.4)

- Added `application_inception_resnet_v2()` pre-trained model

- Added `dataset_fashion_mnist()` dataset

- Added `layer_cudnn_gru()` and `layer_cudnn_lstm()` (faster
  recurrent layers backed by [CuDNN](https://developer.nvidia.com/cudnn))

- Added `layer_minimum()` function

- Added `interpolation` parameter to `image_load()` function

- Add `save_text_tokenizer()` and `load_text_tokenizer()` functions.

- Fix for progress bar output in Keras >= 2.0.9

- Remove deprecated `implementation` argument from recurrent layers

- Support for passing generators for validation data in `fit_generator()`

- Accept single integer arguments for kernel sizes

- Add standard layer arguments to `layer_flatten()` and `layer_separable_conv_2d()`

- Added `image_array_resize()` and `image_array_save()` for 3D image arrays.

- Allow custom layers and lambda layers to accept list parameters.

- Expose `add_loss()` function for custom layers


# keras 2.0.8

- Add `use_session_with_seed()` function that establishes a random seed for the Keras session.
  Note that this should not be used when training time is paramount, as it disables GPU
  computation and CPU parallelism by default for more deterministic computations.

- Fix for plotting training history with early stopping callback (thanks to @JamesAllingham).

- Return R training history object from `fit_generator()`

- Rename `to_numpy_array()` function to `keras_array()` reflecting automatic use
  of Keras default backend float type and "C" ordering.

- Add standard layer arguments (e.g. `name`, `trainable`, etc.) to merge layers

- Better support for training models from data tensors in TensorFlow (e.g. Datasets, TFRecords). Add a related example script.

- Add `clone_model()` function, enabling to construct a new model, given an existing model to use as a template. Works even in a TensorFlow graph different from that of the original model.

- Add `target_tensors` argument in `compile()`, enabling to use custom tensors or placeholders as model targets.

- Add `steps_per_epoch` argument in `fit()`, enabling to train a model from data tensors in a way that is consistent with training from arrays. Similarly, add `steps` argument in `predict()` and `evaluate()`.

- Add `layer_subtract()` layer function.

- Add `weighted_metrics` argument in compile to specify metric functions meant to take into account `sample_weight` or `class_weight`.

- Enable stateful RNNs with CNTK.


## keras 2.0.6

- `install_keras()` function which installs both TensorFlow and Keras

- Use keras package as default implementation rather than tf.contrib.keras

- Training metrics plotted in realtime within the RStudio Viewer during fit

- `serialize_model()` and `unserialize_model()` functions for saving
  Keras models as 'raw' R objects.

- Automatically convert 64-bit R floats to backend default float type

- Ensure that arrays passed to generator functions are normalized to C-order

- `to_numpy_array()` utility function for custom generators (enables
  custom generators to yield C-ordered arrays of the correct float type)

- Added `batch_size` and `write_grads` arguments to `callback_tensorboard()`

- Added `return_state` argument to recurrent layers.

- Don't re-export `install_tensorflow()` and `tf_config()` from tensorflow
  package.

- `is_keras_available()` function to probe whether the Keras python
  package is available in the current environment.

- `as.data.frame()` S3 method for Keras training history

- Remove names from `keras_model()` inputs

- Return result of `evaluate()` as named list

- Write run metrics and evaluation data to tfruns

- Provide hint to use r-tensorflow environment when importing keras


# keras 2.0.5

- Initial CRAN release
