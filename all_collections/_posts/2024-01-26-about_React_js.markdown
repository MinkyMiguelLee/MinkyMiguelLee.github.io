---
layout: post
title: "about React.js"
date: 2024-01-25 10:32:00 +0100
categories: ["study", "React.js"]
published: true
---

### **다국어 페이지는 어떤 방식으로 제공하나?**

SSR의 경우 HTTP 요청 시 클라이언트에서 Accept-Language 헤더와 같이 기본 언어 설정에 대한 정보를 보낸다. 서버는 이 정보를 이용하여 해당 언어로 된 문서 버전을 반환한다. 반환된 문서는 lang 속성을 선언한다. CSR의 경우 클라이언트 사이드에서 해당 언어 팩(JSON 등)을 가져와 출력한다.

### **브라우저의 동작 원리는?**

브라우저의 기본적인 역할은 HTML, CSS 명세에 따라 HTML 파일을 해석하여 표시하는 것이다. 브라우저를 구성하는 요소는 사용자 인터페이스, 브라우저 엔진(크롬, 사파리는 Webkit, 파이어폭스는 Gecko), 렌더링 엔진, 통신, UI 백엔드, 자바스크립트 해석기, 자료 저장소 등이 있다. 렌더링 엔진은 먼저 HTML 문서를 파싱해서 DOM 트리를 구축한다. 그리고 CSS 마크업을 파싱해서 앞서 구축한 DOM 트리와 함께 렌더링 트리를 만든다. 렌더링 트리는 화면에 보여줄 것들만 가지고 있는 트리로, 구축이 되면 순차적으로 화면에 배치한다. 부모에서 자식 순서로 배치가 진행된다. 배치가 완료되면 그리기를 시작한다.

### **빈 화면에서 렌더링이 완료되기까지 너무 오래 걸린다는 피드백이 있을 때, 어떻게 하면 이 문제를 해결할 수 있나?(단, 캐시는 이미 적용 됨)**

script 파일을 body tag 가장 하단에 위치시키거나 script 태그에 async 속성을 부여한다. 또는 네트워크 리소스 블라킹을 통해 불필요한 무거운 파일들을 제한한다.

### **서버 사이드 렌더링(SSR)과 클라이언트 사이드 렌더링(CSR)의 차이는?**

서버 사이드 렌더링은 전통적인 웹 방식을 의미하며 페이지가 새로고침 될 때마다 서버로부터 리소스를 전달받아 화면에 렌더링 하는 방식이다. 하지만 React, Vue 등의 라이브러리가 등장하면서 훨씬 더 좋은 성능의 SPA 방식의 개발 환경을 선호하기 시작하였다. 클라이언트 사이드 렌더링에서 서버는 단지 JSON 파일을 보내주는 역할만 할 뿐이며, html을 그리는 역할은 클라이언트에서 수행한다. 하지만, CSR은 자바스크립트가 모든 동작을 수행한 후에 화면에 내용이 나타나므로 초기 구동속도는 SSR에 비해 느리다는 단점이 있다. SEO를 할 수 없고 보안적으로 취약하다는 문제점도 나타난다.

### **생명주기(lifecycle) 메소드는 무엇인가?**

클래스 기반 컴포넌트는 그들이 mount(DOM에 렌더링)되었을 때, unmount될 때 등과 같이 그들의 생명주기 중 특정한 시점에 호출되는 특별한 메소드를 선언할 수 있다. 예를 들면, 컴포넌트가 필요로 하는 것을 셋팅 및 해제하거나, 타이머를 설정하거나 브라우저 이벤트에 바인딩하는 데 유용한다.

아래의 생명주기 메소드들은 컴포넌트를 불러오기 위해 사용할 수 있다.

- **componentWillMount**: 컴포넌트가 생성된 후 DOM에 렌더링되기 전에 호출된다.
- **componentDidMount**: 처음으로 렌더링이 끝나고 컴포넌트의 DOM 엘리먼트가 사용 가능할 때 호출된다.
- **componentWillReceiveProps**: props가 업데이트 될 때 호출된다.
- **shouldComponentUpdate**: 새로운 props를 받았을 때 호출되며, 성능 최적화를 위해 리랜더링을 막을 수 있다.
- **componentWillUpdate**: 새로운 props를 받았고 shouldComponentUpdate가 true를 리턴할 때 호출된다.
- **componentDidUpdate**: 컴포넌트가 업데이트된 후에 호출된다.
- **componentWillUnmount**: 컴포넌트가 DOM에서 제거되기 전에 호출되어 이벤트리스너 등을 정리할 수 있게 해준다.

### **요소를 배치하는 방법(static, relative, fixed, absolute)간의 차이는?**

