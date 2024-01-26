---
layout: post
title: "React - LifeCycle"
date: 2021-03-10 15:44:00 +0100
categories: ["study", "React.js"]
---

![LifeCycle](../../../../../assets/images/lifeCycle.png)

위 이미지에 표현된 각각의 박스들은 모두 일종의 함수이다.

---

## Mounting : 컴포넌트가 브라우저 상에 나타나는 것

&nbsp;
&nbsp;
• **constructor** : 생성자 함수. 컴포넌트가 처음 브라우저에 나타날 때, 만들어지는 과정에서
가장 처음 실행되는 함수.
초기값(component가 가지고 있을 state 등...)을 세팅한다던지 컴포넌트
생성 과정에서 선행되어야 하는 작업이 있다면 constructor에서 수행한다.

```js
  constructor(props){
    super(props);
    // 컴포넌트 생성 시 가장 먼저 수행하고자 하는 작업 수행
    ...
  }
```

• **getDerivedStateFromProps** : Props로 받은 값을 State에 동기화시키고 싶을 때 사용.
Mounting(새로운 컴포넌트 생성) 과정에서도 사용되고,
props가 바뀔 때에도 사용된다. static 형태로 선언해야 함.

```js
  static getDerivedStateFromProps(nextProps, prevState){
    // nextProps : state에 넣어줄 props 값
    // prevState : 업데이트 되기 전의, 현재 state

    if(prevState.value !== nextProps.value){
      return{
        value: nextProps.value;
      }
    }else{
      return null;
    }
  }
```

• **render** : 만들 Dom과 Tag등을 이 곳에 정의한다.
&nbsp;
• **componentDidMount** : 생성한 컴포넌트가 성공적으로 브라우저 상에 나타났을 때 호출된다.
컴포넌트가 브라우저에 나타난 시점에 무언가 작업이 필요하다면 이 곳에
명시한다. 이벤트 리스닝, API 요청, 외부 라이브러리 연동,
컴포넌트에서 필요한 데이터 요청, DOM 관련 작업을 수행한다.

```js
  componentDidMount(){
    ...
  }
```

---

## Updating : 컴포넌트의 props나 State가 변경된 것

&nbsp;
&nbsp;
• **shouldComponentUpdate** : 컴포넌트가 업데이트되는 성능을 최적화하고자 할 때 사용한다.
만약 부모 컴포넌트가 re-render되면, 그 부모 컴포넌트의 자식
컴포넌트들도 모두 render 함수를 수행하게 되는데, 이러한 작업이
불필요하다면, 불필요한 컴포넌트를 virtual dom에 그리는것조차
자원 낭비이므로(바뀐 게 없다면, virtual dom에만 그리고 실제
화면엔 그리지 않는다),
성능 최적화를 위해 render를 하지 않도록 한다.
(true-render/false-not render)

```js
  shouldComponentUpdate(nextProps, nextState){
    // 이 함수를 별도로 설정해주지 않는다면, 기본적으로 return true;
    return true;
  }
```

• **getSnapshotBeforeUpdate** : 컴포넌트 업데이트 후, rendering 결과가 브라우저상에 반영되기
바로 직전에 호출되는 함수.업데이트 되기 바로 직전의 dom 상태를
return 하여, 그 return값을 componentDidUpdate에서
받아오도록 할 수 있다.
주로 rendering 후, 스크롤의 위치, dom의 크기 등을 사전에
가져오고자 할 때 사용된다.

```js
  getSnapshotBeforeUpdate(prevProps, prevState){
    // 이전의 상태값들을 받아오므로, 이전 상태값이 필요하다면 활용 가능하다.
    // 여기서 리턴한 값을 componentDidupdate에서 snapshot이라는 parameter로
    // 받아서 사용할 수 있다.
  }
```

• **componentDidUpdate** : Updating 작업 후, component가 update 완료된 후 호출되는 함수.

```js
  componentDidUpdate(prevProps, prevState, snapshot){
    // 업데이트 이전 값을 받아올 수 있다.
    // 예를 들어, 특정 props 값이 변경되었을 때 이를 확인하여 작업을 수행할 수 있다.
  }
```

---

## Unmounting : 컴포넌트가 브라우저 상에서 사라지는 것

&nbsp;
&nbsp;
• **componentWillUnmount** : componentDidMount에서 선언한 이벤트 리스너를 없애는 작업 등을 수행

```js
  componentWillUnmount(){
    // 컴포넌트가 사라질 때 수행한다. 특정 조건일 때 이 함수를 호출하여 특정 작업을
    // 수행하도록 할 수 있다.
  }
```

---

## on Error : 컴포넌트에 에러가 발생했을 때 유용하게 사용 가능한 API!

&nbsp;
&nbsp;
• **componentDidCatch** : React 함수에서 에러 발생 시, 리액트 앱이 크래쉬된다. 이러한 상황에
componentDidCatch를 사용하여 크래쉬를 방지하고 특정 작업을
수행할 수 있다.

```js
  componentDidCatch(error, info){
    // error : 어떤 에러가 발생했는지
    // info : 에러 발생 위치 등 정보...
  }
```
