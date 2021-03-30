---
layout: post
title:  "React - Functional Style VS class style"
date:   2021-03-10 15:44:00 +0100
categories:
---

# functional style과 class style
&nbsp;
  • 기존의 리액트에서, class style은 구현이 복잡하고 신경써야 할 부분이 더 많지만 lifecycle 처리, state 조작 등 더욱 상세한 처리가 가능했고, functional style은 구현이 간단하지만 static과 lifecycle 처리가 불가능해 단순한 값 전달을 수행하는 컴포넌트에만 사용했다.
  • 리액트에 Hook 개념이 등장하면서 기존의 class style과 functional style이 대등하게 사용될 수 있게 되었다.
    기존의 Functional style은 state를 사용할 수 없었고, 컴포넌트의 lifecycle 처리가 불가능했다. 하지만 
    현재는 Hook을 통해 이러한 조작이 가능하게 되었다.
  • class style의 경우, 컴포넌트에서 render() 함수를 정의하여 리턴해주어야 한다.
  • functional style의 경우, 컴포넌트 그 자체가 render()함수의 역할을 한다.
  • 컴포넌트를 이용하는 쪽에서는 props를 통해 컴포넌트를 이용하고, 만드는 쪽에서는 state를 통해 내부 조작을 수행한다.
  • 함수형 컴포넌트를 리액트가 호출할 때, 첫 번째 파라미터로 props값을 준다.
  • 함수형 컴포넌트에서 state를 만들고 싶다면, react의 기본 제공 Hook인 useState()에 파라미터로 어떠한 값을 전달하면 그 값이 state의 초기값이 된다. 그렇게 하여 리턴된 값의 첫 번째 자리(useState의 리턴값은 무조건 배열 형태이고, 첫 번째 값이 해당하는 값이 된다.)가 그 값이 된다.
  • useState()의 리턴값인 배열의 두 번째 값이 상태를 바꾸는 함수이다. 이 함수의 첫 번째 인자로 해당 state에 넣고자 하는 값을 전달하면, state의 값이 변경되고 새로 render된다.


