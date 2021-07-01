---
layout: post
title:  "react - 배열 조작"
date:   2021-03-29 12:31:00 +0100
categories: "study"
---

# react - 배열 조작 등
&nbsp;
• 리액트에서는 **불변성**을 항상 지켜줘야 한다. 어떤 값을 수정하게 될 때, 꼭 **setState**를 사용해야 하며
  기존의 배열이나 객체를 수정할 때에는 값을 할당하지 말고 새 객체를 만들어 값을 주입하는 방식으로
  사용해야 한다. 배열의 경우,
```
  this.setState({
    /*
      information이라는 배열에 data를 추가하고 싶을 경우
      information에 push하는 방식이 아닌, 지금 가지고 있는 information에 data를 
      붙인 새로운 배열을 만들어 information에 할당한다.
    */ 
    information(배열명): this.state.information.concat(data),
  })
```
&nbsp;
&nbsp;
• **비구조 할당** 문법을 사용하면, 코드를 간결하게 작성하는 데 도움이 된다.
```
  state = {
    information: [],
  }

  // render될 컴포넌트의 onCreate에 할당하여 render시에 information이라는 
  // 배열에 값을 할당하도록 하는 함수
  handleCreate = (data) => {
    /*
      이 부분이 비구조 할당 문법을 사용한 부분이다.
      비구조 할당이란 배열이나 객체의 속성을 해체하여 그 값을 개별 변수에 담을 수 
      있게 하는 표현식으로, 여기선 this.state를 information이라는 변수에 
      할당하여 아래의 setState 함수에서 this.state.information.concat(data) 
      형식으로 사용하여야 했을 부분을 information.concat(data) 형태로 훨씬 간략하게 
      표현할 수 있다.
    */
    const { information } = this.state
    
    this.setState({
      /*
        information이라는 배열에 data를 추가하고 싶을 경우
        information에 push하는 방식이 아닌, 지금 가지고 있는 information에 
        data를 붙인 새로운 배열을 만들어 information에 할당한다.
      */ 

      information: information.concat(data),
      // information(배열명): this.state.information.concat(data),
    })
  }
```
&nbsp;
&nbsp;
• **defaultProps**를 통해 props의 초기 값을 선언할 수 있다.
```
  // defaultProps 를 사용할 때에는 static으로 선언해야 한다.
  static defaultProps = {
    data: []
  }
```
&nbsp;
&nbsp;
• 여러 개의 컴포넌트를 동시에 렌더링할 때, key 값(해당 컴포넌트가 가질 고유값)을 지정하여
  업데이트 성능을 최적화할 수 있다.
• 이 key 값은 배열을 렌더링할 때 사용된다.
• key가 없다면, 배열 내부의 값을 조작할 때 비효율적으로 작동한다.
• key는 고유한 값이여야 한다.
• shouldComponentUpdate 함수를 사용하여 불필요한 렌더링을 수행하지 않을 수 있다.
```
  shouldComponentUpdate(nextProps, nextState) {
    // state값이 이전과 달라지면, 새로 렌더링한다.
    if(this.state !== nextState){
      return true;
    }
    // props의 info값이 이전과 달라지면, 새로 렌더링한다.
    return this.props.info !== nextProps.info;
  }
```








