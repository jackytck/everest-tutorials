# Test

Write tests for sustainability. We use `Jest` in our testing.

---

## install

`babel-jest` allows you to use es6 in your \*.test.js file. Install them if they are not in `package.json`

```
yarn add -D jest babel-jest regenerator-runtime
```

## Create tests

write test scripts under `./test/`, [Reference](https://facebook.github.io/jest/).

A test file example `GeoPose.test.js.`

The first test verifies the class is defined. The second test verifies the class creates no memory leak, and its methods are working as expected.

```js
import GeoPose from '../src/Core/Earth/GeoPose'
import { iterate } from 'leakage' // import { iterate, MemoryLeakError } from 'leakage'

test('Class defined', () => {
  expect(GeoPose).toBeDefined()
})

describe('methods', () => {
  it('Constructor', () => {
    let res = iterate(() => {
      let instance = new GeoPose()
    }, {iterations: 3, gcollections: 3})
    let lastChange = res.heapDiffs[res.heapDiffs.length - 1]['change']
    expect(lastChange['size_bytes']).toBe(0)
  })
  it('set/get/clone/equals', () => {
    let a = new GeoPose({lng: 12.12345})
    expect(a.lng).toBe(12.12345)
    let b = a.clone()
    expect(a.equals(b)).toBeTruthy()
    b.lat = 23.45678
    expect(a.equals(b)).not.toBeTruthy()
    expect(b.lat).toBe(23.45678)
  })
})
```

## Run tests

```
node_modules/.bin/jest
```

If test include memory heap detector, must use  single thread

```
node_modules/.bin/jest --runInBand
```

---

learning material

* [getting started](https://facebook.github.io/jest/docs/en/getting-started.html)
* [Setup & Writing Our First Jest Test - JS Testing 101 with Jest](https://www.youtube.com/watch?v=4kNfeI37xu4)

* [Testing JavaScript Code with Jest](https://medium.com/piecesofcode/testing-javascript-code-with-jest-18a398888838)



