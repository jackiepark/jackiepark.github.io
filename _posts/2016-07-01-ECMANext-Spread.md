---
layout: post
title: esnext spread
---

## jquery extend

### copy & merge

```js
const x = {a:1}
const z = $.extend({}, x) // {a:1}

const x = {a:1}
const y = {b:1}
const z = $.extend(x, y) // z = {a:1, b:1}, x = {a:1, b:1} 
```

## underscore

### copy & merge

```js
const x = {a:1}
const z = _.extend({}, x) // {a:1}

const x = {a:1}
const y = {b:1}
const z = _.extend(x, y) // z = {a:1, b:1}, x = {a:1, b:1} 
```

## object assign

### copy

```js
const x = {a:1}
const z = Object.assign({}, x) // {a:1} // copy only its ownproperty


const x = {a:1}
const y = {b:1}
const z = Object.assign({}, x, y) // z = {a:1, b:1}
```

### merge

```js
const x = {a:1}
const y = {b:1}
const z = Object.assign(x, y) // {a:1, b:1}

const x = {a:1}
const y = {a:2}
const z = Object.assign(x, y) // z = {a:2}, x = {a:1}

const x = {a: {a:1}}
const y = {a: {b:2}}
const z = Object.assign(x, y) // z = {a:{b:2}}, x = {a:{b:2}}
```

## spread

### copy

```js
const x = {a:1}
const y = {b:1}
const z = {...x, ...y} // z = {a:1, b:1} // copy only its ownproperty
const _z = { { __proto__: x.__proto__ }, x} // copy like extend

var _extends = Object.assign || 
  function (target) {
    for (var i = 1; i < arguments.length; i++) { 
      var source = arguments[i]
      for (var key in source) {
        if (Object.prototype.hasOwnProperty.call(source, key)) { 
        target[key] = source[key]
      }
    } 
  }
  return target
}

const z = _extends({}, x, y)
```

```js
let x = [1];
let y = [2];
let z = [3];

let a = x.concat(y,z)
let a = [...x, ...y, ...z]

x.push(y) // [1, Array[1]]
x.push(2) // [1,2]
Array.prototype.push.apply(x, y) // [1,2]
```

### apply

```js
Math.max.apply(null, [1,2,3])
Math.max(...[1,2,3])
```
