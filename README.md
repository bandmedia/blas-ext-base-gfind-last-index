https://github.com/bandmedia/blas-ext-base-gfind-last-index/releases

# ⚡ Fast BLAS Array Last-Index Finder for JavaScript ndarrays

[![Releases](https://img.shields.io/badge/Releases-Download-blue?logo=github)](https://github.com/bandmedia/blas-ext-base-gfind-last-index/releases)

Return the index of the last element that passes a predicate test. Works with typed arrays and strided data layouts. Designed for numeric code that follows BLAS-style strided conventions.

- Topics: array, blas, extended, find, get, index, javascript, last, math, mathematics, ndarray, node, node-js, nodejs, search, stdlib, strided

---

<!-- TOC -->
## Table of contents

- What this does
- Why use it
- Key features
- Install
- Quick example
- API
- Strided and ndarray examples
- Performance and complexity
- Tests and build
- Release assets
- Contributing
- License

---

## What this does

This module finds the last index in an array where a predicate returns true.  
It handles strided arrays. It works with typed arrays used in numeric code.  
It returns the zero-based index relative to the provided view. If no element passes the test, it returns -1.

Use it when you need a fast last-index search in numeric loops. The API mirrors BLAS-style functions with N, stride, and offset parameters for direct array access.

---

## Why use it

- You write numeric code in JavaScript or Node.js.
- You work with typed arrays and strided buffers.
- You need a simple, predictable API.
- You want a function that fits into BLAS-style pipelines and ndarrays.

This module uses a small surface area. It focuses on one job and does it well.

---

## Key features

- Works with all typed arrays and standard arrays.
- Supports stride and offset parameters.
- Supports custom predicate functions.
- Returns -1 when no element satisfies the predicate.
- Small and dependency-free.
- Tested with edge cases and typical numeric input.

---

## Install

Install from npm:

```bash
npm install blas-ext-base-gfind-last-index
```

Or clone the repo and use it locally:

```bash
git clone https://github.com/bandmedia/blas-ext-base-gfind-last-index.git
cd blas-ext-base-gfind-last-index
npm install
```

If you prefer to fetch a release asset, go to the releases page, download the release file and execute it as needed. For example, download the archive file named like blas-ext-base-gfind-last-index-1.2.0.tgz and run:

```bash
npm install ./blas-ext-base-gfind-last-index-1.2.0.tgz
```

You can also run the included examples after install.

---

## Quick example

This example searches a Float64Array for the last negative value.

```js
const gfindLastIndex = require('blas-ext-base-gfind-last-index');

const x = new Float64Array([1.0, -3.5, 2.2, -0.1, 4.0]);

// predicate: negative values
function isNegative(v) {
  return v < 0;
}

// N = length to search, stride = 1, offset = 0
const idx = gfindLastIndex(x.length, x, 1, 0, isNegative);

console.log(idx); // 3 (the index of -0.1)
```

---

## API

Signature (CommonJS):

```js
const gfindLastIndex = require('blas-ext-base-gfind-last-index');
```

Function signature:

gfindLastIndex( N, x, stride, offset, predicate[, thisArg] ) -> number

- N: number of elements to examine (integer >= 0)
- x: input array (typed array or Array)
- stride: step between elements (integer, can be negative)
- offset: starting index in x (integer)
- predicate: function invoked as predicate(value, index, array)
- thisArg: optional this value for predicate

Returns: the last index (zero-based, relative to the full underlying array) where predicate returns true, or -1 if none.

Behavior:

- The loop inspects elements at indices offset + i*stride for i in [0, N-1].
- The function evaluates elements in reverse logical order to find the last match with a single pass.
- If stride is 1 and offset is 0, the examined indices are [0 .. N-1].
- Works with negative strides. For negative stride, offset points to the start position and scanning goes backward.

Example edge cases:

```js
// If N is 0, returns -1
gfindLastIndex(0, x, 1, 0, () => true); // -1

// Negative stride example:
const a = new Float32Array([10, 20, 30, 40, 50]);
const n = 3;
const stride = -1;
const offset = 4; // start at value 50
const idx = gfindLastIndex(n, a, stride, offset, (v) => v < 40);
console.log(idx); // 1 (refers to underlying index 1 within a)
```

---

## Strided and ndarray examples

This module fits into strided workflows. Here are three patterns.

1) Simple strided vector:

```js
const arr = new Float64Array([1, 2, -3, 4, -5, 6]);
const res = gfindLastIndex(6, arr, 2, 0, (v) => v < 0);
// Checks indices 0,2,4 -> finds -5 at index 4 -> returns 4
```

2) Interleaved channels (two-channel data):

```js
// [ch0, ch1, ch0, ch1, ...]
const buf = new Float32Array([0.1, 1.0, -0.2, 2.0, 0.5, -1.0]);
const N = buf.length / 2;
const idxCh0 = gfindLastIndex(N, buf, 2, 0, (v) => v < 0);
// returns 2 (the underlying index 4 in the full array)
```

3) Ndarray-style view with base offset:

```js
const base = new Float64Array([9, 8, 7, 6, 5, 4]);
const offset = 1;
const N = 4;
const stride = 1;
const idx = gfindLastIndex(N, base, stride, offset, (v) => v <= 6);
// checks base[1..4] -> returns 4 (underlying index 4)
```

---

## Performance and complexity

- Time: O(N) in worst case.
- Space: O(1) extra memory.
- The implementation avoids allocations.
- It uses direct index arithmetic for performance.
- For common patterns (stride = 1), it optimizes iteration order.

This function aims to fit into hot loops in numeric code. It keeps the overhead low.

---

## Tests and build

The project includes unit tests and some microbenchmarks.

Run tests:

```bash
npm test
```

Run lint:

```bash
npm run lint
```

Run benchmarks:

```bash
npm run bench
```

Example package.json scripts:

```json
{
  "scripts": {
    "test": "node test/test.js",
    "bench": "node bench/bench.js",
    "lint": "eslint ."
  }
}
```

Test cases cover:

- All typed arrays
- Negative and positive stride
- Zero-length arrays
- Predicate throwing errors

---

## Release assets

You can download release assets here:
https://github.com/bandmedia/blas-ext-base-gfind-last-index/releases

This link leads to release files. Download the release asset that matches your environment. For example, you may download a tarball named like blas-ext-base-gfind-last-index-1.2.0.tgz. After download, install or execute as appropriate:

- Install a tarball with npm:
  npm install ./blas-ext-base-gfind-last-index-1.2.0.tgz

- Or extract an archive and run included example scripts:
  tar -xzf blas-ext-base-gfind-last-index-1.2.0.tgz
  node examples/strided.js

Use the releases page badge above to jump to the page.

---

## Contributing

Contributions follow a simple flow.

- Fork the repo.
- Create a branch for your change.
- Run tests locally.
- Open a pull request.

Code style: keep functions small. Use clear names. Add tests for edge cases. Use GitHub issues to propose design changes.

---

## Images and badges

You can use the release badge above for quick access. Here are a few related badges you can clone:

[![Build](https://img.shields.io/badge/build-passing-brightgreen)]()
[![License](https://img.shields.io/badge/license-MIT-blue)]()

And a visual to emphasize numeric arrays:

![Matrix](https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/Matrix.svg/320px-Matrix.svg.png)

---

## License

MIT License. See the LICENSE file for details.