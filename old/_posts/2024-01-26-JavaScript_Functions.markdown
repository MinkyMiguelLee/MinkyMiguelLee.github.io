---
layout: post
title: "JavaScript - 중요 Function 모음"
date: 2024-01-25 10:41:00 +0100
categories:
---

# JavaScript - 중요 Function 모음

# 배열 중복 제거

## **1. Set를 이용하여 중복 제거**

Set은 중복 데이터 저장을 허용하지 않는 자료구조이다. 이 특성을 이용하여 배열의 모든 요소를 Set에 추가하면 중복이 저절로 제거가 된다. `new Set(arr)`는 배열의 데이터가 추가된 Set 객체가 생성되며, 중복은 허용되지 않기 때문에 1개의 요소만 추가된다. Set를 Array로 변경할 때는 [Spread operator](https://codechacha.com/ko/javascript-concat-array/)를 이용하면 `[...set]`처럼 간단하게 변환할 수 있다.

```
const arr = ['A', 'B', 'C', 'A', 'B'];

const set = new Set(arr);
const newArr = [...set];
console.log(newArr)
```

Output:

`[ 'A', 'B', 'C' ]`

Set를 Array로 변환할 때 아래와 같이 `Array.from()`을 이용할 수도 있다.

```
const arr = ['A', 'B', 'C', 'A', 'B'];
const newArr = Array.from(new Set(arr));
console.log(newArr)
```

## **2. filter(), indexOf()를 이용하여 중복 제거**

`filter()`와 `indexOf()`를 사용하여 중복을 제거할 수도 있다. `indexOf(element)`는 배열에서 요소의 Index를 리턴하는데, 중복된 값이 있을 때 낮은 Index를 리턴한다. 따라서 `indexOf(element) === index`가 true가 되는 경우는 하나 뿐이고, 다른 중복된 값은 false가 리턴되여 `filter()`에 의해 필터링된다. 참고로, `filter(lambda)`는 lambda가 true일 때 그 요소를 결과에 포함시킨다.

```
const arr = ['A', 'B', 'C', 'A', 'B'];

const newArr = arr.filter((element, index) => arr.indexOf(element) === index);
console.log(newArr);
```

Output:

`[ 'A', 'B', 'C' ]`

## **3. reduce()를 이용하여 중복 제거**

`reduce()`는 배열 요소의 값들을 순차적으로 순회하면서 하나의 값을 만드는 함수이다. 초기값 initialValue가 주어지면, 배열의 0번 index부터 연산을 수행하고, 그 결과를 다음 연산의 인자(accumulator)로 전달한다.

아래 예제는 `reduce()`로 중복을 제거하는 방법인데 accumulator에 추가 안된 요소라면 추가하고, 그렇지 않으면 추가하지 않다. 이것을 첫번째 요소부터 마지막 요소까지 순회하면서 수행한다.

```
const arr = ['A', 'B', 'C', 'A', 'B'];

const initialValue = []
const newArr = arr.reduce((acc, obj) => acc.includes(obj) ? acc : [...acc, obj], initialValue)
console.log(newArr)
```

Output:

`[ 'A', 'B', 'C' ]`

## **4. for 루프를 이용하여 중복 제거**

반복문을 사용하여 모든 요소를 순회하고 중복된 값은 제거하도록 직접 구현할 수 있다. 아래 예제는 forEach를 사용하여 구현하였다.

```
const arr = ['A', 'B', 'C', 'A', 'B'];

const newArr = [];
arr.forEach((element) => {
  if (!newArr.includes(element)) {
    newArr.push(element);
  }
});
console.log(newArr);
```

Output:

`[ 'A', 'B', 'C' ]`

---

# 문자열 아스키 코드 관련

1. String.charCodeAt(idx)
2. String.fromCharCode(65, 83, 67, 73, 73); // ASCII

---

# Reduce

### **Reduce 메서드란?**

reduce 메서드는 map, forEach와 비슷하게 배열의 요소들을 순회하면서 반복적인 연산을 하는 메서드이지만, map과 forEach와는 조금 다른 부분들이 있다.

### **문법**

```
// reduce
const numbers = [1, 2, 3, 4];

numbers.reduce((누산값, 현재요소값, 현재요소의index, 현재배열) => {
  return 다음누산값;
}, 초기누산값);
```

### **콜백 함수**

- 첫 번째는 map과 forEach처럼 콜백 함수를 전달받는데, 이 콜백 함수의 파라미터가 map, forEach와 조금 다르다.
- reduce 메서드가 특별한 이유는 바로 이 콜백 함수의 첫 번째 파라미터인데, reduce 메서드에서 이 **콜백 함수가 동작할 때 return 하는 값이 다음 콜백 함수의 첫 번째 파라미터로 전달**되는 것이다.
  - 그러고 나서 **마지막 콜백 함수가 동작한 이후의 return 값이 reduce 메서드의 return 값**이 되는 것이다.
  - 나머지 2,3,4번째 파라미터는 map, forEach 메소드의 1,2,3번째 파라미터와 역할이 동일하다.
  - 그렇기 때문에 콜백 함수의 **파라미터 생략은, 3,4번째 파라미터만 가능**하다.

### **참고**

참고로 reduce 메서드에서 초기값이라 불리는 두 번째 파라미터는 생략이 가능하다.

만약 두번째 파라미터를 생략하고 콜백 함수만 전달할 경우, **배열의 첫 번째 요소(0번 index)가 초기값이 되어 동작**하게 된다.

### **참고**

참고로 reduce 메서드에서 초기값이라 불리는 두 번째 파라미터는 생략이 가능하다.

만약 두번째 파라미터를 생략하고 콜백 함수만 전달할 경우, **배열의 첫 번째 요소(0번 index)가 초기값이 되어 동작**하게 된다.

```
const numbers = [1, 2, 3, 4];

const sum = numbers.reduceRight((acc, el, i) => {
  console.log(`index: ${i}`)
  console.log(`acc: ${acc}`);
  console.log(`el: ${el}`);

  return el + acc;
}, 0);

console.log(`-----------`);
console.log(`sum: ${sum}`);
```

### 응용

배열의 평균값을 구하는 데 사용한 코드

```
function calculator(list) {
    let answer = list.reduce((a, b) => a + b, 0) / list.length;
    // 배열의 모든 값을 더해준 후, 배열의 길이로 나누기... 초기값은 0
    return answer;
}
```

---

# 이중 Function

```
function solution(tag){
    return function subSol(text){
        return `<${tag}>${text}</${tag}>`;
    }
}

console.log(solution('h1')('제목이다')) // <h1>제목이다</h1>
```

---
