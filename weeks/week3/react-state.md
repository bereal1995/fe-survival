---
description: React State 대해 알아보자
---

# React State

## React state란?

React는 state기반으로 동작한다.
React 컴포넌트는 state를 가질 수 있고, 이 상태는 변할 수 있다.  
이러한 상태를 React state라고 한다.

state의 조건

- state는 변경되어야 한다 변경되지 않는다면 굳이 state로 만들 필요가 없다.
- props를 통해 전달되면 state가 아니라 props로 만들어야 한다.
- 다른 state나 props를 이용해 계산될 수 있다면 굳이 state로 만들 필요가 없다.

react는 state기반으로 동작하기 때문에 state를 최소화하여 사용하는 것이 react를 자유롭게 다룰 수 있는 방법이다.

## DRY 원칙

DRY(Don't Repeat Yourself) 원칙은 중복을 제거하라는 원칙이다.

- 중복을 제거하면 코드가 간결해지고, 유지보수가 쉬워진다.
- 중복을 제거하면 코드의 양이 줄어들고, 버그가 줄어든다.
- 중복을 제거하면 코드의 가독성이 높아진다.

## SSOT(Single Source of Truth)

SSOT(Single Source of Truth)는 단일 진실의 공급원이라는 의미이다.

- React 컴포넌트는 state를 가질 수 있다.
- React 컴포넌트는 state를 가지고 UI를 렌더링한다.

## useState

React에서 state를 사용하기 위해서는 useState를 사용해야 한다.

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>현재 카운트: {count}</p>
      <button onClick={() => setCount(count + 1)}>증가</button>
      <button onClick={() => setCount(count - 1)}>감소</button>
    </div>
  );
}
```

useState는 배열을 반환한다.

- 첫 번째 원소는 state이다.
- 두 번째 원소는 state를 변경하는 함수이다.

## 1급 객체(first-class object)란?

1급 객체란 다음의 조건을 만족하는 객체를 말한다.

- 변수에 담을 수 있다.
- 인자로 전달할 수 있다.
- 반환값으로 전달할 수 있다.

## Lifting State Up

Lifting State Up은 state를 최소화하기 위한 방법이다.

- state를 최소화하면 코드의 양이 줄어들고, 버그가 줄어든다.
- state를 최소화하면 코드의 가독성이 높아진다.

## You Might Not Need an Effect Hook

Effect Hook은 컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.  
Effect를 통해 외부 시스템과 동기화 작업을 할 수 있는데, 관련된 외부 시스템이 없는 경우에는 Effect Hook을 사용할 필요가 없다.

- Effect를 사용하면 React 컴포넌트가 렌더링 될 때마다 특정 작업을 수행할 수 있다.
- 불필요한 Effect가 존재하면 컴포넌트 렌더링을 예측하기 어려워지고, 버그가 발생할 가능성이 높아진다.
- 불필요한 Effect를 제거하면 코드를 읽기 쉬워지고, 버그가 줄어든다.
- React를 자유롭게 핸들링하기 위해서는 Effect를 최소화해야 한다.
