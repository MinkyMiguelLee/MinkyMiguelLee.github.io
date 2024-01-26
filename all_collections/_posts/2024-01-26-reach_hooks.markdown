---
layout: post
title: "[스크랩] 성능 최적화(Hooks)"
date: 2024-01-25 10:42:00 +0100
categories:
published: true
---

# [스크랩] 성능 최적화(Hooks)

오늘은 그동안 React를 공부하고 알아왔던, class기반이 아닌 hooks 기반의 성능 최적화에 대한 방법들을 포스팅 하고자 한다.

먼저 컴포넌트의 리렌더링 되는 조건은 아래와 같다.

- 부모에서 전달받은 `props`가 변경될때
- 부모 컴포넌트가 리렌더링 될 때
- 자신의 `state`가 변경 될 때

설명 드리기 전, 아래 첨부한 코드 링크를 열고 읽으면서, 글을 하나하나 읽으면서 테스트 하면 이해하는데 도움이 되고, 테스트 할 때마다 console에 찍히는 부분이 어떻게 차이가 나는지 확인하면 최적화에 더 와닿을 것이라 생각한다.

[최적화 전 Source Code](https://codesandbox.io/s/react-object-property-before-memoization-ne57e?file=/src/Item.js:0-287) [최적화 이후 Source Code](https://codesandbox.io/s/react-object-property-after-memoization-sbhn2?file=/src/ItemList.js)

# **1. useMemo**

이 함수는 React Hook 중 하나로서 React에서 CPU 소모가 심한 함수들을 캐싱하기 위해 사용된다.만약 컴포넌트내의 어떤 `함수`가 값을 리턴하는데 하나의 변화에도 값을 리턴하는데 많은 시간을 소요한다면 이 컴포넌트가 리렌더링 될 때마다 함수가 호출되면서 많은 시간을 소요하게 될 것이다.또 그 함수가 return되는 값이 자식 컴포넌트에도 사용이 된다면, 그 자식 컴포넌트도 함수가 호출 될 때마다 새로운 값을 받아 리렌더링 된다.

만약 컴포넌트 내에 어떤 함수가 값을 리턴하는데 많은 시간을 소요한다면, 이 컴포넌트가 리렌더링 될 때마다 함수가 호출되면서 많은 시간을 소요하게 될 것이고, 그 함수가 반환하는 값을 하위 컴포넌트가 사용한다면 그 하위 컴포넌트는 매 함수호출마다 새로운 값을 받아 리렌더링할 것이다.

```
//UserList.jsx
import { useState, useMemo useRef } from "react";
import Item from "./Item";
import Average from "./Average";

function UserList() {
  let numberRef = useRef(2);

  const [users, setUsers] = useState([
    {
      id: 0,
      name: "sewon",
      age: 30,
      score: 100
    },
    {
      id: 1,
      name: "kongil",
      age: 50,
      score: 10
    }
  ]);

  const average = (() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
      return acc + cur.age / users.length;
    }, 0);
  })();

  return (
      <div>
       <Average average={average} />
      </div>
  );
}

export default UserList;

```

위 코드에서, 우리는 `average`가 즉시 실행되어 `Average`컴포넌트의 props로 전달되는 것을 볼 수 있다.

이 예제에서 평균값을 구하는 함수는 간단한 함수이지만, 이 평균값을 구하는 연산이 엄청 오랜 시간이 걸린다고 가정해보면, UserList 컴포넌트가 리렌더링 될 때마다 매번 이 비싼 연산을 수행해야만 한다.

위와 같은 문제는 useMemo를 통해 `average`를 최적화 함으로써 해결할 수 있다. useMemo는 아래와 같은 구조를 가진다.

```
useMemo(()=> func, [input_dependency])
```

`func`은 캐시하고 싶은 함수이고, `input_dependency`는 useMemo가 캐시할 func에 대한 입력의 배열로서 해당 값들이 변경되면 func이 호출된다.이것을 적용하면 `input_dependency`가 있는 데이터가 변할 때에만 평균을 구하는 연산을 수행하도록 한다. `input_dependency`에는 `users` state를 넣어준다.

```
  const average = useMemo(() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
      return acc + cur.score / users.length;
    }, 0);
  }, [users]);

```

useMemo는 종속 변수들이 변하지 않으면 함수를 굳이 다시 호출하지 않고 이전에 반환한 참조값을 재사용 한다.즉, 함수 호출 시간도 세이브할 수 있고 같은 값을 props로 받는 하위 컴포넌트의 리렌더링도 방지할 수 있다.

# **2. React.memo 컴포넌트 메모이제이션**

[React.memo, useCallback 사용으로 렌더링 최적화 하기(feat.React-Native,Redux)](https://velog.io/@shin6403/React.memo-useCallback-%EC%82%AC%EC%9A%A9%EC%9C%BC%EB%A1%9C-%EB%A0%8C%EB%8D%94%EB%A7%81-%EC%B5%9C%EC%A0%81%ED%99%94-%ED%95%98%EA%B8%B0feat.React-NativeRedux)앞 전에 memo,useCallback에 대해 쓴 글이 있는데, 참고용으로 좋을 부분이다 :)

React.memo는 Hook이 아니기 때문에 클래스형 컴포넌트에서도 사용할 수 있다.함수형 컴포넌트에서는 shouldComponentUpdate를 사용할 수 없는데, 리액트 공식 문서에서는 그 대안으로 React.memo를 제시하고 있고,**우리는 이것을 통해 컴포넌트의 props가 바뀌지 않았다면, 리렌더링하지 않도록 설정하여 함수형 컴포넌트의 리렌더링 성능을 최적화 해줄 수 있다.**React.memo는 콜백함수를 이용해 메모이제이션을 적용할지 여부를 판단할 수도 있다.

위코드에서 몇가지 추가된 내용이 있다.

```
//UserList.jsx
import { useState, useRef } from "react";
import Item from "./Item";
import Average from "./Average";

function UserList() {
  let numberRef = useRef(2);
  const [text, setText] = useState("");
  const [users, setUsers] = useState([
    {
      id: 0,
      name: "sewon",
      age: 30,
      score: 100
    },
    {
      id: 1,
      name: "kongil",
      age: 50,
      score: 10
    }
  ]);

  const average = useMemo(() => {
    console.log("calculate average. It takes long time !!");
    return users.reduce((acc, cur) => {
      return acc + cur.score / users.length;
    }, 0);
  }, [users]);

   const addUser =() => {
    setUsers([
      ...users,
      {
        id: (numberRef.current += 1),
        name: "yeonkor",
        age: 30,
        score: 90
      }
    ]);
  }

  return (
      <div>
       <input
         type="text"
         value={text}
         placeholder="아무 내용이나 입력하세요."
         onChange={(event) => setText(event.target.value)}
        />
       <Average average={average} />
       <button className="button" onClick={addUser}>
        새 유저 생성
       </button>
      {users.map((user) => {
        return (
          <Item key={user.id} user={user} /> // 아래 코드 참고
        );
      })}
      </div>
  );
}

export default UserList;
```

이 전에 있던 코드에서 `Item`이라는 컴포넌트를 만들어 리스트를 만들어주고 `button`을 클릭할 때마다 `addUser`라는 함수가 실행되어 리스트가 추가되는 것을 구현하였다.

```
//Item.jsx
import React,{ memo } from "react";

function Item({ user }) {
  console.log("Item component render");

  return (
    <div className="item">
      <div>이름: {user.name}</div>
      <div>나이: {user.age}</div>
      <div>점수: {user.score}</div>
      <div>등급: {result.grade}</div>
    </div>
  );
}

export default memo(Item);
```

React.memo를 적용했으므로 새 유저 생성 버튼을 눌러 `users` 배열의 길이를 변화시켜 `UserList.jsx`를 리렌더링 시키더라도 새로 추가된 `Item`만 새로 렌더되고 이미 렌더된 `Item`들은 리렌더링 되지 않는다.

# **3. useCallback**

useMemo가 리턴되는 값을 memoize 시켜주었는데, useMemo와 비슷한 useCallback은 **함수 선언을 memoize 하는데 사용된다.**`UserList` 방금 눌렀던 button 태그를 하위 컴포넌트인 `Button` 컴포넌트를 새로 만들고 교체해 설명을 진행한다.

```
import React.{memo} from "react";

function Button({ onClick }) {
    console.log("Button component render");

  return (
    <button type="button" onClick={onClick}>
      버튼
    </button>
  );
}

export default memo(Button);

```

onClick 함수는 `UserList`에서 전달받고 있다고 한다.UserList는 input에 타이핑을 할때마다, 자식(Button) 트리를 포함하여 리렌더링 된다.그런데 리렌더링마다 addUser라는 함수를 새로 생성하여 Button 컴포넌트에 props로 전달해주고 있다.여기서 `Button` 컴포넌트는 불필요한 렌더링을 막기 위해 memo를 이용하여 memoize 되어 있다.React.memo는 현재와 다음 props를 비교하여 이전 props와 같다면 컴포넌트를 리렌더링 하지 않는다.

> Button 컴포넌트는 onClick props를 함수로 받고 있는데, 언제든 UserList가 리렌더링 될 때 Button에게 전달되는 onClick props가 동일한지 체크한 후 동일하다면 리렌더링 되지 않아야 한다.

하지만 이 경우에 Button 컴포넌트도 같이 리렌더링 되는 문제가 발생되는데, 이 상황에선 Button 컴포넌트에 memo로 감싸도 소용이 없다.그 이유는 **함수는 객체이고, 새로 생성된 함수는 다른 참조 값을 가지기 때문에 Button 입장에서는 새로 생성된 함수를 받을 때 props가 변한 것으로 인지하기 때문이다.**

그래서 이럴때 useCallback을 써야한다.

상위 컴포넌트에서 하위컴포넌트로 함수를 props로 넘겨줄 때, 상위 컴포넌트가 리렌더링 될 때마다 상위 컴포넌트 안에 선언된 함수를 새로 생성하기 때문에 그때마다 새 참조 함수를 하위 컴포넌트로 넘겨주게 된다.이에 따라 하위 컴포넌트도 props가 달라졌으므로 또다시 리렌더링 하게 된다.

그러나 useCallback으로 함수를 선언해주면, 종속 변수들이 변하지 않는 이상 굳이 함수를 재생성하지 않고 이전에 있던 참조 변수를 그대로 하위 컴포넌트에 props로 전달하여, 하위 컴포넌트도 props가 변경되지 않았다고 인지하게 되어 하위 컴포넌트의 리렌더링을 방지할 수 있다.

# **4. 자식 컴포넌트의 props로 객체를 넘겨줄 경우 변형하지말고 넘겨주기**

흔히 작업을 하다보면 props의 값으로 객체를 넘겨주는 경우가 많은데, 이때 props로 전달하는 형태에 주의 하여야 한다.

```
// 생성자 함수
<Component prop={new Obj("x")} />
// 객체 리터럴
<Component prop={{property: "x"}} />
```

이런 경우 새로 생성된 객체가 props로 들어가므로 컴포넌트가 리렌더링 될 때마다 새로운 객체가 생성되어 자식 컴포넌트로 전달된다.props로 전달한 객체가 동일한 값이어도 새로 생성된 객체는 이전 객체와 다른 참조 주소를 가진 객체이기 때문에 자식 컴포넌트는 메모이제이션이 되지않는다.

아래 코드는 메모이제이션 되지 않는 안좋은 예이다.

### **안좋은 예(🙅🏻‍♂️)**

```
// UserList.jsx
function UserList() {
{...}

 const getResult = useCallback((score) => {
    if (score <= 70) {
      return { grade: "D" };
    } else if (score <= 80) {
      return { grade: "C" };
    } else if (score <= 90) {
      return { grade: "B" };
    } else {
      return { grade: "A" };
    }
  }, []);

return(
 <div>
 {users.map((user) => {
    return (
      <Item key={user.id} user={user} result={getResult(user.score)} />
        );
      })}
 </div>

)
export default memo(UserList);

// Item.jsx
function Item({ user, result }) {
  console.log("Item component render");

  return (
    <div className="item">
      <div>이름: {user.name}</div>
      <div>나이: {user.age}</div>
      <div>점수: {user.score}</div>
      <div>등급: {result.grade}</div>
    </div>
  );
}

export default Item;

```

그래서 이럴때는, 아래 코드처럼 생성자 함수나 객체 리터럴로 객체를 생성해서 하위 컴포넌트로 넘겨주는 방식이 아닌, state를 그대로 하위컴포넌트에 넘겨주어 필요한 데이터 가공을 그 하위컴포넌트에서 해주는 것이 좋다.

### **좋은 예(🙆🏻‍♂️)**

```
// UserList.jsx
function UserList() {
{...}

return(
 <div>
 {users.map((user) => {
    return (
      <Item key={user.id} user={user} />
        );
      })}
 </div>

)
export default memo(UserList);

// Item.jsx

function Item({ user }) {
  console.log("Item component render");

  const getResult = useCallback((score) => {
    if (score <= 70) {
      return { grade: "D" };
    }
    if (score <= 80) {
      return { grade: "C" };
    }
    if (score <= 90) {
      return { grade: "B" };
    } else {
      return { grade: "A" };
    }
  }, []);

  const { grade } = getResult(user.score);

  return (
    <div className="item">
      <div>이름: {user.name}</div>
      <div>나이: {user.age}</div>
      <div>점수: {user.score}</div>
      <div>등급: {grade}</div>
    </div>
  );
}

export default memo(Item);

```

# **5. 컴포넌트를 매핑할 때에는 key값으로 index를 사용하지 않는다.**

![https://velog.velcdn.com/images%2Fshin6403%2Fpost%2F205e0f63-c647-4cb1-afe1-1cad840d0bce%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.40.22.png](https://velog.velcdn.com/images%2Fshin6403%2Fpost%2F205e0f63-c647-4cb1-afe1-1cad840d0bce%2F%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-09%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%206.40.22.png)

사람들이 많이 하는 실수 중에 하나가 바로 컴포넌트를 매핑할 때 key값에 index 값을 넣어준다.리액트에서 매핍을 할떄 반드시 고유 key를 부여하도록 강제하고 있는데, 이렇게 index값으로 key값을 부여하면 좋지 않다.왜냐하면, 어떤 배열에 중간에 어떤 요소가 삽입될때 그 중간 이후에 위치한 요소들은 전부 index가 변경된다.이로 인해 key값이 변경되어 React는 key가 동일 할 경우, 동일한 DOM Element를 보여주기 때문에 예상치 못한 문제가 발생한다. 또한, 데이터가 key와 매치가 안되어 서로 꼬이는 부작용도 발생한다.

## **그러면 index 요소는 반드시 사용하면 안되는 걸까?**

배열의 요소가 필터링, 정렬 삭제, 추가 등의 기능이 들어간다면 문제가 발생할수 있으나 다음과 같은 경우에서는 index로 사용해도 무방다.다만, 가급적이면 코드의 일관성을 위해 최대한 index 를 사용 안하는 것을 권장한다.

- 배열과 각 요소가 수정, 삭제, 추가 등의 기능이 없는 단순 렌더링만 담당하는 경우
- id로 쓸만한 unique 값이 없을 경우
- 정렬 혹은 필터 요소가 없어야 함

# **6. useState의 함수형 업데이트**

기존의 useState를 사용하며, 대부분 setState시에 새로운 상태를 파라미터로 넣어주었다.setState를 사용할 때 새로운 상태를 파라미터로 넣는 대신, 상태 업데이트를 어떻게 할지 정의해 주는 업데이트 함수를 넣을 수도 있는데,이렇게 하면 **useCallback을 사용할 때 두 번째 파라미터로 넣는 배열에 값을 넣어주지 않아도 된다.**

```
// 예시) 삭제 함수
const onRemove = useCallback(
  id => {
    setTodos(todos.filter(todo => todo.id !== id));
  },
  [todos],
);

// 예시) 함수형 업데이트 후
const onRemove = useCallback(id => {
  setTodos(todos => todos.filter(todo => todo.id !== id));
}, []);

```

# **7. Input에 onChange 최적화**

보통 input 태그에 onChange 이벤트를 줄때 타이핑을 할때마다 해당 컴포넌트가 렌더링 되어, 최적화 방법을 많이 찾곤한다.`lodash`라고 최적화 라이브러리를 쓰기도 하는데, 아래 코드는 라이브러리를 쓰지 않고, 최적화 시킬수 있는 방법이다.

```
// 예시) 최적화 전(X)
//UserList.jsx
function UserList() {
 {...}
  return (
      <div>
       <input
         type="text"
         value={text}
         placeholder="아무 내용이나 입력하세요."
         onChange={(event) => setText(event.target.value)}
        />
   {...}
      </div>
  );
}

export default UserList;

// 예시) 최적화 후(O)
//UserList.jsx
function UserList() {
 {...}
  return (
      <div>
       <input
          ref={searchRef}
          type="text"
          placeholder="아무 내용이나 입력하세요."
          onKeyUp={() => {
            let searchQuery = searchRef.current.value.toLowerCase();
            setTimeout(() => {
              if (searchQuery === searchRef.current.value.toLowerCase()) {
                setText(searchQuery);
              }
            }, 400);
          }}
        />
   {...}
      </div>
  );
}

export default UserList;

```

이렇게 말고도 더 deep하고 알아갈 부분들이 많지만, 이번 시간엔 실생활에서 바로 적용할 수있는 최적화 방법에 대해 알아보았다.리액트는 단방향 하향식 데이터 흐름을 가지고 있어, 부모 컴포넌트에서 자식 컴포넌트 방향으로 데이터(props, state)가 흘러간다.이 데이터들의 변화는 컴포넌트를 리렌더링시키는데, state는 그것이 선언된 컴포넌트 내에서 사용되고, props는 부모 컴포넌트로부터 받은 데이터이다. 이 기본 구조를 숙지하고 가면 최적화 방법이 쉽게 적용 가능하다.
