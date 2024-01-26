---
layout: post
title: "CSS - Reflow & Repaint"
date: 2024-01-25 10:33:00 +0100
categories: ["study", "CSS"]
published: true
---

## **Reflow & Repaint(Redraw)**

화면에 모든 요소가 그려지고 나서 사용자 인터랙션 또는 페이지의 기능에 따라 일부 변경이 생기는 경우에 발생하는 현상이다.

### **Reflow**

노드의 크기 또는 위치가 변경되어 현재 레이아웃에 영향을 미쳐서 배치를 다시 해야하는 경우에 발생한다.

_Repaint 보다 심각한 성능 저하를 유발시킬 수 있다._

- Reflow가 발생하는 경우
  - 윈도우 리사이징
  - 노드의 추가 / 제거 / 수정
  - 요소의 위치 변경
  - 요소의 크기 변경 (margin, padding, border, width, heght …)
  - 폰트 변경
  - 텍스트 내용 변경
  - 이미지 크기 변경
  - 페이지 초기 렌더링
  - 엘리먼트에 대한 offsetWidth, offsetHeight 등과 같은 위치 값을 계산 시
- Reflow 발생 시의 이벤트 로그

### **Repaint (or Redraw)**

Reflow가 발생하거나 배경색 변경 등의 단순한 스타일 변경과 같은 작업에 발생한다. 즉, 화면의 레이아웃에는 영향을 미치지 않는 경우에 발생한다.

- Repaint가 발생하는 경우
  - 특정 엘리먼트의 색상 값 변화
- Repaint 발생 시의 이벤트 로그

### **리플로 최소화 방법**

렌더링 성능 향상을 위해서는 먼저 Reflow를 줄여야한다. *Reflow를 최소화함으로써 렌더링 성능을 향상*시킬 수 있다.

### **클래스 변화에 따른 스타일 변경 시, 최대한 DOM 구조 상 끝단에 위치한 노드에 준다.**

스타일 변화가 발생할 경우, 일부 노드로 제한할 수 있다.

### **인라인 스타일을 최대한 배제한다.**

만약, 인라인 스타일이 없는 경우, 외부 스타일 클래스의 조합으로 단 한번만 Reflow를 발생시킬 수 있다. 즉, 코드 가독성과 Reflow 비용을 줄일 수 있다.

### **애니메이션이 들어간 노드는** `position: fixed` **또는** `position: absolute`**로 지정한다.**

position 속성을 “fixed” 또는 “absoute”로 값을 주면 지정된 노드는 전체 노드에서 분리된다. 즉, 전체 노드에 걸쳐 Reflow 비용이 들지 않고, 해당 노드의 Repaint 비용만 든다.

> 또는 노드의 position 값을 초기에 적용하지 않았더라도 에니메이션 시작 시 값을 변경(fixed, absolute)하고 종료 시 다시 원복 시키는 방법을 사용해도 비용을 줄일 수 있다.

### **퀄리티와 퍼포먼스 사이에서 타협하라.**

속도가 빠른 디바이스에서는 큰 차이가 없는 것으로 보일 수 있으나, 속도가 느린 디바이스에서는 그 차이가 클 수 있다.

### **테이블 레이아웃을 피하라.**

테이블로 구성된 페이지 레이아웃은 점진적(progressive) 페이지 렌더링이 적용되지 않으며, 모두 로드되고 계산(Recalculate)된 후에야 화면에 뿌려지게 된다. 하지만 해당 테이블에 table-layout:fixed 속성을 주는 것이 디폴트값인 auto에 비해 성능면에서 더 좋다

### **IE의 경우, CSS에서의 JS표현식을 피하라.**

Reflow가 발생할 때마다 표현식이 다시 계산되므로 비용이 발생할 수 밖에 없다.

### **CSS 하위선택자는 필요한 만큼만 정리하라.**

하위 선택자의 룰이 적을 수록 비용이 절감된다.

