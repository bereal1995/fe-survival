---
description: useRef & CustomHook에 대해 알아보자.
---

# useRef & CustomHook

## useRef

- `useRef`는 `DOM`을 직접 선택할 수 있게 해주는 `Hook`이다.
- `useRef`를 사용하여 `DOM`을 선택하면 `ref` 객체가 반환된다.
- `ref` 객체의 `current` 속성을 통해 `DOM`에 접근할 수 있다.
- `useRef`로 관리하는 변수는 값이 바뀌어도 컴포넌트가 리렌더링되지 않는다.
- 꼭 `DOM`에만 사용할 필요는 없다.
  - `setTimeout`, `setInterval`의 `id`를 관리할 때도 사용할 수 있다.
  - react에서 렌더링에 영향을 주지 않는 값을 관리할 때도 사용할 수 있다.
  - react의 렌더링과 브라우저의 렌더링이 동기화되지 않는 경우에도 사용할 수 있다.

```jsx
import React, { useRef } from 'react';

const RefSample = () => {
  const id = useRef(1);
  const setId = (n) => {
    id.current = n;
  };
  const printId = () => {
    console.log(id.current);
  };
  return <div>refSample</div>;
};

export default RefSample;
```

## Hook의 규칙

- `함수형 컴포넌트`에서만 호출해야 한다.
- `컴포넌트의 Top level`에서만 호출해야 한다.
  - 반복문, 조건문, 중첩된 함수 내에서 호출하면 안된다.
- `함수형 컴포넌트` 내부에서만 호출해야 한다.

## Custom Hook

- `Hook`을 사용하여 만든 함수이다.
- `함수명`이 `use`로 시작해야 한다.
- Custom Hook도 Hook의 규칙을 따라야 한다.

```jsx
import React, { useState, useEffect } from 'react';

const useTitle = (initialTitle) => {
  const [title, setTitle] = useState(initialTitle);
  const updateTitle = () => {
    const htmlTitle = document.querySelector('title');
    htmlTitle.innerText = title;
  };
  useEffect(updateTitle, [title]);
  return setTitle;
};

const UseTitle = () => {
  const titleUpdater = useTitle('Loading...');
  setTimeout(() => titleUpdater('Home'), 5000);
  return <div>useTitle</div>;
};

export default UseTitle;
```
