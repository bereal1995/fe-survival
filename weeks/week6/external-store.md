---
description: External Store에 대해 알아보자
---

# External Store

## [관심사의 분리](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)

Separation of Concerns, SoC  
컴퓨터 프로그램을 구별된 부분으로 분리하는 디자인 원칙, 각 부분은 다른 부분에 영향을 주지 않고 독립적으로 개개의 관심사에만 집중할 수 있어야 한다.

- 관심사의 분리를 이용하면 설계가 단순해지고, 유지보수가 쉬워진다.
- 단순화 및 유지보수가 쉬워지므로, 코드의 재사용성이 높아진다.
- 관심사의 분리는 추상화의 일종
- 추상화를 해야하기 때문에, 인터페이스가 필요하고, 실행에 따른 불필요한 오버헤드가 발생할 수 있다.
- 복잡한 시스템을 수정할 때, 하나의 관심사만 수정하면 되므로, 자유도가 높아진다.

## Layered Architecture

Layered Architecture는 관심사의 분리를 위해 사용되는 아키텍처 패턴 중 하나이다.  
Layered Architecture는 계층화된 구조를 가지며, 각 계층은 하위 계층의 기능을 사용할 수 있지만, 상위 계층의 기능은 사용할 수 없다.  
통상 아래 3가지로 구성된다.

### 계층

- Presentation Layer
  - 사용자와 직접적으로 상호작용하는 계층
  - 사용자의 요청을 받아 처리하는 역할
  - 사용자에게 결과를 보여주는 역할
- Business Layer
  - Presentation Layer와 Data Layer 사이에서 비즈니스 로직을 처리하는 계층
  - Presentation Layer로부터 요청을 받아 처리하는 역할
  - Data Layer로부터 받은 데이터를 가공하여 Presentation Layer에 전달하는 역할
- Data Layer
  - 데이터를 저장하고 관리하는 계층
  - 데이터베이스와 같은 저장소에 접근하여 데이터를 저장하고, 가져오는 역할

시스템의 복잡도에 따라 계층을 더 세분화할 수 있다.  
결국 계층화된 구조를 가지는 시스템은 각 계층이 독립적으로 동작하므로, 관심사의 분리를 할 수 있다.

## [Flux Architecture](https://haruair.github.io/flux/docs/overview.html)

Flux Architecture는 meta(Facebook)에서 개발한 아키텍처 패턴이다.  
클라이언트 사이드 웹 애플리케이션을 위해 설계되었으며, React와 함께 사용하기 위해 만들어졌다.  
Flux Architecture는 Layered Architecture를 기반으로 하고 있으며, 단방향 데이터 흐름을 가지고 있다.  

- React는 View를 담당하고, Flux Architecture는 나머지 계층을 담당한다.
- Layered Architecture를 기반으로 하고 있으므로, 각 계층은 독립적으로 동작한다.
- 단방향 데이터 흐름을 가지고 있으므로, 데이터의 흐름을 추적하기 쉽다.

### 핵심 3가지 부분

보통 사이클은 Dispatcher -> Store -> View -> Dispatcher 순으로 진행된다. (단방향 데이터 흐름)

- Dispatcher
  - Action을 Store에 전달하는 역할
  - Action을 Store에 전달할 때, Action의 Type을 확인하여 Store를 구분한다.
- Store
  - Action을 받아 처리하는 역할
  - Action을 받아 처리할 때, Action의 Type을 확인하여 처리한다.
  - 처리가 완료되면, View에게 변경된 데이터를 전달한다.
- View
  - Store로부터 데이터를 받아 화면에 보여주는 역할
  - 사용자의 입력을 받아 Dispatcher에게 전달하는 역할

## [Redux 핵심](https://ko.redux.js.org/tutorials/essentials/part-1-overview-concepts/)

Redux는 Flux Architecture를 기반으로 한 상태 관리 라이브러리이다.  
Redux는 Flux Architecture의 핵심 3가지 부분을 다음과 같이 구현하고 있다.  

- Action
  - Store에게 전달할 데이터를 가지고 있는 객체
  - Type을 가지고 있다.
- Reducer
  - Action을 받아 처리하는 함수
  - Action을 받아 처리할 때, Action의 Type을 확인하여 처리한다.
  - 처리가 완료되면, Store에게 변경된 데이터를 전달한다.
