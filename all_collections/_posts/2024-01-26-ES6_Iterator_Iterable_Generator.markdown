---
layout: post
title: "ES6 - Iterator Iterable Generator"
date: 2024-01-25 10:36:00 +0100
categories: ["study", "javaScript"]
---

# **Iterator**

Iterator는 `next`메서드를 가지고있는 객체이다. 이 메서드는 순차적으로 원소들을 탐색하며, `next`메서드의 호출시마다 새로운 객체를 반환한다. 반환되는 객체는 `value` 와(또는 = and/or) `done` 프로퍼티를 가지고 있으며, 탐색이 완료될 때 `done`프로퍼티의 값이 true가 된다. 대부분의 Iterator들은 탐색이 완료될때, `value`를 생략한다. —> `return {done: true};`

Iterable은 `Symbol.iterator`라는 메서드를 가지고있는 객체이다. 이 메서드가 Iterator를 반환한다.

### **Iterable/Iterator Example**

피보나치 수열을 생성하는 코드를 작성하였다. `fibonacci`변수가 참조하고 있는 객체는 Iterable이다.

```js
const fibonacci = {
  [Symbol.iterator]() {
    let n1 = 0,
      n2 = 1,
      value;
    return {
      next() {
        value = n1;
        n1 = n2;
        n2 = value + n2;
        // [value, n1, n2] = [n1, n2, n1 + n2]

        if (value > 100) {
          return { done: true };
        } else {
          return { value };
        }
      },
    };
  },
};

for (const n of fibonacci) {
  console.log(n);
}
```

# **Iterable Object**

아래는 기본적인 Iterable 객체/타입들이다.

1. `Array`
2. `Set`
3. `Map` - 원소들을 key/value 쌍으로 탐색한다.
4. DOM `NodeList` - `Node`객체들을 탐색한다.
5. primitive `string` - 각 유니코드별로 탐색한다.

`Array(typed array 포함)`, `Set`, `Map`의 아래 메서드들은 Iterator를 반환한다.

1. `entries` - key,value쌍 [key, value]
2. `keys`
3. `values`

이 메서드들에서 리턴되는 객체는 Iterable이면서 Iterator이다.`Array`에서 key는 인덱스들을 나타내며, `Set`에서는 key,value가 둘 다 value로 같다.

커스텀 객체는 `Symbol.iterator` 메서드를 추가함으로써 iterable이 될 수 있다.

```js
function objectEntries(obj) {
  let index = 0,
    keys = Object.getOwnPropertyNames(obj).concat(
      Object.getOwnPropertySymbols(obj)
    ); // Reflect.ownKeys(obj);

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      if (index === keys.length) {
        return { done: true };
      } else {
        let k = keys[index],
          v = obj[k];
        index += 1;

        return { value: [k, v] };
      }
    },
  };
}

let obj = { foo: 1, bar: 2, baz: 3 };
for (const pair of objectEntries(obj)) {
  // for (const [k, v] of objectEntries(obj))
  console.log(pair[0], "is", pair[1]); // console.log(k, 'is', v);
}
```

# **Iterable Consumers**

다음 문법들은 Iterable을 사용한다.

**for-of loop**

```js
for (const value of someIterable) {
  // ...
}
```

**spread Operator**

```js
// Iterable의 모든  value들이 arr에 추가된다.
let arr = [firstElem, ...someIterable, lastElem];

// Iterable의 모든 value들이 arguments에 추가된다.
someFunction(firstArg, ...someIterable, lastArg);
```

**Positional Destructing**

```js
// Iterable의 첫 3개 값들이 저장된다.
let [a, b, c] = someIterable;
```

# **Generators**

Generator는 Iterable이면서 Iterator인 객체의 특별한 종류이다. 이 객체는 일시정지와 재시작 기능을 여러 반환 포인트들을 통해 사용할 수 있다. 이러한 반환 포인트들은 `yield` 키워드를 통해 구현할 수 있으며, 오직 generator 함수에서만 사용할 수 있다. `next`호출시마다 다음 `yield`의 expression이 반환된다.`yield value`를 사용하면 한가지 값을 반환할 수 있고, `yield* iterable`을 사용하면 해당되는 Iterable의 값들을 순차적으로 반환시킬 수 있다.

