---
description: Routes에 대해 알아보자
---

# [Routes](https://reactrouter.com/en/main/components/routes)

## 라우터란?

라우터는 경로에 따라 다른 컴포넌트를 렌더링하는 것을 말한다.  
예를 들어, `/` 경로에는 `Home` 컴포넌트를, `/about` 경로에는 `About` 컴포넌트를 렌더링하는 것이다.  
이러한 라우팅을 위해 `react-router-dom` 패키지를 사용한다.

## [React Router](https://reactrouter.com/en/main/start/overview)

React Router는 React에서 라우팅을 구현하기 위한 패키지이다.  
`react-router-dom` 패키지는 React Router의 DOM을 위한 구현체이다.  
React Router는 클라이언트 측 라우팅을 활성화 한다.

설치

```bash
npm install react-router-dom
```

### [Browser Router](https://reactrouter.com/en/main/router-components/browser-router)

`BrowserRouter`는 HTML5의 `history` API를 사용하여 클라이언트 측 라우팅을 활성화한다.
Route를 사용하기 위해서는 `BrowserRouter`로 감싸야 한다.

```tsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import {BrowserRouter} from 'react-router-dom';

import App from './App';

function main() {
  const element = document.getElementById('root');

  if (!element) {
    return;
  }

  const root = ReactDOM.createRoot(element);

  root.render((
    <React.StrictMode>
      <BrowserRouter>
        <App />
      </BrowserRouter>
    </React.StrictMode>
  ));
}

main();
```

### [Route](https://reactrouter.com/en/main/route/route#route)

`Route`는 경로에 따라 다른 컴포넌트를 렌더링한다.  
선언적으로 라우팅을 구현할 수 있다.

```tsx
import {Route, Routes} from 'react-router-dom';
import Header from './components/Header';
import Footer from './components/Footer';

import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

export default function App() {
  return (
    <div>
      <Header/>
      <main>
        <Routes>
          <Route path='/' element={<HomePage/>}/>
          <Route path='/about' element={<AboutPage/>}/>
        </Routes>
      </main>
      <Footer/>
    </div>
  );
}
```

- `path`: 경로
  - 동적 경로를 사용할 수 있다. `"/teams/:teamId"`와 같이 사용한다.
  - 선택적 경로를 사용할 수 있다. `"/teams/:teamId?"`와 같이 사용한다.
- `element`: 해당 경로에 렌더링할 컴포넌트
- `index`: `true`로 설정하면 해당 경로에 대한 라우팅이 기본으로 설정된다.
- `loader`: 렌더링되기 전에 호출되며 useLoaderData를 통해 요소에 대한 데이터를 제공한다.
- 등등 나머지들은 공식문서를 참고하자.

### [Memory Router](https://reactrouter.com/en/main/router-components/memory-router)

`MemoryRouter`는 메모리 내의 `history`를 사용하여 클라이언트 측 라우팅을 활성화한다.  
`MemoryRouter`는 주로 테스트에 사용된다.  

```tsx
import {render, screen} from '@testing-library/react';
import {MemoryRouter} from 'react-router-dom';

import App from './App';

const context = describe;

describe('App', () => {
  context('"/" route', () => {
    it('HomePage를 렌더링 한다.', () => {
      render((
        <MemoryRouter initialEntries={['/']}>
          <App/>
        </MemoryRouter>
      ));

      screen.getByText(/Home/);
    });
  });

  context('"/about" route', () => {
    it('AboutPage를 렌더링 한다.', () => {
      render((
        <MemoryRouter initialEntries={['/about']}>
          <App/>
        </MemoryRouter>
      ));

      screen.getByText(/this is Test/);
    });
  });
});
```

- `initialEntries`: 초기 경로를 설정한다.
  - 기본값은 `["/"]`이다.
- `initialIndex`: 초기 경로의 인덱스를 설정한다.
  - 기본값은 마지막 인덱스이다.
