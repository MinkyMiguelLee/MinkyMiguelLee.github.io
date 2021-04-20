---
layout: post
title:  "React - useEffect vs. useLayoutEffect"
date:   2021-04-20 17:44:00 +0100
categories:
---

# useEffect vs. useLayoutEffect
&nbsp;
&nbsp;
## useEffect
- render : 화면 렌더링 완료 후 비동기적으로 실행
- 사용 경우
  - 일부 상태를 즉시 발생할 필요가 없을 경우
  - 페이지에 시각적으로 영향을 주지 않는 무언가를 동기화 할 경우
  - 이벤트 핸들러를 설정하는 경우
  - 모달 상자가 나타나거나 사라질 때 일부 상태를 재설정하는 경우 
&nbsp;
## useLayoutEffect
- render : 렌더링 후 화면이 업데이트 되기 전에 동기적으로 실행
- 사용 경우
  - 상태가 업데이트 될 때 요소가 깜박이는 경우
  - DOM을 변경하려는 경우
&nbsp;
&nbsp;
- 대부분의 상태 변경, 화면 렌더링 등에 useEffect를 사용하게 되면서, 과도하게 화면이 깜빡이거나 불필요한 렌더링이 수행되는 경우가 늘었다.
- 사용자에게 보여지는 Dom 변경의 경우, useEffect를 사용하면 화면이 깜빡이게 된다.
- 이럴 때 useLayoutEffect를 활용하여 위와 같은 상황을 방지할 수 있다고 들었다.
  - useLayoutEffect내의 함수는 DOM이 업데이트 되면 바로 동기적으로 실행이 되고 이러한 과정은 브라우저가 화면에 렌더링하기 이전에 수행된다. 따라서 사용자는 업데이트 되기 전의 화면을 보지 않게 된다.
&nbsp;
&nbsp;
[reactJS Hook API reference](https://ko.reactjs.org/docs/hooks-reference.html#useeffect)