```
// DON'T
.section_service .list_service li .box_name .btn_more
{display:block;width:100px;height:30px;}

// DO
.section_service .list_service .btn_more
{display:block;width:100px;height:30px;}
```

### **position:relative 사용 시 주의하자.**

페이지를 새로 열거나 Reflow가 발생되어 CSS Calculation이 발생할 경우, Box model Calculation → Normal Flow 의 순서로 계산이 진행된다. (Normal flow는 Layout 또는 Reflow라 불리는 과정에 속하는 일부임.) 일반적인 경우, 엘리먼트 들은 margin, border, padding, content(width,height) 등 Box model을 먼저 계산한 후 Normal flow 상태의 레이아웃에 배치된다. (다른말로 선형적 배치)

### **작업 그룹화 하여 처리하기 (cssText 또는 클래스를 활용하라)**

DOM 요소의 정보를 요청하고 변경하는 코드는 같은 형태의 작업끼리 그룹화하여 실행한다.

**예제1 : cssText 또는 클래스명을 이용하여 수정한다.**

```
// DON'T - 최악의 경우 2번의 Reflow를 발생시킨다
let div = document.getElementById('box');
div.style.padding = '16px';
div.style.width = '600px';

// DO - 한 번만 돔 객체를 수정한다
// 1) cssText를 이용
div.style.cssText = 'padding: 16px; width: 600px;';

// 2) 클래스명을 이용
div.className += ' clsName';
```

**예제2 : 스타일 변경 작업을 그룹화하여 처리한다.**

```
// DON'T
let width = document.getElementById('box1').style.width;
document.getElementById('box2').style.width = width;

let height = document.getElementById('layer1').style.height;
document.getElementById('layer2').style.height = height;

// DO
let width = document.getElementById('box1').style.width;
let height = document.getElementById('layer1').style.height;

document.getElementById('box2').style.width = width;
document.getElementById('layer2').style.height = height;
```

### **캐쉬를 활용하여 Reflow를 최소화한다.**

브라우저는 레이아웃의 변경을 큐에 저장했다가 한 번에 실행하는 방법으로 Reflow를 줄인다. 하지만, `offset, scrollTop...`과 같은 계산된 스타일 정보를 요청할 때마다 정확한 정보를 제공하기 위해 큐를 비우고 모든 변경을 다시 적용한다. 그러므로 중복되는 수치에 대한 요청 수를 줄임으로 Reflow 비용을 최소화 할 수 있다.

```
function collect() {
  let el = document.getElementById('box');
    let width = el.style.width;
    return parseInt(cw, 10) * parseInt(cw + document.documentElement.clientWidth, 10);
}
```

### **DOM 사용을 최소화한다.**

노드 조각(document.createDocumentFragment), 노드 사본(el.cloneNode)를 활용하여 DOM 접근을 최소화한다.

```
// DON'T
function ex1BadCase() {
    var el = document.getElementById('container');
    for (var i = 0; i < 10; i++) {
        var a = document.createElement('a');
        a.href = '#';
        a.appendChild(document.createTextNode('test' + i));
        el.appendChild(a);
    }
    return false;
}

// DO - 노드 조각(document.createDocumentFragment)
function noReflow() {
    var frag = document.createDocumentFragment();
    for (var i = 0; i < 10; i++) {
        var a = document.createElement('a');
        a.href = '#';
        a.appendChild(document.createTextNode('test' + i));
        frag.appendChild(a);
    }
    document.getElementById('container').appendChild(frag);
    return false;
}

// DO - 노드 사본(el.cloneNode)
function noReflow() {
    var el = document.getElementById('container');
    var clone = el.cloneNode(true);

    for (var i = 0; i < 10; i++) {
        var a = document.createElement('a');
        a.href = '#';
        a.appendChild(document.createTextNode('test' + i));
        clone.appendChild(a);
    }
    el.appendChild(clone);
    return false;
}
```