- **static**: 기본값으로 요소들이 겹치지 않고 상→하로 배치된다.
- **relative**: 원래 배치되어야 할 위치에서 지정한 값 만큼 떨어진 곳에 요소를 배치한다.
- **fixed**: 웹 브라우저 화면 전체를 기준으로 배치한다. 스크롤을 하더라도 위치가 고정된다.
- **absolute**: 가장 가까운 상위 요소의 위치를 기준으로 지정한 값 만큼 떨어진 곳에 요소를 배치한다.

### **제어 컴포넌트와 비제어 컴포넌트의 차이는?**

HTML 문서에서 많은 form 엘리먼트들(<select>, <textarea>, <input> 등)은 고유한 state를 유지한다. 비제어 컴포넌트는 DOM을 이러한 input들의 state에 대한 진짜 근원(source of truth for the state of these inputs)으로 취급한다. 제어 컴포넌트에서 내부 state는 엘리먼트의 값(value)를 추적하기 위해 사용된다. input의 값이 변경되면 리액트는 input을 다시 렌더링한다. 비제어 컴포넌트는 non-React 코드와 합칠 때(예를 들어 jQuery 플러그인의 일부를 지원해야 할 때) 유용하게 사용될 수 있다.

### **쿠키와 세션 스토리지, 로컬 스토리지의 차이는?**

기본적으로 쿠키와 로컬 스토리지, 세션 스토리지는 모두 브라우저에서 데이터 저장소의 역할을 한다. 웹에서 로그인을 하기 위해서는 토큰을 발급받아 API를 호출해야 한다. 하지만 반복되는 작업을 계속하게 되는 것은 비효율적이고, 이를 보완하기 위해 쿠키를 서버와 클라이언트에 생성해서 토큰 발급 없이 쿠키만 가지고 서버에 요청을 할 수 있게 된다. 쿠키는 저장 공간이 4KB로 작은 편인데 이러한 단점을 보완하여 만든 것이 웹 스토리지이다.

웹 스토리지는 서버에 클라이언트 데이터를 저장하지 않는다. 웹 스토리지에는 로컬 스토리지와 세션 스토리지가 있으며, 로컬 스토리지는 브라우저에 정보가 계속해서 남아있는 반면, 세션 스토리지는 해당 세션이 끝나면, 즉 브라우저가 닫히면 데이터가 사라진다. 웹 스토리지는 데스크탑 기준 5~10MB의 저장 공간을 가지고 있어서 쿠키에 비해 훨씬 저장공간이 크다는 장점이 있다. 웹 스토리지는 반면 HTML5부터 지원하기 때문에 이전 브라우저에서는 지원이 되지 않는다는 단점이 있다.

### **크로스 브라우징이란?**

크로스 브라우징은 웹 표준에 따라 서로 다른 OS 또는 플랫폼에 대응하는 것을 말한다. 브라우저별 렌더링 엔진이 다른 상황 등 어떠한 상황 속에서도 문제 없이 동작하게 하는 것을 목표로 한다. 프론트엔드 개발자는 여러가지 전략을 세울 수가 있으며, feature detection(기능 탐지)을 사용해서 해당 기능이 해당 브라우저에 있는지를 확인하는 방법을 사용할 수도 있다. 특히 한 쪽 환경에 최적화를 하는 것 보다, 전체적인 웹 표준을 지키는 데에 노력해야 한다.

### **클래스형 컴포넌트와 함수형 컴포넌트의 차이는?**

React 16.8(hooks 도입) 이전에는 내부 state를 유지하는데 필요한 컴포넌트를 생성하거나 생명주기 메소드(lifecycle methods)(즉, componentDidMount 및 shouldComponentUpdate)를 활용하기 위해 클래스 기반 컴포넌트를 사용했다. 클래스 기반 컴포넌트는 리액트의 Component 클래스를 확장하는 ES6 클래스이다. 또한 최소한 render() 메서드를 포함해야 한다.

(hooks 도입 이전의) 함수형 컴포넌트는 state를 갖지 않으며 렌더링할 출력 결과를 리턴(반환)한다. 함수형 컴포넌트는 클래스 기반 컴포넌트보다 심플하기 때문에 props에만 의존하는 UI을 렌더링하는데 선호된다.

### **클리어링(Clearing)에는 어떤 것들이 있으며, 각각은 어떨 때 사용하나?**

float 속성의 영향에서 벗어나기 위해 사용하는 clear 속성은 float의 특성을 지워주는 역할을 한다. 총 4가지 값이 있는데 both는 양쪽의 float 속성을 지워주며, left와 right는 각각 왼쪽, 오른쪽 속성을 지워주고 none은 기본 값으로 아무 것도 지워주지 않다. 보통은 float 속성을 감싸고 있는 요소들의 height를 조정하기 위해 사용된다.

### **프로그레시브 렌더링(Progressive Rendering)이란?**