- Store
  - Reducer를 통해 데이터를 관리하는 객체
  - Reducer를 통해 데이터를 관리할 때, Action의 Type을 확인하여 처리한다.
  - 처리가 완료되면, View에게 변경된 데이터를 전달한다.

## useReducer

기존 클래스 컴포넌트에서는 forceUpdate를 통해 상태를 강제로 업데이트할 수 있었다.  
함수 컴포넌트에서는 forceUpdate가 없으므로, useReducer를 통해 상태를 강제로 업데이트할 수 있다.

```tsx
// useForceUpdate.ts
import {useCallback, useState} from 'react';

export default function useForceUpdate() {
  const [, setState] = useState({});
  return useCallback(() => {
    setState({});
  }, []);
}

// Counter.tsx
// Business Logic
const state = {
  count: 0,
};

function increase() {
  state.count += 1;
}

function decrease() {
  state.count -= 1;
}

// UI
export default function Counter() {
  const forceUpdate = useForceUpdate();

  const handleClickIncrease = () => {
    increase();
    forceUpdate();
  };

  return (
    <div>
      <h1>카운터</h1>
      <p>현재 카운트: {state.count}</p>
      <button onClick={handleClickIncrease}>Jump</button>
    </div>
  );
}
```

- useReducer는 상태를 강제로 업데이트할 수 있지만, 권장되지 않는다.
- forceUpdate를 설명하는 이유는 External Store들도 결국에는 상태를 React에 맞게 업데이트 해야하는데, 이를 위해 forceUpdate를 사용할 수 있기 때문이다.
- 위 코드에서는 Business Logic과 UI를 주석으로 구분했는데, 이는 External Store를 사용할 때, Business Logic과 UI를 분리하는 것을 추천하기 때문이다.

### Business Logic과 UI를 분리하는 이유

- Business Logic과 UI를 분리하면, UI를 변경할 때, Business Logic을 변경할 필요가 없다. (즉 관심사의 분리)
- Business Logic은 UI에 비해 변경이 적다. (유지보수)
- 관심사의 분리가 잘되어 있으면, 테스트하기 쉽다. (유지보수)

## useCallback

- 함수를 캐싱하는 Hook
- 함수를 캐싱하기 때문에, 함수를 매번 새로 생성하지 않고 dependency가 변경될 때, 함수를 새로 생성한다.
- 위 코드에서는 useCallback을 이용해서 useForceUpdate훅을 여러곳에서 사용해도, useForceUpdate훅이 한번만 생성된다.
  - useForceUpdate훅이 한번만 생성되므로, forceUpdate가 호출될 때, 리렌더링이 발생한다.
  - useForceUpdate을 여러곳에서 사용하더라도, useCallback함수는 한번만 생성되므로, forceUpdate가 호출될 때, 리렌더링이 발생한다.

## 마무리 글

관심사의 분리, Layered Architecture, Flux Architecture, Redux, useReducer, useCallback에 대해 알아보았다.  
개념적으로 보면 관심사의분리 안에 Layered Architecture가 있고, Layered Architecture 안에 Flux Architecture가 있으며, Flux Architecture 안에 Redux가 있다.  
결국, 관심사의분리를 잘하기 위해서 나온 아키텍처들이라고 생각된다.  
관심사의 분리도 결국 코드를 쉽게 이해하고, 유지보수하기 위한 방법인데, 이것을 위해서 많은 사람들이 여러고민을 하고 있고 여러가지 아키텍처를 보면서 개발에서 설계와 유지보수가 얼마나 중요한지 느낄 수 있었다.

- 관심사의 분리는 추상화의 일종으로 복잡한 시스템을 단순화하고, 유지보수를 쉽게한다.
- Layered Architecture는 계층화된 구조를 가지고, 각 계층은 독립적으로 동작한다.
- Flux Architecture는 Layered Architecture를 기반으로 하고 있으며, 단방향 데이터 흐름을 가지고 있다.
- Redux는 Flux Architecture를 기반으로 한 상태 관리 라이브러리이다.
- useReducer는 상태를 강제로 업데이트할 수 있지만, 권장되지 않는다.
- useCallback은 함수를 캐싱하는 Hook이다.
- 결국 모든것은 관심사의분리를 잘하기 위해...
