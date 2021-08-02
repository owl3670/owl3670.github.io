---
layout: single
title: '[Javascript] 자바스크립트 엄격모드 (Use Strict)'

categories:
  - Javascript

toc: true
toc_sticky: true
toc_label: 'Index'
toc_icon: 'list'
---

자바스크립트는 굉장히 유연한 언어로 그에따른 여러 단점도 여럿 존재한다.  
때문에 자바스크립트를 좀 더 엄격하게 사용하여 실수를 방지하고 싶은 개발자들을 위해 **ECMAScript 5** 에서 부터
**use strict** 라는 기능을 제공하게 되었다.

해당 기능을 사용하면 다음과 같은 변경들이 일어나게 된다.

1. 무시되던 에러들이 throw 되게 된다.
2. Javascript 엔진이 최적하 하기 어렵게 만드는 실수를 방지 할 수 있다.
3. 차기 ECMAScript 에서 정의될 문법이 사용되지 않을 수 있게 할 수 있다.

### 기본 사용법

엄격모드를 적용하기 위해 use strict 를 구문 작성 전에 삽입하면 된다.

## 스크립트에 적용

```javascript
'use strict';

const a = 0;
```

## 함수에만 적용

```javascript
function sum(a, b) {
  'use strict';

  return a + b;
}
```

## 모듈

ECMAScript 2015 에서 등장한 모듈은 별도의 구문 없이도 자동으로 엄격모드가 된다.

```javascript
function sum(a, b) {
  return a + b;
}

export default sum;
```

### 엄격모드시의 동작

## 에러 발생

엄격모드가 아닐시에는 무시되던 작성자의 실수 등을 에러로 발생시키게 된다.

```javascript
'use strict';

// 선언되지 않은 변수의 사용 (변수명 입력 실수 방지됨)
let variable = 3;
variable33 = 5; //ReferenceError 발생

// 쓸수 없는 전역변수 변수명에 할당
let undefined = 5; // TypeError 발생
let Infinity = 5; // TypeError 발생

// readonly 프로퍼티에 할당
let obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 9; // TypeError 발생

// getter-only 프로퍼티에 할당
let obj2 = {
  get x() {
    return 17;
  },
};
obj2.x = 5; // TypeError 발생

// 확장 불가 객체에 새 프로퍼티 할당
let fixed = {};
Object.preventExtensions(fixed);
fixed.newProp = 'ohai'; // TypeError 발생

// 삭제 할 수 없는 프로퍼티를 삭제하려 시도
delete Object.prototype; // TypeError 발생

// parameter 명의 중복 사용
function sum(a, a, c) {
  // SyntaxError 발생
  return a + b + c; // 코드가 실행되면 잘못된 것임
}

// 8진수 구문을 사용
let sum =
  015 + // SyntaxError 발생
  197 +
  142;

// Primitive 값에 프로퍼티 설정 금지
(function () {
  false.true = ''; // TypeError 발생
  (14).sailing = 'home'; // TypeError 발생
  'with'.you = 'far away'; // TypeError 발생
})();
```

## 변수 사용 단순화

엄격모드는 변수 이름의 맵핑을 단순화 하여 컴파일러의 최적화를 도모할 수 있다.

```javascript
'use strict';

// with 구문을 사용 할 수 없게 만듬
let x = 17;
with (obj) {
  // SyntaxError 발생
  x;
  // x 가 전역변수 x인지 obj 객체안의 x인지
  // 코드를 실행 하기 전까지는 알 수 없다는
  // 이유로 인해 최적화 할 수 없으므로
  // 엄격모드에서는 with의 사용을 막는다
}

// eval 이 새로운 변수를 주변 scope에 추가하지 않는다.
let a = 17;
let evalA = eval('let a = 42; a');
console.log(a === 17);
console.log(evalA === 42);
```