프로그레시브 렌더링은 컨텐츠를 가능한 빨리 표시하기 위해 성능을 향상시키는 기술이다. 인터넷 속도가 느리거나 불안정한 모바일 환경이 아직 많이 남아있기 때문에 이러한 경우에 유용하게 사용한다. 대표적으로 레이지 로딩이 있다. 이미지를 한 번에 로드하는 것이 아니라, 자바스크립트를 통해 사용자가 표시하려는 부분만 스크롤 시에 이미지를 로드하는 것이다.

### **호이스팅이란?**

ES6 이후에서 함수나 변수 선언이 해당 유효 범위(스코프)의 최상단으로 끌어올려지는 것처럼 보이는 현상이다. 실제로는, 컴파일 타임에 변수/함수 선언이 메모리에 들어가되, 할당은 코드를 작성한 위치에서 진행된다. 호이스팅은 변수/함수 선언에만 적용되는데, 초기화만 해주는 경우 호이스팅이 일어나지 않다. 또한, var로 변수를 선언한 경우에만 호이스팅이 일어난다.

### **화살표 함수와 일반 함수의 차이는?**

화살표 함수는 ES6에서 새로 추가되었다. 화살표 함수는 익명 함수로, 이름이 없는 함수, 즉시 실행이 필요할 경우 사용하는 함수이다.

우선 바인딩이란, 함수 호출과 실제 함수를 연결하는 방법이다. 함수를 호출하는 부분에서 실제 함수가 위치한 메모리를 연결하는 것도 바인딩이다. 바인딩은 정적 바인딩(static binding)과 동적 바인딩(dynamic binding)으로 구분할 수 있다. (정적 바인딩은 실행 시간 전에 일어남. 실행 시간에는 변하지 않는 상태로 유지. 동적 바인딩은 실행 시간에 이루어지거나 실행 시간에 변경됨.)

1. **this**: 자바스크립트에서 모든 함수는 실행될 때마다 함수 내부에 this라는 객체가 추가된다. 일반 함수는 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다. 화살표 함수는 선언할 때 this에 바인딩할 객체가 정적으로 결정된다. 를 가리킨다(Lexical this). 또한, call, apply, bind 메소드를 사용하여 this를 변경할 수 없다.

   화살표 함수의 this는 언제나 상위 스코프의 this

2. **생성자 함수로 사용 가능 여부**: 일반 함수는 생성자 함수로 사용할 수 있다. 이다.

   화살표 함수는 생성자 함수로 사용할 수 없다. prototype 프로퍼티를 가지고 있지 않기 때문

3. **arguments 사용 가능 여부**: 일반 함수에서는 함수가 실행될 때 암묵적으로 arguments 변수가 전달되어 사용 가능한다. .

   화살표 함수에서는 arguments 변수가 전달되지 않다

### **box model이 무엇이며, 브라우저에서 어떻게 동작하나?**

box model은 각각의 object를 박스 형태로 나타내어 브라우저에 배치하기 위한 규칙이다. W3C 박스 모델과 IE 박스 모델이 있으며 두 가지 박스 모델은 차이가 있다. W3C 박스 모델은 content-box로 width가 content만 포함하는 반면, IE 박스 모델은 border-box로 width에 content, padding, border를 모두 포함한다.

### **class와 id의 차이는?**

id와 class의 차이는 id는 유일한 요소에 적용할 때, 그리고 class는 복수의 요소에 적용할 때 사용한다는 점이다. 하나의 id는 한 문서에서 한 번만 사용 가능하지만, 하나의 class는 여러 번 사용이 가능한다. 우선순위는 id가 class보다 높다.

### **CORS가 무엇이며 어떻게 해결을 할 수 있나?**

다른 도메인에서 리소스 요청 시 cross-origin HTTP에 의해 요청을 하는데, 대부분의 브라우저는 보안 상의 이유로 이 요청을 제한한다. 이를 동일 오리진 정책(Same Origin Policy)이라고 한다. 요청을 보내기 위해서는 요청 보내는 대상과 프로토콜이 같아야 하고, 포트도 같아야 한다. JSONP(JSON-padding)을 통해 해결하거나 특정 HTTP 헤더를 추가하여 이 이슈를 해결할 수 있다. 이와 같이 타 도메인 간 자원을 공유할 수 있게 해주는 것을 Cross Origin Resource Sharing, 줄여서 CORS라고 한다.

### **CSS-in-JS의 장점은?**

- 컴포넌트 단위로 생각할 수가 있다. CSS-in-JS는 CSS 모델을 문서 레벨이 아니라 컴포넌트 레벨로 추상화한다.(모듈성)
- 진정한 분리 법칙을 따른다. CSS에는 명시적으로 정의하지 않은 경우, 부모 요소에서 자동으로 상속되는 속성이 있다. CSS-in-JS의 경우 부모 요소의 속성을 상속하지 않다.

