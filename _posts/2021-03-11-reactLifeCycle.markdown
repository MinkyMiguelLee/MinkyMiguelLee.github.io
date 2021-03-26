---
layout: post
title:  "React - LifeCycle"
date:   2021-03-10 15:44:00 +0100
categories: "study"
---

# React의 LifeCycle

![LifeCycle](../../../../assets/images/lifeCycle.png){ width=50% }

위 이미지에 표현된 각각의 박스들은 모두 일종의 함수이다.


## Mounting : 컴포넌트가 브라우저 상에 나타나는 것
• constructor : 생성자 함수. 컴포넌트가 처음 브라우저에 나타날 때, 만들어지는 과정에서 가장 처음 실행되는 함수. 
초기값(component가 가지고 있을 state 등...)을 세팅한다던지 컴포넌트 생성 과정에서 선행되어야 하는 작업이 있다면 constructor에서 수행한다.
```
constructor(props){
    super(props);
    ...
}

```

• getDerivedStateFromProps : Props로 받은 값을 State에 동기화시키고 싶을 때 사용. 
Mounting(새로운 컴포넌트 생성) 과정에서도 사용되고, props가 바뀔 때에도 사용된다. static 형태로 선언해야 함.

• render : 만들 Dom과 Tag등을 이 곳에 정의한다.

• componentDidMount : 생성한 컴포넌트가 성공적으로 브라우저 상에 나타났을 때 호출된다.
컴포넌트가 브라우저에 나타난 시점에 무언가 작업이 필요하다면 이 곳에 명시한다. 이벤트 리스닝, API 요청, 외부 라이브러리 연동,
컴포넌트에서 필요한 데이터 요청, DOM 관련 작업을 수행한다.


## Updating : 컴포넌트의 props나 State가 변경된 것
• shouldComponentUpdate : 컴포넌트가 업데이트되는 성능을 최적화하고자 할 때 사용한다. 
만약 부모 컴포넌트가 re-render되면, 그 부모 컴포넌트의 자식 컴포넌트들도 
모두 render 함수를 수행하게 되는데, 이러한 작업이 불필요하다면, 불필요한 컴포넌트를 virtual dom에
그리는것조차 자원 낭비이므로(바뀐 게 없다면, virtual dom에만 그리고 실제 화면엔 그리지 않는다),
성능 최적화를 위해 render를 하지 않도록 한다. (true-render/false-not render)

• getSnapshotBeforeUpdate : rendering 결과가 브라우저상에 반영되기 바로 직전에 호출되는 함수.
주로 rendering 후, 스크롤의 위치, dom의 크기 등을 사전에 가져오고자 할 때 사용된다.

• componentDidUpdate : Updating 작업 후, component가 update 완료되었을 때 호출되는 함수.


## Unmounting : 컴포넌트가 브라우저 상에서 사라지는 것
• componentWillUnmount : componentDidMount에서 선언한 이벤트 리스너를 없애는 작업 등을 수행함.




