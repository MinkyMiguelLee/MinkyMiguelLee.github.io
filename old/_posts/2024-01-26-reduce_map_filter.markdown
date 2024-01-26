---
layout: post
title: "reduce / map / filter"
date: 2024-01-25 10:43:00 +0100
categories:
---

# reduce / map / filter

### for문

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

for (let i = 0; i < practice.length; i++) {
  console.log(practice[i].name);
}
```

### while문

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

let i = 0;
while (i < practice.length) {
  console.log(practice[i].name);
  i++;
}
```

- while문도 마찬가지로 i를 활용하여 카운팅을 하고 practice 길이만큼만 반복하게 하였다.

### forEach

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

practice.forEach((data) => console.log(data.name));
```

- forEach는 아주 단순한 반복문으로 별다른 부가기능은 없다는 특징이 있다.
- 가장 기본적인 반복문이다.
- 그렇지만 for문보다 코드가 더 예뻐 보인다.
- 그 이유는 아래와 같다!
  - 임시 변수(i)를 사용하지 않아도 된다.
  - 길이를 따로 설정하지 않아도 된다.

### map

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

const practiceNames = practice.map((data) => data.name);

console.log(practiceNames);
```

- map은 어떤 배열을 다른 형태의 배열로 재생산할 때 사용된다.
- 기존 배열을 수정하는 것이 아니라 새 배열을 반환하는 것이다!
- map은 배열의재탄생을 도와준다.

### filter

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

const practice220 = practice.filter((data) => data.value < 220);

console.log(practice220);
```

- filter는 배열 안에서 특정 조건을 가진 요소만 뽑아내는 반복문이다.
- filter도 map과 같이필터링을 통과한 값에 대해 새 배열을 반환한다.
- 위 코드는 value의 값이 220 미만인 값에 대해 필터링을 진행 후 필터를 통과한 값으로 이루어진 배열을 반환하였다.
- 검색 기능에서 많이 활용된다.

### reduce

```
const practice = [
  { name: "개발자", value: 150, active: false },
  { name: "퉁이리", value: 200, active: true },
  { name: "프론트엔드", value: 110, active: true },
  { name: "티스토리", value: 300, active: true },
  { name: "깃허브", value: 250, active: true },
];

const practiceSum = practice.reduce((pre, cur) => pre + cur.value, 0);

console.log(practiceSum);
```

- practice에 있는 모든 value 값들의 합을 구하고 싶을 때, reduce를 사용한다.
- 누적 합을 구할 때 reduce를 많이 사용한다.
- pre는 누산 값 cur은 현재 값 0은 초기 값
- reduce 메서드 역시 하나의 결괏값을반환한다.