대표적인 라이브러리로 styled-components가 있다.

### **CSS 애니메이션과 JS 애니메이션의 차이는?**

- **CSS**: 일반적으로, 마우스를 올렸을 때 혹은 메뉴 버튼의 전환과 같은 간단하게 처리하는 애니메이션의 경우 CSS로 처리할 수 있다. CSS 자체가 선언형(declarative)이기 때문에 어떤 요소가 애니메이션을 가져야 한다는 직관적인 표현이 가능한다. 낮은 버전의 브라우저에서 지원을 안 하는 경우가 있다.
- **JS**: 애니메이션을 세밀하게 제어해야 하는 경우 JS를 사용한다. 크로스 브라우징 측면에서 JS 애니메이션을 사용하는 것이 유리한다. velocity.js와 같은 라이브러리를 사용하면 CSS보다 성능이 좋다.

### **CSS 전처리기(Pre-Processor)의 장점과 단점은?**

CSS 전처리기를 사용하게 되면 selector를 nesting으로 관리할 수 있고, 조건문이나 반복문, 간단한 연산 등을 할 수 있어서 CSS를 마치 프로그래밍 하듯이 코딩할 수 있다는 장점이 있다. 단점은 웹에서는 CSS만 동작하기 때문에 전처리기는 직접 동작시킬 수가 없다. 따라서 CSS로 컴파일 후 동작시켜야 한다.

### **display 속성에 어떤 것들이 있나?**

display 속성에는 block, inline, inline-block, none이 있다.

- **block**: 항상 새로운 라인에 요소가 시작되고 화면 크기의 전체 가로폭을 영역으로 차지한다. width 속성 값을 부여해주면 그 너비만큼 영역을 차지한다.
- **inline**: 새로운 라인에서 시작되지 않으며 다른 요소들과 같은 줄에 배치될 수 있고 content 너비만큼의 영역을 차지한다. 그리고 width, height, margin-top, margin-bottom 속성이 적용되지 않다.
- **inline-block**: block 레벨 요소와 inline 레벨 요소의 특징을 모두 가지고 있다. 한 줄에서 inline 레벨 요소들과 같이 배치될 수 있으며 width와 height 속성으로 영역의 크기를 지정할 수 있다.
- **none**: 선택한 요소들을 화면에 나타나지 않게 한다. 'visibility: hidden'과의 차이점은 영역이 남아있는지 여부가 다르다는 점이다(display: none은 영역도 없앰).

### **element와 component의 차이는?**

element는 DOM 노드 또는 다른 component들과 관련하여 화면에 표시할 내용을 표현하는 일반 객체이다. elements는 다른 elements들을 포함할 수 있다. React element를 만드는 비용은 저렴한다. element는 생성되면 변형되지 않다.

반면 component는 여러 방법으로 선언될 수 있다. render 메서드가 있는 class일 수도 있다. 간단한 component 일 경우 function으로 정의 될 수 있다.입력된 component를 바탕으로 element 트리를 만든다. 마지막에 JSX는 createElement로 변환된다.

### **flex를 사용하는 이유는?**

flex는 레이아웃을 좀 더 편하게 잡기 위해서 만들어진 css 속성이다. flex를 사용하면 요소들의 크기나 위치를 쉽게 잡을 수 있다. 기존에 수평 구조를 만들 때 사용하는 속성이 float나 inline-block 등이 있으며 이들은 여러가지 문제를 가지고 있었는데 flex를 사용하면 이러한 속성의 한계를 보완할 수 있다. 물론 수평 뿐만 아니라 수직도 가능한다.

flex는 컨테이너와 아이템 개념을 사용하여 요소의 크기가 불분명하거나 동적인 경우에도 요소를 효율적으로 정렬할 수 있게 해준다.

### **float는 어떻게 동작하나?**

float 속성은 현재 위치의 왼쪽이나 오른쪽으로 shift되어 배치되는 박스의 일종이다. 이 때 컨텐츠는 float 속성이 적용된 요소의 주변에 위치하게 된다.

### **Hash Table이란?**

해시테이블은 key-value 형태의 데이터 구조이며, key를 통해 해당 데이터에 직접적인 접근이 가능하며 순차검색에 비해 해시테이블을 이용한 검색은 속도 측면에서 획기적이라고 할 수 있다. 해시함수는 해시테이블의 key로 레코드가 저장되어 있는 주소(혹은 색인)를 산출하는 함수라고 할 수 있다.

### **HTML5 tag란?**

모든 HTML 문서는 <!DOCTYPE> 선언으로 시작한다. <!DOCTYPE>은 태그는 아니지만 브라우저가 어떤 타입을 받아들여야 할지를 알려주는 정보이다.

