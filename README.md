## QuantLib Async Bindings for Node.js [![npm version](https://badge.fury.io/js/quantlib.svg)](http://badge.fury.io/js/quantlib) [![Twitter Follow](https://img.shields.io/twitter/follow/quantlibnode.svg?style=social&maxAge=3600)](https://twitter.com/quantlibnode)

This [open source project](https://github.com/quantlibnode/quantlibnode) brings [QuantLib](http://quantlib.org/) to the Node.js community, it's similar to [QuantLibXL](http://quantlib.org/quantlibxl/) project which is for Microsoft Excel.

Most functions in QuantLibXL can be used in the similar way in Node on the server side.

All functions in this project are Async, they are exported to [Promise](https://www.promisejs.org/) sytle function, please see [Example](#example) below.


## Getting started

```sh
npm install quantlib
```

* Windows - 32-Bit

`npm install quantlib` will do everything, including the node package installation and pre-built native addon (no dependency) download, you can start use it right away.

* Windows - 64-Bit

I will try to build and upload the pre-built addon, before that, Please refer to [how to build](#building-the-native-addon) below

* Linux & Mac

Please refer to [how to build](#building-the-native-addon) below


## QuantLib Documents

* [Function Categories](http://quantlib.org/quantlibxl/categories.html)

* [Function List](http://quantlib.org/quantlibxl/allfunctions.html)

## Version Matrix

| QuantLib | QuantLibAddin | Node.js | quantlib.node |
| -------- | ------------- | ------- | ------------- |
|    1.7.1 |         1.7.0 |   6.9.1 |         0.1.x |
|    1.8.1 |         1.8.0 |   6.9.5 |         0.2.x |

## Building the native addon

#### Prerequisite

* CMake 2.8 or above, Visual C++ for windows, Xcode for Mac, GCC for Linux
* Node.js according to [version matrix](#version-matrix)
* [nan](https://github.com/nodejs/nan) ^2.2.0
* [node-gyp](https://github.com/nodejs/node-gyp) ~3.0.3
* QuantLib, QuantLibAddin, ObjectHandler source code according to [version matrix](#version-matrix), they need to be put in the same directory
* `boost` - which is required to build QuantLib

#### Set environment variable

please refer to `cmake/*.cmake` and `CMakeList.txt` files

* `NAN_DIR` - location of `nan`
* `NODE_GYP_DIR` - location of `.node-gyp` generated by `node-gyp` tool, which is at `~/.node-gyp`, if it doesn't exist, follow the instruction in [node-gyp](https://github.com/nodejs/node-gyp), and build a helloword program, it will generate the `.node-gyp` directory
* `QUANTLIB_ROOT` - location of QuantLib, QuantLibAddin, ObjectHandler source code
* `BOOST_ROOT` - location `boost` installed

#### Use cmake to build the addon

1. Build QuantLib and QuantLibAddin, please check `CMakeList.txt` for library name, and make sure generated library names are the same in `CMakeList.txt`
2. from `quantlibnode` root directory `cd build`
3. `cmake ..` for Windows and Linux, `cmake -G Xcode ..` for Mac OS X
4. `cmake --build . --config Release`
5. For Linux, you may need to put `quantlib.node` under `build/Release` manually,

## Example

```js
var ql = require('quantlib');

var mtx1 =
[
  [1.00000,	0.97560,	0.95240,	0.93040,	0.90940,	0.88940,	0.87040,	0.85230,	0.83520,	0.81880],
  [0.97560,	1.00000,    0.97560,    0.95240,    0.93040,    0.90940,    0.88940,    0.87040,    0.85230,    0.83520],
  [0.95240,	0.97560,	1.00000,	0.97560,	0.95240,	0.93040,	0.90940,	0.88940,	0.87040,	0.85230],
  [0.93040,	0.95240,	0.97560,	1.00000,	0.97560,	0.95240,	0.93040,	0.90940,	0.88940,	0.87040],
  [0.90940,	0.93040,	0.95240,	0.97560,	1.00000,	0.97560,	0.95240,	0.93040,	0.90940,	0.88940],
  [0.88940,	0.90940,	0.93040,	0.95240,	0.97560,	1.00000,	0.97560,	0.95240,	0.93040,	0.90940],
  [0.87040,	0.88940,	0.90940,	0.93040,	0.95240,	0.97560,	1.00000,	0.97560,	0.95240,	0.93040],
  [0.85230,	0.87040,	0.88940,	0.90940,	0.93040,	0.95240,	0.97560,	1.00000,	0.97560,	0.95240],
  [0.83520,	0.85230,	0.87040,	0.88940,	0.90940,	0.93040,	0.95240,	0.97560,	1.00000,	0.97560],
  [0.81880,	0.83520,	0.85230,	0.87040,	0.88940,	0.90940,	0.93040,	0.95240,	0.97560,	1.00000]
];

ql.SymmetricSchurDecomposition('mtx#1',mtx1).then(function(obj){

  ql.SymmetricSchurDecompositionEigenvalues(obj).then(function(r){
    console.log(r);
  });

}).catch(function(e){
  console.log(e);
});

```

```sh
>
[ 9.270906840163782,
  0.4207173234885105,
  0.12674770658244172,
  0.059239731356788505,
  0.03595303870722261,
  0.024956978505270924,
  0.019117669503864024,
  0.01580103250921176,
  0.01377474504269164,
  0.012784934140218302 ]
```
