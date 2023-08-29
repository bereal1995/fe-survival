---
description: Router에 대해 알아보자
---

# Router

## ReactRouter - RouterProvider

React Router v6.4부터는 `RouterProvider`를 지원한다.  
`RouterProvider`를 사용하면 라우팅 처리를 별도의 파일로 관리하기 더 편리하다.

- `RouterProvider`는 `BrowserRouter`와 `Routes`를 포함한다.
- 라우터 객체를 만들어 `RouterProvider`에 전달하여 사용한다.

### 사용법

App.tsx

```tsx
import {RouterProvider, createBrowserRouter} from 'react-router-dom';

import routes from './routes';

const router = createBrowserRouter(routes);

export default function App() {
  return (
    <RouterProvider router={router}/>
  );
}
```

routes.tsx

```tsx
import Layout from './components/Layout';

import AboutPage from './pages/AboutPage';
import HomePage from './pages/HomePage';

const routes = [
  {
    element: <Layout/>,
    children: [
      {path: '/', element: <HomePage/>},
      {path: '/about', element: <AboutPage/>},
    ],
  },
];

export default routes;
```

- routes.tsx에서 `Layout` 컴포넌트를 `element`로 사용하고 있다.
- `Layout` 컴포넌트는 `children`을 통해 `HomePage`과 `AboutPage`를 렌더링한다.
  - `<Layout>{children}</Layout>` 형태로 렌더링한다고 생각하면 된다.

### 테스트 코드

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

  context('"/" route', () => {
    it('HomePage를 렌더링 한다.', () => {
      renderRouter('/');

      screen.getByText(/Home/);
    });
  });

  context('"/about" route', () => {
    it('AboutPage를 렌더링 한다.', () => {
      renderRouter('/about');

      screen.getByText(/this is Test/);
    });
  });
});
```

- createMemoryRouter를 사용하여 테스트에 필요한 라우터 객체를 생성한다.
- `initialEntries`를 통해 초기 경로를 설정하여 원하는 경로로 테스트를 진행할 수 있다.