여러가지 태그가 있는데 주요한 것들 위주로 살펴보면, HTML5의 필수 태그는 html, head, body 등이 있다. html 태그는 HTML문서의 가장 최상단에 위치하는 태그이며, head 태그에는 style, script, title, link, meta 태그 등이 들어간다. body 태그는 HTML 문서의 내용이 들어간다.

meta 태그는 head 부분에서 다른 태그들(script, style, link, title 등)로 나타낼 수 없는 메타데이터를 나타내는 태그를 의미한다. <meta name="keywords" content="ABC"> 와 같이 검색 엔진을 위한 키워드나 <meta name="description" content="OWEN">과 같이 문서에 대한 설명 등에 사용된다. 화면에는 별다르게 표시되는 내용이 없지만, 검색 엔진이나 브라우저에서 읽힌다.

### **JSX란?**

JSX는 HTML처럼 보이는 코드를 작성할 수 있게 해주는 자바스크립트 문법의 확장이다. JSX는 자바스크립트 함수 호출 방식으로 컴파일되어 컴포넌트에 대한 마크업을 만들 수 있는 더 좋은 방법을 제공한다.

### **key는 어떻게 사용되나?**

리액트에서 collection을 렌더링할 때 엘리먼트와 데이터 사이의 관계를 추적하기 쉽도록 반복되는 각 엘리먼트에 key를 추가하는 것이 중요한다. 키는 고유한 ID(이상적으로는 UUID 또는 기타 고유 문자열)를 사용해야 하지만, 마지막 수단으로 Array index가 될 수 있다.

key를 사용하지 않으면 collection에 item을 추가하거나 제거할 때 예상치 못한 동작 결과가 발생할 수 있다.

### **padding과 margin의 차이는?**

margin은 대상의 외부 여백을 의미하고, padding은 대상의 내부 여백을 의미한다.

### **prop로 전달되는 값의 type을 어떻게 강제하나? 또 prop가 필수적으로 전달되게끔 어떻게 강제하나?**

컴포넌트 props의 type을 확인하기 위해서는 prop-types 패키지(리액트 15.5까지는 리액트에 포함되어 있었다)를 이용하여 기대되는 값의 type과 prop가 필수적(require)인지 여부를 선언할 수 있다.

### **prop drilling은 무엇이고 어떻게 피할 수 있나?**

prop drilling은 부모 컴포넌트에서 하위 컴포넌트(자식 컴포넌트의 자식 컴포넌트 등으)로 데이터를 전달할 때 발생하는 것으로, props를 전달하는 것 외에는 props를 필요로 하지 않는 다른 컴포넌트를 통해 “drilling”(내리꽂기) 된다.

컴포넌트를 리팩토링하고, 컴포넌트를 더 작은 컴포넌트들로 쪼개지 않고, state를 가장 가까운 부모 컨포넌트와만 공유함으로써 prop drilling 회피할 수 있다. 위계상 멀리/깊게(deep/far) 떨어진 컴포넌트와 state를 공유할 때, React의 Context API 혹은 Redux와 같은 state 관리 라이브러리를 사용할 수 있다.

### **Pure Components(순수 컴포넌트)란?**

Pure Component는 동일한 상태에서 동일한 결과를 반환한다. shouldComponentUpdate 메서드를 다룰 수 있다는 점을 제외하고는 component와 동일한다.props 또는 state가 변경될 때 Pure Component는 state와 props에 대해 Shallow Compare을 수행한다.반면 component는 현재 props와 변형될 state를 비교하지 않다. 그렇기 때문에 component는 shouldComponentUpdate가 호출 될 때마다 다시 render된다. (shouldComponentUpdate의 기본값은 true이기 때문에)

### **React란?**

React는 SPA (Single Page Application) 즉, 단일 페이지 응용 프로그램에서 사용자 인터페이스를 구성하는데 사용되는 오픈 소스 프론트엔드 JS 라이브러리 이다. 웹 및 모바일 앱의 Layer를 다루는데 사용된다.

### **React의 특징은 무엇인가?**

React의 주요 특징은 다음과 같다:

- RealDOM을 조작하는데 많은 비용이 들어간다는 점을 고려하여 리액트는 RealDOM 대신 VirtualDOM을 사용한다.
- 서버 사이드 렌더링을 지원한다.
- 단방향 데이터 흐름 또는 데이터 바인딩을 따른다.
- UI 구성 요소를 재사용할 수 있도록 개발할 수 있다.

### **React 애플리케이션을 스타일링(styling)하는 보편적인 방식은 무엇인가?**

리액트 컴포넌트를 스타일링하는 다양한 방법이 있고, 각각은 장단점이 있다. 주로 사용되는 것들은 다음과 같다:

