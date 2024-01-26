---
layout: post
title: "ECMAScript"
date: 2024-01-25 10:34:00 +0100
categories: ["study", "javaScript"]
---

## ES6

- let
  - 변수 선언시 let으로 선언
  - 블록 범위로 선언됨
- const
  - 블록 범위 상수 선언
  - 변경 불가능
- Arrow function
  - 기존 함수 선언을 화살표 선언을 통해 간단하게 선언
  - 화살표 함수
    1. function 키워드 생략 가능
    2. 함수 매개변수가 한개라면 괄호 생략 가능
    3. 매개변수가 없는 함수는 괄호가 필요함
    4. 함수 바디가 표현식 하나라면 중괄호와 return문 생략 가능
  - 제한 사항
    1. this 나 super 바인딩이 없고, method로 사용 불가
       1. 메소드란 객체와 관련된 함수로써, 데이터와 멤버 변수에 대한 접근 권한을 가진다.
       2. 객체의 메소드로 호출할 경우 this는 그 메소드를 포함하는 객체에 바인딩된다. 하지만 화살표 함수는 this를 바인딩하지 않으므로 둘러싸인 스코프에서 this의 값을 찾을 수 없다. 따라서 메소드로 사용하는것은 적합하지 않다.
    2. [new.target](http://new.target/) 키워드가 없음
    3. 일반적으로 스코프를 지정할 때 사용하는 call, apply, bind메소드 이용 불가
    4. 생성자로 사용 불가
    5. yield를 화살표 함수 내에서 사용 불가
  - 장점
    1. 짧은 함수 작성 가능
    2. 자신만의 this를 생성하지 않고 자신을 포함하는 context의 this를 이어받음 → lexical scoping
- for-of 반복문
  - 반복 가능한 객체에 사용 가능한 반복문
  - 배열, 문자열, 맵, 리스트, 객체 등의 자료형에 대해 사용 가능
    ```js
    for (variable of iterable) {
      // code block to be executed
    }
    ```
  - for-in은 variable에 해당 반복의 인덱스가 할당되는 반면, for-of는 해당 반복의 실제 값이 들어가게 된다.
- Map 객체
  - 키-값 쌍을 가지는 객체
  - 맵은 키가 삽입된 순서를 기억하고 있음
- Set 객체
  - Unique 값만을 가질 수 있는 객체
  - 각 값은 Set 내부에 단 하나만 존재할 수 있음
  - Set은 어떤 데이터 타입의 어떤 값이던 가질 수 있음
  - Set은 별도의 key값을 가지지 않기 때문에, keys() 메소드를 호출하여도 values() 메소드 호출과 동일한 값을 리턴함.
    - 동일한 이유로 entries() 메소드 호출시 [키, 값] 쌍이 아닌 [값, 값] 쌍을 리턴함. 이 형식으로 리턴함으로써 Map 객체와 쉽게 비교할 수 있음
- class
  - 자바스크립트 객체의 템플릿
  - 항상 constructor() 를 가져야 한다.
  - class는 객체가 아니고 객체의 템플릿일 뿐이다.
- Promise

  - 프로미스?

    1. producing code와 consuming code를 연결하는 js의 객체이다.
       1. producing code의 수행이 완료될 때까지 consuming code는 대기해야 한다.
       2. producing code가 fulfill되어 then()을 호출하면, 비로소 consuming 코드가 then() 내부 로직에 따라 수행된다.
    2. callback 중첩으로 인한 callback hell을 해결하기 위해 Promise 등장
    3. 비동기 작업과 그 결과를 갖고 해야 할 일을 캡슐화한 객체로서, 작업 완료 시 이 객체에 캡슐화한 콜백을 호출한다.
    4. 프로미스의 네 가지 상태

       ![PROMISE STATE](../../../../../assets/images/ECMA.png)

       1. 프로미스가 fulfill되면, then() 호출
       2. 프로미스가 reject되면, catch() 호출

  - 프로미스 체인이란?
    1. 여러 개의 프로미스를 then()으로 연결하여 사용

- Symbol type

  - js의 데이터타입인 심볼은 Number, String, Boolean과 같이 원시 데이터 타입이다.
    - 원시 타입 : 객체도, 메서드도 아닌 타입
  - 심볼 생성

  ```js
  const sym = Symbol();

  // 심볼 생성시, 구분하기 위한 데이터를 추가하여 생성할 수도 있다.
  const sym = Symbol(data);
  ```

  - 심볼은 동일한 값을 인자로 하여 생성하더라도 값은 같지 않다.

    - 심볼로부터 반환되는 값은 항상 고유하다
    - 동일한 심볼을 사용하고 싶다면

    ```js
    const s1 = Symbol();
    const s2 = s1;

    // 또는, Symbol.for() 메서드를 통해 생성한다

    const s1 = Symbol.for("val");
    const s2 = Symbol.for("val");
    ```

  - 일반적인 원시 데이터 타입과는 다르게, 심볼은 new연산자를 통해 Wrapper를 생성할 수 없다.
  - 심볼(Symbol)은 프로그램이 이름 충돌의 위험 없이 속성(property)의 키(key)로 쓰기 위해 생성하고 사용할 수 있는 값이다.
    - 문자열 값이나 넘버 값처럼, 심볼 값도 속성(property)의 키(key)로 사용할 수 있다. 심볼 값은 다른 어떤 값과도 다르기 때문에, 심볼을 키로 갖는 속성은 다른 어떤 속성과도 충돌되지 않을 것을 보장 받는다.
    - 배열의 요소처럼, 심볼을 키로 갖는 속성(symbol-keyed property)은 [obj.name](http://obj.name/) 같이 점(dot)을 사용해서 접근할 수 없다. 심볼을 키로 갖는 속성은 반드시 꺽쇠 기호를 써서 접근해야 한다.

- 디폴트 파라미터 값
  - ES6에서는 함수의 파라미터에 디폴트 값을 줄 수 있다.
  ```js
  function myFunction(x, y = 10) {
    // y is 10 if not passed or undefined
    return x + y;
  }
  myFunction(5); // will return 15
  ```
- rest parameter

  - rest parameter(...)는 함수에 정의되지 않은 여러 개의 아규먼트를 배열 형태로 받을 수 있도록 해준다.

  ```js
  function sum(...args) {
    let sum = 0;
    for (let arg of args) sum += arg;
    return sum;
  }

  let x = sum(4, 9, 16, 25, 29, 100, 66, 77);
  ```

- String 관련 메소드
  - String.includes() - 메소드의 인자가 문자열에 포함되어있는지 여부 확인
  - String.startsWith() - 메소드의 인자로 문자열이 시작하는지 여부 확인
  - String.endsWith() - 메소드의 인자로 문자열이 끝나는지 여부 확인
- Array 관련 메소드
  - Array.from() - 인자로 반복 가능한 객체나 '길이'개념이 있는 객체가 전달되면, 해당 객체의 요소들로 배열 생성
  - Array.keys() - 배열의 키값을 배열 형태로 반환
  - Array.find() - 인자로 전달한 테스트 함수를 만족하는 배열의 첫 번째 인자를 찾아 반환함.
  - Array.findIndex() - 인자로 전달한 테스트 함수를 만족하는 배열의 첫 번째 인자의 인덱스를 찾아 반환함.
- Math 관련 메소드
  - Math.trunc() - 인자의 정수 부분을 반환한다
  - Math.sign() - 인자가 음수인지, null인지, 양수인지 여부를 반환
  - Math.cbrt() - 인자의 세제곱근 반환
  - Math.log2() - 인자의 base2 알고리즘 수행결과 반환
  - Math.log10() - 인자의 base10 알고리즘 수행결과 반환
- Number 관련 요소
  - Number.EPSILON - 엡실론값
  - Number.MIN_SAFE_INTEGER - 최소 가능 정수
  - Number.MAX_SAFE_INTEGER - 최대 가능 정수
- Number 관련 메소드
  - Number.isInteger() - 정수/실수 여부 확인
  - Number.isSafeInteger() - 인자가 safe number인지 확인
- 글로벌 메소드
  - isFinite() - 유리수인지 확인
  - isNaN() - Number 외의 다른 자료형인지 확인
