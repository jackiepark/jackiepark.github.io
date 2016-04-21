---
layout: post
title: esnext variable
---

## let

> block scoped 를 지원하는 variable

```js
{
  let a = 1;
  {
    let a = 2;
    console.log(a); // 2
  }
  console.log(a); // 1
}
```

> global let 은 window 객체에서 접근할 수 없습니다

```js
let a = 1;
console.log(window.a); // undefined
```

> 여러개의 for 문 작성시 i, j, k ... 으로 초기 인자를 바꿀 필요가 없어졌습니다.(temparal variable의 중복사용이 가능)

```js
for(let i = 0; i < 5; i++) {}
for(let i = 0; i < 5; i++) {}
for(let i = 0; i < 5; i++) {}
```

> variable 을 선언전에 사용할 경우 error 를 발생 시킵니다.
선언전까지의 블록 영역을 tempral dead zone 이라 부릅니다. (오늘 기준으로 아직 babel + core-js 에서 미구현 상태입니다.)
따라서 블록상단에 선언문을 모아주는 것이 좋습니다.

```js
console.log(a); // ReferenceError
let a = 1;
```

> var 는 function scope 를 가지므로 let 으로 변경시 주의합니다.

```js
function f() {
  {
    var a = 1;
  }
  console.log(a); // 1
}

function f() {
  {
    let a = 1;
  }
  console.log(a); // ReferenceError
}
```

> 같은 block 에서 반복해서 선언할 수 없습니다
예방을 위해 module export 를 사용합니다

```js
{
  let a = 1;
  let a = 2; // x
}
function f(a) {
  let a; // Duplicate declaration "a"
}


```

## const

> 역시 block scope 를 지원하는 variable 입니다.
let 과의 차이점은 선언 시점에만 값을 할당할 수 있다는 것입니다. (single assignment)

```js
const a = 1;
a = 2; // SyntaxError
```

> 자신의 reference 는 바꿀 수 없지만 할당된 mutable object 내부의 value 는 바꿀 수 있습니다

```js
const a = {a:1, b:2};
a.a = 2;
console.log(a.a) // 2
```
> Object.freeze 를 사용하여 immutable 상태로 만들 수 있습니다

```js
const a = Object.freeze({a:1, b:2});
a.a = 2; // x
```

> Object.freeze 를 사용해도 object 내부의 object 는 변경이 가능합니다

```js
const a = Object.freeze({b: {c: 1}});
a.b.c = 2;
console.log(a.b.c); // 2
```

> 대안으로 Immutable.js 를 사용하는 방법이 있습니다.

```js
import Immutable = require('immutable');
var nested = Immutable.fromJS({a:{b:{c:[3,4,5]}}});
// Map { a: Map { b: Map { c: List [ 3, 4, 5 ] } } }
```