- **인라인 스타일링(Inline styling)**: 프로토타입을 만들 때 훌륭하지만 한계가 많다. (예로, pseudo-classes를 사용할 수 없다)
- **클래스 기반 CSS 스타일**: 인라인 스타일링보다 유용하고 React에 익숙하지 않은 개발자들도 쉽게 사용할 수 있다.
- **CSS in JS 스타일링**: 컴포넌트 안에서 스타일을 자바스크립트로 선언하여 스타일링할 수 있게 해주는 많은 라이브러리가 있다.

### **React context란?**

리액트는 하나의 앱 안에서 복수의 컴포넌트들이 state를 공유할 때 발생하는 문제들을 해결하기 위해 context API를 제공한다. context가 도입되기 전에는 Redux와 같은 별도의 상태 관리 라이브러리를 가져오는 것이 유일한 방법이었다. 그러나 많은 개발자들은 (특히 작은 앱에서) Redux가 불필요한 복잡성을 유발한다고 느꼈다.

### **React hooks이란?**

Hooks는 클래스 기반 컴포넌트의 장점(예를 들면, 내부 state와 생명주기 메소드)을 함수형 컴포넌트로 가져오려는 리액트의 시도이다.

### **React hooks의 장점은 무엇인가요?**

React에 hooks를 도입해서 얻을 수 있는 여러 이점들은 다음과 같다:

- 클래스 기반 컴포넌트, lifecycle hooks, this의 필요성이 사라진다.
- 공통 기능을 커스텀 hook로 만들어서 로직을 재사용하기 쉬워진다.
- 컴포넌트 자체에서 로직을 분리할 수 있어서 읽기 쉽고 테스트하기 쉬운 코드를 작성할 수 있다.

### **Redux란?**

Redux는 React를 위한 써드파티 state 관리 라이브러리로, context API가 개발되기 전부터 존재했다. Redux는 store라고 불리는 state 컨테이너의 개념을 기반으로 하는데, store 컴포넌트는 데이터를 props로 받을 수 있다. store를 업데이트하는 유일한 방법은 reducer를 통해 전달되는 store에 action을 보내는 것이다. reducer는 action과 현재의 state를 받고, 새로운 state를 반환(return)하고, 구독된(subscribed) 컴포넌트를 다시 렌더링하게 만든다.

### **<section>과 <article>의 차이는?**

section은 보통 비슷한 특성의 컨텐츠를 담는 구역을 설정할 때 사용한다. 예를 들어, header, footer 사이에 sidebar나 content를 담는 식이다. 반면 article은 관련성이 없고 독립적인 내용들을 담을 때 사용한다. 예를 들어, section 안에서 서로 다른 기사들을 나열해야 할 때 각각의 기사를 article로 담는 식이다.

### **Sementic tag란?**

시멘틱 태그는 HTML5에 도입되었는데, 개발자와 브라우저에게 의미있는 태그를 제공하는 것을 의미한다. 예를 들어 <div> 태그는 non-sementic 태그이고, <table>, <article>은 sementic 태그에 속한다. 시멘틱 태그는 태그만 보고 대략적으로 들어갈 내용을 유추할 수 있다는 장점이 있다. 헤더와 푸터를 설정할 때에도 과거에는 <div id="header"></div> 와 같이 했던 것을 이제는 <header> 하나로 깔끔하게 정리할 수 있다.

### **SEO란?**

검색 엔진 최적화(SEO)란, 웹 페이지 검색엔진이 자료를 수집하고 순위를 매기는 방식에 맞게 웹 페이지를 구성해서 검색 결과의 상위에 나올 수 있게 하는 작업을 말한다. SPA를 개발하는 경우 여러 가지 이점이 있음에도 불구하고 SEO가 잘 되지 않는다는 약점이 있다. 따라서 정보 제공을 목적으로 하는 웹 페이지는 SPA 방식이 불리할 수 있으며, React나 Angular 같은 프레임워크는 서버 렌더링을 통해 SEO에 대응할 수 있는 기술을 지원하므로 선별적으로 사용하면 된다.

### **state를 직접 변경하지 않고 왜 setState를 이용하는가?**

만약 컴포넌트의 state를 직접 변경하려고 시도한다면, 리액트는 컴포넌트를 다시 렌더링해야 하는지 알 수 있는 방법이 없다. setState() 메소드를 사용하면 리액트는 컴포넌트의 UI를 업데이트할 수 있다.

### **state와 props의 차이는?**

props는 부모 컴포넌트에서 자식 컴포넌트로 전달되는 데이터이다. props는 수정될 수 없으며 표시되거나 다른 값을 계산하는데만 사용된다. state는 컴포넌트의 생명 주기 동안 수정될 수 있는 내부 데이터로, 다시 렌더링해도 유지된다.

### **var, let, const의 차이는?**

- var는 함수 레벨의 스코프이며, let과 const는 블록 레벨의 스코프이다.
- var로 선언한 변수는 선언 전에 사용해도 에러가 발생하지 않지만, let과 const는 에러가 발생한다.
  - → 호이스팅
