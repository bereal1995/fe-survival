---
description: Navigation에 대해 알아보자
---

# Navigation

## [Web APIs - History](https://developer.mozilla.org/en/docs/Web/API/History)

History 인터페이스는 브라우저의 세션 기록, 즉 현재 페이지를 불러온 탭 또는 프레임의 방문 기록을 조작할 수 있는 방법을 제공합니다.

### 속성

- History.length(읽기전용) : 현재 페이지를 포함해, 세션 기록의 길이를 나타내는 정수 반환
- History.scrollRestoration : 페이지가 이동되었을 때 스크롤 위치를 복원할지 여부, auto, manual만 가능
- History.state(읽기전용) : 기록 스택 최상단의 state를 반환, popstate 이벤트를 기다리지 않고 현재 기록의 state를 바로 읽을 수 있음

### 메서드

- History.back : 세션 기록에서 이전 페이지로 이동
  - 브라우저 `뒤로가기`버튼을 눌렀을때, `history.go(-1)`과 같다.
  - 첫번째 페이지에서 호출해도 오류가 발생하지 않고, 아무런 효과가 없다.
- History.forward : 세션 기록에서 다음 페이지로 이동
  - 브라우저 `앞으로가기`버튼을 눌렀을때, `history.go(1)`과 같다.
  - 마지막 페이지에서 호출해도 오류가 발생하지 않고, 아무런 효과가 없다.
- History.go : 세션 기록에서 특정 페이지로 이동
- History.pushState : 세션 기록에 새로운 state를 추가
  - state에는 직렬화 가능한 모든 객체를 넣을 수 있다.
  - Safari를 제외한 모든 브라우저는 `title` 매개변수를 무시
- History.replaceState : 세션 기록의 현재 state를 변경
  - state에는 직렬화 가능한 모든 객체를 넣을 수 있다.
  - Safari를 제외한 모든 브라우저는 `title` 매개변수를 무시

### [History.pushState](https://developer.mozilla.org/en/docs/Web/API/History/pushState)

이 메서드는 비동기로 동작한다.  
네비게이션이 언제 완료되었는지 결정하기 위해 popstate 이벤트 리스너를 추가한다.  
이때, state 매개변수를 사용할 수 있다.

```js
pushState(state, unused)
pushState(state, unused, url)
```

- state : 직렬화 가능한 객체
  - 사용자가 새로운 state로 이동할 때마다 `popstate` 이벤트의 `state` 속성에 저장된다.
  - 16MiB(16.7772MB) 크기 제한이 있다.
- unused : 무시
  - 이 매개변수는 역사적인 이유로 존재해서 생략할 수 없다.
  - 빈 문자열을 전달하는 것이 나중에 메서드가 변경될 경우에도 안전하다.
- url(optional) : 새로운 기록 항목의 URL
  - `pushState()`를 호출하면, 브라우저는 주소 표시줄의 URL을 업데이트하지 않는다.
  - url은 상대경로인 경우, 현재 URL을 기준으로 한다.
    - ex. 현재 URL이 `https://example.com/path1/path2`이고, `pushState(null, null, 'path3')`을 호출하면, `https://example.com/path1/path3`으로 이동한다.
  - url의 origin이 다른 경우, `SecurityError`가 발생한다.

### [popstate](https://developer.mozilla.org/en-US/docs/Web/API/Window/popstate_event)

사용자가 세션 기록을 탐색하는 동안 활성 기록 항목이 변경되면 popstate 이벤트가 발생한다.  
사용자가 마지막으로 방문한 페이지의 기록 항목으로 변경하거나, `history.pushState()`를 사용하여 기록 스택에 추가한 경우 해당 기록 항목이 대신 사용됩니다.

```js
addEventListener("popstate", (event) => {});
onpopstate = (event) => {};
```

- event.state : `pushState()` 또는 `replaceState()`를 호출할 때 전달한 state 객체의 복사본을 반환
- history.pushState() 또는 history.replaceState()를 호출한다고 popstate가 트리거 되지는 않는다.
- 브라우저 뒤로가기/앞으로가기 버튼을 눌렀을 때, `history.back()`, `history.forward()`, `history.go()`를 호출하면 popstate가 트리거 된다.
- 브라우저마다 페이지 로드시 이벤트를 다르게 처리한다.
  - Chrome(v34 이전), Safari: 페이지 로드시 popstate 이벤트가 트리거 된다.
  - Firefox: 페이지 로드시 popstate 이벤트가 트리거 되지 않는다.

## React Router - NavLink, Link, Navigate, useNavigate

### [NavLink](https://reactrouter.com/en/main/components/nav-link)

