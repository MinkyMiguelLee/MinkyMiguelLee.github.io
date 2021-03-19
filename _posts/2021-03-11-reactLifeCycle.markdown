---
layout: post
title:  "React - LifeCycle"
date:   2021-03-10 15:44:00 +0100
categories:
---

# React의 LifeCycle

![LifeCycle](../assets/images/lifeCycle.png)

위 이미지에 표현된 각각의 박스들은 모두 일종의 함수이다.

## Mounting : 컴포넌트가 브라우저 상에 나타나는 것
• constructor : 생성자 함수. 컴포넌트가 처음 브라우저에 나타날 때, 만들어지는 과정에서 가장 처음 실행되는 함수. 
초기값을 세팅한다던지 컴포넌트 생성 과정에서 선행되어야 하는 작업이 있다면 constructor에서 수행한다.
• getDerivedStateFromProps : Props로 받은 값을 State에 동기화시키고 싶을 때 사용. 
Mounting 과정에서도 사용되고, props가 바뀔 때에도 사용된다.
• render : 만들 Dom과 Tag등을 이 곳에 정의한다.
• componentDidMount : 생성한 컴포넌트가 성공적으로 브라우저 상에 나타났을 때 수행된다. 
컴포넌트가 브라우저에 나타난 시점에 무언가 작업이 필요하다면 이 곳에 명시한다. 이벤트 리스닝, API 요청 등을 이 곳에서 수행한다.

## Updating : 컴포넌트의 props나 State가 변경된 것
• shouldComponentUpdate : 컴포넌트가 업데이트되는 성능을 최적화하고자 할 때 사용한다. 
만약 부모 컴포넌트가 업데이트 되었다면, 그 부모 컴포넌트의 자식 컴포넌트들도 
모두 render 함수를 수행하게 되는데, 이러한 작업이 불필요하다면 


## Unmounting : 컴포넌트가 브라우저 상에서 사라지는 것

.
.
.

정리중 업무로 인해 중단