- var는 이미 선언되어 있는 이름과 동일한 이름의 변수를 선언하여도 에러가 발생하지 않지만, let과 const는 에러가 발생한다.
- var와 let은 변수 선언시 초기값을 설정하지 않아도 되지만, const는 반드시 초기값을 설정해야 한다.
- var와 let은 값을 재할당할 수 있지만 const는 한 번 할당한 값을 변경할 수 없다.

### **Virtual DOM이란?**

Virtual DOM은 어플리케이션의 UI를 구성하는 HTML 엘리먼트를 메모리 내에서 구현한 것이다. 컴포넌트가 다시 렌더링될 때, virtual DOM은 업데이트할 요소의 목록을 만들기 위해 기존의 DOM 모델에서 변경되는 사항을 비교한다. DOM 전체를 다시 렌더링할 필요 없이 실제 DOM에 필요한 최소한만 변경하여 효율성이 높다는 것이 큰 장점이다.

**1. real DOM 과 vitual DOM 개념 설명**

- **DOM** : Document Object Model 로 HTML 문서를 프로그래밍적으로 접근가능하게 해주는 인터페이스
  - HTML 은 브라우저에 의해 해석되어 실제 문서를 나타내는 노드 개체들의 트리구조로 변환된다.(DOM Parser) DOM 의 목적은 javascript 를 사용해서 이 문서에 대한 프로그래밍 인터페이스를 제공하는 것이다.
  - DOM node 에 접근하여 편집을 하면 DOM 이 업데이트 되는데 비용이 많이든다.
  - 새로운 node 를 추가하면 DOM 에 해당 node 를 추가하여 업데이트 해줘야하며, 만약 이러한 업데이트로 인해 레이아웃에 대한 변화가 생기면 웹페이지 일부 또는 전체를 다시 랜더링 될 수 있다.(reflow, layout)
- **virtual DOM** : real DOM 을 추상화한 DOM -> real DOM 에 사본정도로 이해
  - DOM 조작 및 업데이트에 대해 성능 최적화를 하고자 등장
  - document.createDocumentFragment() -> 가상돔도 결국에는 real DOM 에 반영을 해줘야하기 때문에 createDocumentFragment() 호출한다.
  - 결국에는 가상돔은 DOM 조작 및 업데이트를 자동화 해주는 수단으로 이해해도 된다.
  - react 에서는 2개의 가상돔을 비교(diffing)하여, real DOM 에 변경사항을 그룹화 하여 수행한다.

**2. react 란?**

- 페이스북에서 개발하고 관리하는 UI 를 만들기 위한 javascript 라이브러리다.

**3. react 특징은?**

- 단방향 데이터 흐름 : 데이터를 추적하기 쉽고, 디버깅을 쉽게 해줌
- virtual DOM : 가상돔을 사용하여, DOM 변경 시 필요한 최소한만 갱신하게 하여 성능 개선
- UI Component 기반 : UI 를 컴포넌트로 쪼개어 재사용성 및 유지보수 이점을 취함

**4. JSX 란?**

- JavaScript XML 의 약자다.
- JSX 는 **React.createElement(component, props, ...children) 생성한다.**
- JSX 는 자바스크립트로 HTML 코드 작성을 쉽게 도와주는 문법(템플릿 언어는 아니다)

**5. HOC(High-Order-Component) 란?**

- 컴포넌트 로직을 재사용하기 위한 기술
- 컴포넌트를 받아, 컴포넌트를 반환함
- HOC 접두사는 with 로 시작하는게 관행
  const HOC = ReactComponent => EnhancedReactComponent;
  or
  const HOC = (ReactComponent) => {return ReactComponent}

**6. FLUX 설명**

- 프론트엔드에서 적용된 MVC 패턴에 대한 문제로 나온 패턴

(양방향, 규모가 클수록 데이터가 어떻게 변경되는가를 추적하기 어렵고 많은 Model 전부를 제어하는것도 어려워짐, View 와 Model 의 관계가 복잡해짐)

- 단방향 데이터 흐름 모델의 개념을 따르는 아키텍쳐

![https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMTky/MDAxNTg1NDc1NDk5NDU2.tOa9Fx0-ZPs9IQO1EVmIJyTk_OTj1TGSzARbP2wlFSkg.sHJpaq1oE_Sn5Dp9ucd9SDusc-BWd_DoWRjcI3f_iXog.PNG.z1004man/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C.png?type=w800](https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMTky/MDAxNTg1NDc1NDk5NDU2.tOa9Fx0-ZPs9IQO1EVmIJyTk_OTj1TGSzARbP2wlFSkg.sHJpaq1oE_Sn5Dp9ucd9SDusc-BWd_DoWRjcI3f_iXog.PNG.z1004man/%EB%8B%A4%EC%9A%B4%EB%A1%9C%EB%93%9C.png?type=w800)

