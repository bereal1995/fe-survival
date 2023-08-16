---
description: MSW에 대해 알아보자
---

# MSW

## Service worker

브라우저 및 네트워크의 중간에 위치하여 브라우저와 서버 사이의 통신을 제어하는 브라우저 내 백그라운드 프로세스, 프록시 서버와 같은 역할을 한다.  
웹 페이지와 별개로 동작하며, 브라우저가 종료되어도 계속해서 백그라운드에서 동작한다.

- 브라우저와 서버 사이의 통신을 제어
- 브라우저의 캐시를 제어
  - fetch 요청을 가로채서 캐시된 데이터를 반환
  - 만약 캐시가 삭제되지 않았다면 인터넷에 연결되지 않아도 캐시된 데이터를 반환
- 푸시 알림과 같은 기능을 제공
  - 백그라운드에서 동작하므로 푸시 알림을 보낼 수 있다.
- 백그라운드 동기화
  - 작업 도중 네트워크가 끊어진 경우, 네트워크가 다시 연결되면 백그라운드에서 동기화를 수행한다.

## MSW(Mock Service Worker)

브라우저의 Service Worker를 가로채서 가짜 응답을 반환하는 방식으로 테스트를 진행할 수 있게 해주는 라이브러리  
테스트 혹은 개발 환경에서만 사용되며, 실제 서비스에는 영향을 주지 않는다.

### MSW 설치

```bash
npm install msw --save-dev
```

### MSW 설정

```ts
// jest.config.js
moduel.exports = {
  // ...
  setupFilesAfterEnv: [
    '@testing-library/jest-dom/extend-expect',
    '<rootDir>/src/setupTests.ts',
  ],
  // ...
};

// src/setupTests.ts
import 'whatwg-fetch';
import server from './mocks/server';

beforeAll(() => {
  server.listen({onUnhandledRequest: 'error'});
});

afterAll(() => {
  server.close();
});

afterEach(() => {
  server.resetHandlers();
});


// src/mocks/handlers.ts
import {rest} from 'msw';
import fixtures from '../../fixtures';

// eslint-disable-next-line @typescript-eslint/naming-convention
const BASE_URL = process.env.API_BASE_URL ?? 'http://localhost:3000';

const handlers = [
  rest.get(`${BASE_URL}/products`, async (req, res, ctx) => {
    const {products} = fixtures;

    return res(
      ctx.status(200),
      ctx.json({products}),
    );
  }),
];

export default handlers;


// src/mocks/server.ts
import {setupServer} from 'msw/node';

import handlers from './handlers';

const server = setupServer(...handlers);

export default server;
```

- 이렇게 설정하면 `http://localhost:3000/products`로 요청이 들어오면 `src/mocks/handlers.ts`에 정의된 핸들러가 동작한다.
- `server.listen()`을 통해 서버를 시작하고, `server.close()`를 통해 서버를 종료한다.
- `server.resetHandlers()`를 통해 핸들러를 초기화한다.
- `server.use()`를 통해 핸들러를 추가할 수 있다.
- node버전이 낮으면 fetch를 사용할 수 없으므로 `whatwg-fetch`를 설치해야 한다.

## polyfill(폴리필)

- 브라우저가 지원하지 않는 기능을 지원하도록 하는 코드
- 브라우저마다 지원하는 기능이 다르므로, 브라우저마다 다른 코드를 작성해야 한다.
- babel, swc 등의 트랜스파일러를 통해 폴리필을 적용할 수 있다.
  - babel은 폴리필을 적용할 때, 전역 객체를 오염시킨다.
  - swc는 폴리필을 적용할 때, 전역 객체를 오염시키지 않는다.

### [whatwg-fetch](https://github.com/JakeChampion/fetch)

github에서 만든 fetch 폴리필 라이브러리  
fetch를 사용할 수 없는 브라우저에서 fetch를 사용할 수 있도록 해준다.

### whatwg-fetch 설치 및 사용

```bash
npm install whatwg-fetch --save-dev
```

```ts
// src/setupTests.ts
import 'whatwg-fetch';
```

- `whatwg-fetch`를 설치하고, `src/setupTests.ts`에 import한다.
- setupTests.ts에 import하면 jest가 실행될 때마다 import된 코드가 실행된다.
