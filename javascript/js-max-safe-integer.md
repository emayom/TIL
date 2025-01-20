---
title: 자바스크립트에서 안전한 최대 정수 값은 얼마일까?
category: JavaScript
tags:
  - JavaScript
  - max safe integer
date: '2024-02-06T18:16:10.000Z'
last_modified_at: '2025-01-20T16:13:43.089Z'
---

# 자바스크립트에서 안전한 최대 정수 값은 얼마일까?

### 정수에서의 안전함

자바스크립트에서 **"안전한"** 최대 정수의 값은 얼마일까?  
여기서의 안전함이란 정수를 정확하고 올바르게 비교할 수 있음을 의미한다. 

자바스크립트의 `Number` 자료형은 ECMAScript는 [IEEE 754](https://en.wikipedia.org/wiki/IEEE_754)에 기술된 배정밀도 부동소숫점 형식 숫자 체계를 사용한다.  
따라서, -(2<sup>53</sup>- 1)과 2<sup>53</sup> - 1 사이의 숫자만 안전하게 표현할 수 있으며 범위를 벗어나면, 더 이상 정수를 안전하게 표시할 수 없다. 

### 안전한 최대 정수 값 확인하기

`Number`로 사용 가능한 최대 정수 값은 Number의 정적 속성 [Number.MAX_SAFE_INTEGER](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/MAX_SAFE_INTEGER)을 통해 확인할 수 있으며,  
전달된 정수의 값이 2<sup>53</sup> - 1을 초과하지 않는 안전한 값인지 확인하기 위해서는 [Number.isSafeInteger()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Number/isSafeInteger) 매서드를 사용하여 확인할 수 있다. 


```js
const x = Number.MAX_SAFE_INTEGER + 1;
const y = Number.MAX_SAFE_INTEGER + 2;

console.log(Number.isSafeInteger(x));   // false
console.log(Number.isSafeInteger(y));   // false

console.log(Number.MAX_SAFE_INTEGER);   // 9007199254740991
console.log(x === y);                   // true
```

`Number.MAX_SAFE_INTEGER`보다 큰 값은 실제로는 `Number.MAX_SAFE_INTEGER`와 같은 값으로 취급되어 정확한 비교를 할 수 없다.  

### BigInt를 통한 큰 정수 처리

자바스크립트에서 `Number`의 최댓값인 2<sup>53</sup> - 1. 보다 큰 정수 숫자를 다뤄야 한다면 명시적으로 `BigInt`를 사용해야 한다.  
`BigInt`는 `Number` 자료형과는 달리 2<sup>53</sup> - 1 보다 큰 정수를 안전하게 처리할 수 있다.

```js
const theBiggestInt = 9007199254740991n;

const alsoHuge = BigInt(9007199254740991); // 9007199254740991n
```

`BigInt`를 `Number`로 변환하는 과정에서 정확도를 유실할 수 있으므로, 2<sup>53</sup>보다 큰 값을 예상할 수 있는 경우에는 `BigInt`만 사용하는 것이 좋다.