![https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMTYy/MDAxNTg1NDc1NTU1MTgx.TbVja4Up5WeDv7KvmVwC94t1B6faMFTaIezq07vB29Eg.pnzkyiPWZ3GiAbbW6zQqr5pMTL8M-7L-XuhVB32x1NAg.PNG.z1004man/flux-528x174.png?type=w800](https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMTYy/MDAxNTg1NDc1NTU1MTgx.TbVja4Up5WeDv7KvmVwC94t1B6faMFTaIezq07vB29Eg.pnzkyiPWZ3GiAbbW6zQqr5pMTL8M-7L-XuhVB32x1NAg.PNG.z1004man/flux-528x174.png?type=w800)

- flux 에서는 UI 는 데이터를 전달받기만 하면 된다
- UI 쪽에 데이터를 변경할때마다 직접 Store 와 동기화를 하는게 아닌, action 을 일으켜 store 에 변경사항을 업데이트해주고 그 변경사항을 UI 에 전달해준다.
- > 가장 큰 장점은 한방향으로 흐르기 때문에 추적이 쉽고 예측가능하다는 점이다

**7. Redux 란?**

- Flux 아키텍처를 기반으로 단방향 데이터 흐름 상태관리 라이브러리

![https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMjIz/MDAxNTg1NDc1OTgwMDI1.S0ztwC7ZluPxHlDKRZ09Bq2jfSKKhsIFBUC7veGr4AUg.MM0pIwKoSNrn8NSNzlnOdZGqQ1LAhrL_ST-mCqZ0WY4g.PNG.z1004man/Image3-43.png?type=w800](https://mblogthumb-phinf.pstatic.net/MjAyMDAzMjlfMjIz/MDAxNTg1NDc1OTgwMDI1.S0ztwC7ZluPxHlDKRZ09Bq2jfSKKhsIFBUC7veGr4AUg.MM0pIwKoSNrn8NSNzlnOdZGqQ1LAhrL_ST-mCqZ0WY4g.PNG.z1004man/Image3-43.png?type=w800)

- action : UI 에서 상태변경이 일어난 모든 사건 (dispatch)
- reducer : 사건에 따른 상태값에 대한 변화를 일으킨다 (mutation)
- store : 상태값들이 있는 저장소

**8. Redux 3가지 원칙**

- **single source of truth(하나의 진실)** : redux 는 애플리케이션 상태를 한곳에서 관리하기 위해 단 한개의 store 만을 사용(flux 는 여러개의 스토어 사용가능)
- **state is readOnly(상태는 읽기전용)** : View 에서 state 를 직접 접근하여 변경할 수 없다
- **changes are made with Pure Functions(변화는 순수함수로 만들어져야한다)** : reducer 는 순수함수(pure function) 으로만 되야한다는 의미이다 -> side-effect(부수효과) 가 없는 함수를 의미

**9. Redux-sage 란?**

- 애플리케이션에서 Redux 사용시 발생가능한 side-effect(사이드 이펙트 : 부작용) 을 쉽게 관리하고자 사용하는 라이브러리
- 여기서 말하는 부작용은 비동기 로직, Axios Call, request success/fail 처리 등이다.

**10. 함수형 프로그래밍(FP)이란?**

함수형 프로그래밍은
Function - 함수를 이용해서
No Side-Effect - 사이드 이펙트 없도록
Declarative Programming - 선언형 프로그래밍을 이용하는 것

함수는 인풋과 아웃풋이 있고(입력과 출력이 없을수도 있지만..),
각각의 인풋과 아웃풋이 연결이 되어 하나의 커다란 아웃풋을 만들게 되며 연결되게 됨.
순수 함수는 항상 동일한 인풋에 대해 동일한 아웃풋을 낸다. 그래서 상태를 가지지 않음.
-> 항상 동일한 출력을 한다

// 선언형 패턴
const shortNames = names.filter(name => name.length < 5);

- 함수는 재사용 가능하도록 설계된 프로그램 코드의 집합
- 결국에는 순수함수들을 조합하여 애플리케이션을 만드는 방식
- 선언형 프로그래밍은 무엇(What)을 할 것인가를 표현

**11. 상태가 없는 컴포넌트와 상태가있는 컴포넌트에 대한 설명**

상태가 없는 컴포넌트(stateless component)

- 내부적으로 state 를 가지지 않는 컴포넌트
- 기계적으로 부작용이 없다는 것이 보증 + 컴포넌트가 알 수 없는 상태에 따라 동작이 바뀌지 않음

상태가 있는 컴포넌트(satateful component)

- 내부적으로 state 가지고 있는 컴포넌트
- UI 와 관련된 state 를가짐(특정 UI에서 props 를 통해 toggle 하는게 아닌, 자신이 직업 toggle 관련 state를 관리)
