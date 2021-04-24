# node-red-contrib-tf-function
Mimic the original function node and add `tf` into the context. Then you can
directly access Tensorflow.js APIs through global variable: `tf`. By using
this package, you don't have to modify the `settings.js` to add `tf` into
the global context.

## Installation

`npm install node-red-contrib-tf-function`