# highs-js

[![npm version](https://badge.fury.io/js/highs.svg)](https://www.npmjs.com/package/highs)
[![CI status](https://github.com/lovasoa/highs-js/actions/workflows/CI.yml/badge.svg)](https://github.com/lovasoa/highs-js/actions/workflows/CI.yml)

This is a javascript linear programming library.
It is built by compiling [HiGHS](https://highs.dev) to WebAssembly using emscripten.

## Usage

```js
const highs = await require("highs")();

const PROBLEM = `Maximize
 obj: x1 + 2 x2 + 3 x3 + x4
Subject To
 c1: - x1 + x2 + x3 + 10 x4 <= 20
 c2: x1 - 3 x2 + x3 <= 30
 c3: x2 - 3.5 x4 = 0
Bounds
 0 <= x1 <= 40
 2 <= x4 <= 3
End`;

const sol = Module.solve(PROBLEM);

assert.deepEqual(sol, {
  Columns: {
    x1: {
      Index: 0,
      Status: 'UB',
      Lower: 0,
      Upper: 40,
      Primal: 40,
      Dual: 1.29167,
      Name: 'x1'
    },
    x2: {
      Index: 1,
      Status: 'BS',
      Lower: 0,
      Upper: Infinity,
      Primal: 10.2083,
      Dual: -0,
      Name: 'x2'
    },
    x3: {
      Index: 2,
      Status: 'BS',
      Lower: 0,
      Upper: Infinity,
      Primal: 20.625,
      Dual: -0,
      Name: 'x3'
    },
    x4: {
      Index: 3,
      Status: 'BS',
      Lower: 2,
      Upper: 3,
      Primal: 2.91667,
      Dual: -0,
      Name: 'x4'
    }
  },
  Rows: [
    {
      Index: 0,
      Status: 'UB',
      Lower: -Infinity,
      Upper: 20,
      Primal: 20,
      Dual: -1.64583
    },
    {
      Index: 1,
      Status: 'UB',
      Lower: -Infinity,
      Upper: 30,
      Primal: 30,
      Dual: -1.35417
    },
    {
      Index: 2,
      Status: 'FX',
      Lower: 0,
      Upper: 0,
      Primal: 0,
      Dual: -4.41667
    }
  ]
});
```

## Demo

See the online demo at: https://lovasoa.github.io/highs-js/