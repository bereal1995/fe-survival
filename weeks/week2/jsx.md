---
description: JSX에 대해 알아보자
---

# JSX (JavaScript XML)

## jsx란 ?

*JSX* 는 JavaScript 파일 내에서 HTML과 유사한 마크업을 작성할 수 있는 JavaScript용 구문 확장. 구성 요소를 작성하는 다른 방법이 있지만 대부분의 React 개발자는 JSX의 간결함을 선호하고 대부분의 코드베이스에서 이를 사용한다.

- JSX는 특정함수 호출로 컴파일되며, 컴파일러는 JSX를 사용하여 정적 구조를 분석하고 중첩된 구성 요소를 생성
- `/** @jsx {blah blah} */`처럼 주석을 사용하여 호출할 수 있는 다른 함수를 지정할 수도 있다.
- React에서 많이 사용하지만, React에 종속적이지 않다.
- jsx 규칙
  - 단일 루트 요소 반환
    - 구성 요소에서 여러 요소를 반환하려면 **단일 상위 태그로 요소를 래핑해야한다.** (예: `<div>`, `<section>`, `<article>`, `<React.Fragment>`, 또는 `<></>`)
    - 래핑을 해야하는 이유
      - JSX는 HTML처럼 보이지만 내부적으로는 일반 JavaScript 객체로 변환되는데 여러 개의 루트 요소를 반환하면 React가 어떤 것을 렌더링해야 할지 알 수 없다.
  - 모든 태그 닫기
    - JSX에서는 태그가 명시적으로 닫혀야 한다. `<img>`같은 자동 닫힘 태그 `<img />`와 `<li>oranges` 같은 래핑 태그는 `<li>oranges</li>`로 작성되어야 한다.
  - 카멜케이스
    - JavaScript에는 변수 이름에 대한 제한이 있기때문에 하이픈으로 작성이 불가능
    - 역사적인 이유로 `[aria-*](https://developer.mozilla.org/docs/Web/Accessibility/ARIA)`및 `[data-*](https://developer.mozilla.org/docs/Learn/HTML/Howto/Use_data_attributes)`속성은 대시가 있는 HTML로 작성된다.

## React에서 JSX를 사용하는 목적

JSX는 React.createElement() 호출로 컴파일되며, 컴파일러는 JSX를 사용하여 정적 구조를 분석하고 중첩된 구성 요소를 생성한다.

- 선언적으로 UI를 만들 수 있다.
- 컴포넌트의 구조를 쉽게 파악할 수 있다.
- JSX를 사용하여 ReactElement를 만들 수 있다.
- 리액트에서는 기존에는 html과 javascript로 분리되어 유지보수가 어려웠던 부분을 JSX를 통해 JavaScript에서 마크업을 작성해 렌더링 로직과 마크업이 같은 위치에서 작성할 수 있는 장점을 소개해준다.

## 선언적으로 UI를 만들면 뭐가 좋을까?

선언적으로 UI를 만들면 코드가 더 유지보수하기 쉽고, 가독성이 좋아집니다. 또한, 개발자 도구를 사용하여 디버깅할 때 도움이 됩니다.  
예를 들어, 다음과 같은 코드가 있다고 가정해봅시다.

```jsx
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

이 코드는 다음과 같이 컴파일됩니다.

```jsx

const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

만약 JSX를 사용하지 않는다면, 위 코드를 작성하고 유지보수를 해야하는 것인데, 지금은 단순한 구조이지만 여러 컴포넌트가 중첩되어 있고 복잡한 구조라면 코드를 작성하고 유지보수하는데 어려움이 있을 것입니다.

## Syntactic sugar

요약 문법과 의미를 바꾸지 않으면서도 새로운 기능을 기존에 있는 기능으로 표현함으로써 언어에 추가하는 것

- 자바스크립트를 공부하다보면 자주 만나는 키워드이다.
- jsx관점에서 보았을 경우 친숙한 문법(마크업)으로 작성하고 React함수(createElement)로 변환(jsx컴파일러)을 해주는 작업을 문법적 설탕으로 볼 수 있다.

## React.createElement

- 리액트 엘리먼트를 만드는 함수
- jsx를 사용안하는 경우 대안으로 사용가능
- type, props, optional(…children) 총 3가지의 매개변수를 받는다

## React Element

- 리액트 앱의 가장 작은 단위
- 일반 객체이며, React.createElement()로 생성된다.
- 실제 DOM 엘리먼트와 다르다.
- 생성한 이후에는 자식을 추가하거나 속성을 변경할 수 없다. (불변객체)
- 컴포넌트와는 다른개념이며 리액트 엘리먼트는 컴포넌트의 구성요소이다.

## React StrictMode

개발 초기에 구성 요소의 일반적인 버그를 찾을 수 있도록 도와준다.  
아래처럼 활성화 할 수 있다.