Generator의 반복이 끝나는 시점은 3가지 경우인데, generator 함수에서 `return` 사용, 에러 발생 그리고 마지막으로 함수의 끝부분까지 모두 수행된 이후, 이렇게 3가지 경우이다. 그리고 이때 done 프로퍼티가 true가 될 것이다.

이러한 generator함수는 Generator객체를 반환하며, `function*`키워드로 정의할 수 있다. 또는 클래스에서 메서드 이름 앞에 `*`을 붙여 정의할 수도 있다.

# **A Basic Generator**

```js
function* myGenFn() {
  yield 1;
  yield 2;
  return 3;
}

let iterator = myGenFn(),
  capture;

console.log("--------------- do-while loop ---------------");
do {
  capture = iterator.next();
  console.log(capture);
} while (!capture.done);

//또는
console.log("\n--------------- for-of loop ---------------");
for (const n of myGenFn()) {
  console.log(n);
}
```

`return`문을 사용하지 않았다면, 마지막 `next`호출에서 value가 3이 아닌 undefined가 반환되었을 것이다.

# **Fibonacci Generator**

다음은 Generator를 이용한 fibonacci 수열 생성이다.

```js
function* fibonacci() {
  let prev = 0,
    curr = 1; // [prev, curr] = [0, 1];
  yield prev;
  yield curr;
  while (true) {
    let temp = prev;
    prev = curr;
    curr = temp + curr;
    // [prev, curr] = [curr, prev + curr];

    yield curr;
  }
}

for (const n of fibonacci()) {
  if (n > 100) break;
  console.log(n);
}
```

또한 generator 메서드를 포함하고 있는 객체로 구현할 수 있다.

```js
let fib = {
  *[Symbol.iterator]() {
    let prev = 0,
      curr = 1; // [prev, curr] = [0, 1];
    yield prev;
    yield curr;
    while (true) {
      let temp = prev;
      prev = curr;
      curr = temp + curr;
      // [prev, curr] = [curr, prev + curr];

      yield curr;
    }
  },
};

for (const n of fib) {
  if (n > 100) break;
  console.log(n);
}
```

# **Generator Methods**

- `next(value)`이 메서드는 다음 값을 얻는 역할을 하며, Iteraotr의 `next`메서드와 유사하지만, optional argument를 받는다는 점이 다르다.(첫번째 호출에서는 받지 않고 무시한다.) 이 매개변수는 바로 이전의 `yield [expression]`의 반환값으로 사용된다. (아래 예시를 참고.)

```js
function* fibonacci() {
  var fn1 = 1;
  var fn2 = 1;
  while (true) {
    var current = fn2;
    fn2 = fn1;
    fn1 = fn1 + current;

    var reset = yield current;
    console.log("----> reset", reset);
    if (reset) {
      fn1 = 1;
      fn2 = 1;
    }
  }
}

var sequence = fibonacci();
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
console.log(sequence.next().value); // 3
console.log(sequence.next().value); // 5
console.log(sequence.next().value); // 8
console.log(sequence.next(true).value); // reset = true -> yield 1
console.log(sequence.next().value); // 1
console.log(sequence.next().value); // 2
console.log(sequence.next().value); // 3
```

- `return(value)`이 메서드는 매개변수로 온 값을 value로써 반환하고, Generator를 종료시킨다.`{value: value, done: true}`
- `throw(exception)`이 메서드는 인자로 받은 에러 발생시키고, Generator를 종료시킨다. generator함수 내부의 catch 구문을 통해 처리할 수도 있다. 또한 내부에서 catch 구문으로 처리한 경우 Generator는 종료되지 않는다.

# **Summary**

Iterator가 훌륭하다면 Generator는 더욱 훌륭하다. for-of loop, spread operator, destructing을 완벽하게 활용하기 위해서 Iterator와 Generator에 대한 이해가 중요하다.
