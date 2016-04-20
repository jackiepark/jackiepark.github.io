---
layout: post
title: esnext destructuring
---

## Destructuring

> array 나 object 에서 data 를 추출하여 원하는 변수에 할당하는 문법

```js

let a, b, rest;
[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2

[a, b, ...rest] = [1, 2, 3, 4, 5];
console.log(a); // 1
console.log(b); // 2
console.log(rest); // [3, 4, 5]

[a, , b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // 3

```

> 변수 swapping 이 간편해졌습니다

```js

[a, b] = [1, 2];
[b, a] = [a, b];
console.log(a); // 2
console.log(b); // 1

// array 와의 교환도 가능합니다.
[a, ...rest] = [1, 2, 3];
[rest, a] = [a, rest];
console.log(a); // [2, 3]
console.log(rest); // 1

{a, b} = {a:1, b:2};
console.log(a); // 1
console.log(b); // 2

// object 에 없는 요소를 선언해도 에러는 나지 않습니다.
let {c, d} = {a:1, b:2};
console.log(c); // undefined
console.log(d); // undefined

// destructuring 시에 let, const, var 혹은 () 를 사용해야 합니다.
let {g} = {g:1}; // ok
({g} = {g:1}) // ok
{g} = {g:1} // error

```

> function 을 이용한 destructuring 도 가능합니다

```js

function f() {
  return [1, 2];
}

[a, b] = f();
console.log(a); // 1
console.log(b); // 2

// 새로운 이름으로 할당이 가능합니다.
let {s: t, u: v} = {s: 1, u: 2};
console.log(s); // undefined
console.log(u); // undefined
console.log(t); // 1
console.log(v); // 2

// default value 를 지정할 수 있습니다
let {a=10, b=20} = {a: 1};
console.log(a); // 1
console.log(b); // 20

```

> module export import 에 활용합니다.

```js

// export.js
export function a() {}
export function b() {}
// import1.js
import {a, b} from './export'
a();
b();
// imoprt2.js (alias 사용이 가능)
imoprt * as e from './export'
e.a();
e.b();

// default export (module 당 하나만 허용)
// export.js
function a() {console.log('a')}
function b() {console.log('b')}
function c() {console.log('c')}
export default {a, b, c}
// import1.js
import i from './export'
i.a(); // a
i.b(); // b
i.c(); // c
// import2.js
import {a, b, c} from './export'
a(); // a
b(); // b
c(); // c

// mix default export and export(많이 사용할 모듈을 default 로 지정)
// export.js
export default function() {console.log('a')}
export function b() {console.log('b')}
// import1.js
import a, {b} from './export';
a(); // a
b(); // b

// export.js
export default function() {console.log('a')}
export function b() {console.log('b')}
export function c() {console.log('c')}
// import1.js
import a, * as m from './export';
a(); // a
m.b(); // b
m.c(); // c

```