```tsx
function Header() {
  return (
    <header>
      <nav>
        <ul>
          <li><NavLink to="/">Home</NavLink></li>
          <li><NavLink to="/about">About</NavLink></li>
          <li>
            <NavLink
              to="/about"
              className={({ isActive, isPending }) =>
                isPending ? "pending" : isActive ? "active" : ""
              }
              style={({ isActive, isPending }) => {
                return {
                  fontWeight: isActive ? "bold" : "",
                  color: isPending ? "red" : "black",
                };
              }}
            >
              About
            </NavLink>
          </li>
        </ul>
      </nav>
    </header>
  );
}
```

- `NavLink`는 현재 경로와 일치하는 경우 특별한 스타일을 적용한다.
- `activeClassName`과 `activeStyle`을 사용하여 스타일을 지정할 수 있다.
- `end` prop을 사용하여 경로의 끝과 일치하는 경우에만 스타일을 적용할 수 있다.
- `style`, `className` prop을 사용하여 스타일을 지정할 수 있다.
  - isActive, isPending 값을 콜백으로 전달받아 스타일을 지정할 수 있다.

### [Link](https://reactrouter.com/en/main/components/link)

```tsx
function Header() {

return (
  <header>
    <nav>
      <ul>
        <li><Link to="/">Home</Link></li>
        <li><Link to="/about">About</Link></li>
      </ul>
    </nav>
  </header>
  );
}
```

- `Link`는 `NavLink`와 비슷하지만, 현재 경로와 일치하는 경우 특별한 스타일을 적용하지 않는다.
- `Link`는 `NavLink`보다 더 낮은 수준의 API이다.
- `preventScrollReset` prop을 사용하여 스크롤 위치를 유지할 수 있다.
- `replace` prop을 사용하여 `history.replace`를 사용할 수 있다.
- `state` prop을 사용하여 `history.state`를 설정할 수 있다.

### [Navigate](https://reactrouter.com/en/main/components/navigate)

```tsx
import {Navigate} from 'react-router-dom';

export default function LogoutPage() {
  return (
    <Navigate to='/'/>
  );
}
```

### [useNavigate](https://reactrouter.com/en/main/hooks/use-navigate)

```tsx
import { useNavigate } from 'react-router-dom';

export default function Header() {
  const navigate = useNavigate();

  const handleClickLogout = () => {
    navigate('/');
  };

  return (
    <header>
      <button type="button" onClick={handleClickLogout}>
        Logout
      </button>
    </header>
  );
}
```

- `useNavigate`는 `navigate` 함수를 반환한다.
- `navigate` 함수는 `history.push`와 비슷하다.
- route를 변경하고 싶을 때 사용한다.

### Navigation 테스트

```tsx
import {render, screen} from '@testing-library/react';
import {RouterProvider, createMemoryRouter} from 'react-router-dom';

import routes from './routes';

const context = describe;

describe('App', () => {
  function renderRouter(path: string) {
    const router = createMemoryRouter(routes, {initialEntries: [path]});
    render(<RouterProvider router={router}/>);
  }

  // ...

  context('"/logout" route', () => {
    it('HomePage로 redirect 한다.', () => {
      renderRouter('/logout');

      screen.getByText(/Home/);
    });
  });
});

```

- “ReferenceError: Request is not defined” 에러가 나면 “whatwg-fetch”를 임포트해서 해결할 수 있다.

## 마무리 글

이번에 React Router v6를 사용해보았다.  
강의를 보기전에는 아무생각 없이 router를 사용하고 있었지만, react-router-dom은 History API를 사용하고 있었다는걸 알았다.  
History API를 잘 사용하면 브라우저에서 url변경을 가로채서 커스텀하게 사용할 수 있고, 이를 통해 라우팅을 구현할 수 있다.  
SPA는 새로고침을 하지 않고 페이지를 이동하기 위해 라우팅을 구현해야 한다.  
이때 HistoryAPI를 사용하고 있는것 같다. (예전에는 hash를 사용해서 구현했다는것 같음)

- History API를 사용하면 SPA 라우팅을 구현할 수 있다.
- React Router는 History API를 사용한다.
- React Router v6는 많은 부분이 변경되었다. (RouterProvider, createBrowserRouter, createMemoryRouter, createMemoryRouter, etc..)
- Routes, Route를 사용할때보다 RouterProvider로 router를 별도로 관리하는게 더 편했다.
  - 별도 파일로 관리할때 배열로 관리하니까 더 직관적으로 느껴져서 좋았다.
  - 테스트 코드를 작성하기 더 좋아짐.
    - 기존에는 Routes, Route를 사용하는곳에서 했지만, RouterProvider로 분리하니까 router파일만 테스트하면 되서 더 좋았다.
- NavLink, Link, Navigate, useNavigate를 사용하면 라우팅을 쉽게 구현할 수 있다.