```jsx
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

- 개발 모드에서만 활성화되며, 프로덕션 빌드에는 영향을 주지 않는다.
- 렌더링으로 인한 버그를 찾기 위해 [이중으로 렌더링을 수행한다.](https://react.dev/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development) (콘솔이 두번 찍히는 이유)
- 더 이상 지원되지 않는 API를 사용하는 것을 [방지한다.](https://react.dev/reference/react/StrictMode#fixing-deprecation-warnings-enabled-by-strict-mode)
- root에서만 사용가능한게 아니라, 부분적으로 사용 가능하다.

## VDOM(Virtual DOM)이란?

- DOM이란?
  - Document Object Model의 약자로, HTML, XML 문서의 프로그래밍 interface이다.
  - DOM은 문서의 구조화된 표현을 제공하며, 프로그래밍 언어가 DOM 구조에 접근할 수 있는 방법을 제공하여 그들이 문서 구조, 스타일, 내용 등을 변경할 수 있게 돕는다.
  - 일반적으로 DOM은 트리 형태로 구성되어 있으며, DOM의 최상위 노드를 문서 노드(Document Node)라고 한다.
  - JavaScript를 사용하여 DOM에 접근하여 웹페이지를 조작하거나 필요한 정보를 추출할 수 있다.
- DOM과 Virtual DOM의 차이
  - VDOM은 DOM을 모방하여 만든 객체이기 때문에 DOM과 유사한 구조를 가지고 있다.
  - DOM은 브라우저가 HTML을 파싱하여 만든 객체이고, Virtual DOM은 JavaScript 객체이다.
  - DOM은 브라우저가 HTML을 파싱하여 만든 객체이기 때문에 브라우저에서 제공하는 API를 사용할 수 있다. 반면에 Virtual DOM은 JavaScript 객체이기 때문에 브라우저에서 제공하는 API를 사용할 수 없다.
  - VDOM은 메모리 상에서만 존재하고 실제 렌더링되는 것은 아니다. 실제 렌더링은 VDOM을 기반으로 DOM을 조작하여 이루어진다.
  - VDOM 역할은 실제 DOM과 동기화를 맞추는 것이다. 실제 DOM과 동기화를 맞추기 위해서는 실제 DOM에 접근하면 비용이 많이 들기 때문에 VDOM을 사용하여 동기화를 맞춘다.

## Reconciliation(재조정) 과정은 무엇인가?

React에서 선언적 API를 사용하면서 애플리케이션 작성이 쉬워졌지만, 그만큼 추상화가 되어있는 것이기 때문에 React내부에서 어떤일이 벌어지는지 명확히 알 수 없다.  
Reconciliation은 이 보이지 않는 과정 중에 하나이고, 렌더링과정에서 이전에 렌더링된 트리와 새로운 트리를 비교하여 변경된 부분만 실제 DOM에 반영하는 과정이다.

- React는 렌더링을 할 때마다 새로운 트리를 만들고 이전 트리와 비교한다.
- 이전 트리와 비교하여 변경된 부분만 실제 DOM에 반영한다.
- 이 과정에서 React 자체 비교 알고리즘을 사용하여 최소한의 업데이트만 수행하고 성능을 최적화한다.

## Shadow DOM이란?

Shadow DOM은 웹 컴포넌트의 일부로써, 웹 구성요소를 캡슐화하고 독립적으로 적용할 수 있게 해준다.

- 캡슐화를 통해 DOM 요소의 스타일과 구조를 다른 DOM 요소와 격리시킬 수 있다.
- React에서는 Shadow DOM 대신 Virtual DOM을 사용한다.
- Shadow DOM은 브라우저에서 제공하는 기능이기 때문에 브라우저에서 제공하는 API를 사용할 수 있다.
- 좀 더 큰 개념으로는 Web Component라고 하는데, Web Component는 Custom Element, Shadow DOM, HTML Template, HTML Imports로 구성되어 있다.
- DOM트리와는 별개로 존재하기 트리이기 때문에 DOM트리에 영향을 주지 않고 독립적으로 존재한다.

## Shadow DOM과 Virtual DOM의 차이

Shadow DOM과 Virtual DOM은 다른 목적으로 사용되는 기술이고, 둘은 별개의 개념이다. 그럼에도 불구하고 아래와 같은 점들 때문에 비교가 되는 것 같다.

- 컴포넌트 기반 접근 방식을 지원한다.
- UI 구성요소를 캡슐화하여 독립적으로 적용할 수 있게 해준다.
- 재사용 가능한 컴포넌트를 만들고 관리할 수 있다.
- 유지보수와 생산성을 높여준다.

|---|Shadow DOM|Virtual DOM|
|---|---|---|
|개념|DOM을 캡슐화하여 독립적으로 적용할 수 있게 해주는 브라우저 기능|DOM을 모방하여 만든 JavaScript 객체|
|목적|요소를 캡슐화하여 나머지 페이지의 영향으로부터 요소를 보호|성능 최적화|
|주체|브라우저|React와 같은 VDOM을 사용하는 프레임워크|

## 궁금한점

- JSX에서 여러규칙중에서 카멜케이스(CamelCase) 규칙을 따르는 이유가 무엇인가요?
- React의 StrictMode는 항상 사용하는것이 좋을까요 ??
  - 사용하지 않는다면 어떤 경우에 사용하는 것이 좋을까요?
- 이번에 web component라는 키워드를 알게 되었는데 깊게 공부해보면 좋을까요?
