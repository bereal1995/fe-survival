---
description: React Hooks에 대해 알아보자
---

# React Hooks

## React Hook 이란

React 16.8에서 새로 추가된 기능으로, 함수형 컴포넌트에서도 상태 관리를 할 수 있는 useState, 렌더링 직후 작업을 설정하는 useEffect 등의 기능을 제공하여 기존의 함수형 컴포넌트에서 할 수 없었던 다양한 작업을 할 수 있게 해준다.

### 기존의 문제점

- Wrapper Hell: 컴포넌트를 감싸는 컴포넌트가 많아지면서 발생하는 문제
  - HOC(Higher Order Component)를 사용하면서 컴포넌트를 감싸는 컴포넌트가 많아지면서 발생했던 문제가 있다.
    - 예를 들어, withRouter를 사용하면 컴포넌트를 감싸는 컴포넌트가 많아지는데, 이러한 컴포넌트들이 많아지면서 컴포넌트의 구조가 복잡해지고, 디버깅이 어려워진다.
- Huge Component: 컴포넌트가 커지면서 발생하는 문제
  - 컴포넌트의 재사용성이 떨어진다.
  - 컴포넌트의 역할이 많아지고, 컴포넌트의 역할이 많아지면서 컴포넌트의 역할을 파악하기 어려워진다.
- Confusing Classes: 클래스형 컴포넌트를 사용하면서 발생하는 문제
  - 클래스형 컴포넌트를 사용하면서 발생하는 문제는 this가 무엇을 가리키는지 헷갈리는 문제가 있다.

### 현재

- Function Component 사용
  - 클래스형 컴포넌트를 사용하지 않고, 함수형 컴포넌트를 사용하면서 컴포넌트의 재사용성을 높일 수 있다.
- 복잡한 컴포넌트를 작은 컴포넌트 혹은 커스텀 훅으로 분리
  - 컴포넌트를 작은 컴포넌트 혹은 커스텀 훅으로 분리하면서 컴포넌트의 역할을 파악하기 쉽고, 컴포넌트의 재사용성을 높일 수 있다.
- 상태 관리 유무를 바로 알기 어려움

## Hooks

### useState

state를 사용할 수 있게 해주는 Hook이다.

```jsx
const [state, setState] = useState(initialState);
```

- state: 컴포넌트에서 사용할 상태
- setState: state를 변경할 수 있는 함수

### useEffect

컴포넌트가 렌더링 될 때마다 특정 작업을 수행하도록 설정할 수 있는 Hook이다.

```jsx
useEffect(() => {
  // componentDidMount, componentDidUpdate 역할을 수행
  return () => {
    // componentWillUnmount 역할을 수행
  };
}, [deps]);
```

- 첫 번째 파라미터: 함수
  - 컴포넌트가 렌더링 될 때마다 실행된다.
  - 두 번째 파라미터에 빈 배열을 넣으면 컴포넌트가 처음 렌더링 될 때만 실행된다.
  - 두 번째 파라미터에 특정 값을 넣으면 컴포넌트가 처음 렌더링 될 때와 특정 값이 변경될 때만 실행된다.
- 두 번째 파라미터: deps
  - deps에 특정 값을 넣으면 컴포넌트가 처음 렌더링 될 때와 특정 값이 변경될 때만 실행된다.
  - deps에 빈 배열을 넣으면 컴포넌트가 처음 렌더링 될 때만 실행된다.
  - deps를 생략하면 컴포넌트가 렌더링 될 때마다 실행된다.
- useEffect는 여러 번 사용할 수 있다.

### useContext

props를 통해 전달받지 않고도 전역적으로 상태를 사용할 수 있게 해주는 Hook이다.  
상위 컴포넌트에서 하위 컴포넌트로 props를 전달하지 않아도 된다.  

```jsx
const value = useContext(MyContext);
```

- MyContext: React.createContext()로 만든 Context 객체
- value: Context의 Provider의 value prop에 지정한 값

### useRef

DOM 노드 혹은 컴포넌트 안의 변수를 담을 수 있는 Ref 객체를 만들어 준다.  
렌더링과 관련되지 않은 값을 관리할 때 사용한다.

```jsx
const refContainer = useRef(initialValue);
```

- initialValue: Ref 객체의 초기값
- refContainer.current: Ref 객체의 현재값

### useLayoutEffect

useEffect와 동일한 기능을 수행하지만, 브라우저가 repaint 전에 실행된다.  
useLayoutEffect는 성능을 저하시킬 수 있기 때문에, 웬만하면 useEffect를 사용하는 것이 좋다.

```jsx
useLayoutEffect(() => {
  // componentDidMount, componentDidUpdate 역할을 수행
  return () => {
    // componentWillUnmount 역할을 수행
  };
}, [deps]);
```

- 첫 번째 파라미터: 함수
  - 컴포넌트가 렌더링 될 때마다 실행된다.
  - 두 번째 파라미터에 빈 배열을 넣으면 컴포넌트가 처음 렌더링 될 때만 실행된다.
  - 두 번째 파라미터에 특정 값을 넣으면 컴포넌트가 처음 렌더링 될 때와 특정 값이 변경될 때만 실행된다.
- 두 번째 파라미터: deps
  - deps에 특정 값을 넣으면 컴포넌트가 처음 렌더링 될 때와 특정 값이 변경될 때만 실행된다.
  - deps에 빈 배열을 넣으면 컴포넌트가 처음 렌더링 될 때만 실행된다.
  - deps를 생략하면 컴포넌트가 렌더링 될 때마다 실행된다.

## React StrictMode 란

React StrictMode는 애플리케이션 내의 잠재적인 문제를 알아내기 위한 도구이다.

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
- 자식 컴포넌트들에 대해서도 적용된다.
  - root에서만 사용가능한게 아니라, 부분적으로 사용 가능하다.
- 컴포넌트의 부수 효과를 두 번 호출하는 것을 감지한다.
  - 렌더링으로 인한 버그를 찾기 위해 [이중으로 렌더링을 수행한다.](https://react.dev/reference/react/StrictMode#fixing-bugs-found-by-double-rendering-in-development) (콘솔이 두번 찍히는 이유)
- 더 이상 지원되지 않는 API를 사용하는 것을 [방지한다.](https://react.dev/reference/react/StrictMode#fixing-deprecation-warnings-enabled-by-strict-mode)
