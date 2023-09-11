---
description: Global Style & Theme에 대해 알아보자
---

# Global Style & Theme

## [Reset CSS](https://meyerweb.com/eric/tools/css/reset/)

Reset CSS는 브라우저마다 기본적으로 제공하는 스타일을 초기화하는 CSS이다.

### [styled-reset](https://github.com/zacanger/styled-reset)

styled-reset은 styled-components에서 사용할 수 있는 Reset CSS이다.

```bash
npm install styled-reset
```

```tsx
import {Reset} from 'styled-reset';

import Greeting from './components/Greeting';
import Switch from './components/Switch';

export default function App() {
  return (
    <div>
      <Reset />
      <Greeting/>
      <Switch/>
    </div>
  );
}

```

### [Global Style](https://styled-components.com/docs/api#createglobalstyle)

Global Style은 styled-components에서 전역으로 적용할 수 있는 스타일이다.

```tsx
import {createGlobalStyle} from 'styled-components';

const GlobalStyle = createGlobalStyle`
  body {
    background: #FFF;
    color: #000;
  }
`;

import Greeting from './components/Greeting';

export default function App() {
  return (
    <div>
      <GlobalStyle />
      <Greeting/>
    </div>
  );
}

```

- Global Style은 보통 전역 스타일을 지정할 때 사용한다.
  - 예를 들어, `body`의 스타일이나, box-sizing, [font-size-trick](https://www.aleksandrhovhannisyan.com/blog/62-5-percent-font-size-trick/), [word-break](https://twitter.com/keepallvillain) 등을 지정할 때 사용한다.
- Global Style은 컴포넌트처럼 사용할 수 있다.

## [`box-sizing`](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing) 속성

- `box-sizing` 속성은 요소의 크기를 계산하는 방법을 지정한다.
- `box-sizing` 속성의 값은 `content-box`, `border-box`, `padding-box`가 있다.
  - `content-box`: 요소의 크기는 콘텐츠 영역의 크기이다.
  - `border-box`: 요소의 크기는 콘텐츠 영역의 크기 + padding + border의 크기이다.
  - `padding-box`: 요소의 크기는 콘텐츠 영역의 크기 + padding의 크기이다.
- `box-sizing` 속성의 기본값은 `content-box`이다.

## [`word-break`](https://developer.mozilla.org/ko/docs/Web/CSS/word-break) 속성

- `word-break` 속성은 단어가 줄 바꿈 되는 방법을 지정한다.
- `word-break` 속성의 값은 `normal`, `break-all`, `keep-all`, `break-word`가 있다.
  - `normal`: 단어가 줄 바꿈 되지 않는다.
  - `break-all`: 단어가 줄 바꿈 된다.
  - `keep-all`: 단어가 줄 바꿈 되지 않는다. 단어가 줄 바꿈 되지 않는다.
  - `break-word`: 단어가 줄 바꿈 된다.
- `word-break` 속성의 기본값은 `normal`이다.

## [Theme](https://styled-components.com/docs/advanced#theming)

Theme은 styled-components에서 전역으로 사용할 수 있는 변수이다.  
ThemeProvider에 props에 Theme을 전달하면, ThemeProvider의 자식 컴포넌트에서 Theme을 사용할 수 있다.

```tsx
const defaultTheme = {
  colors: {
    background: '#FFF',
    text: '#000',
    primary: '#F00',
    secondary: '#00F',
  },
};

export default defaultTheme;
```

### [타입 문제](https://styled-components.com/docs/api#create-a-declarations-file)

styled-components에 theme을 typescript환경에서 사용할 때, 타입 추론이 되지 않아 타입 문제가 발생한다.  
이때, `styled-components.d.ts` 파일을 생성하여 타입 문제를 해결할 수 있다.

```tsx
// Theme.ts
import type defaultTheme from './defaultTheme';

type Theme = typeof defaultTheme;

export default Theme;

// styled-components.d.ts
/* eslint-disable @typescript-eslint/consistent-type-definitions */
import 'styled-components';
import type Theme from './Theme';

declare module 'styled-components' {
  export interface DefaultTheme extends Theme { }
}

// GlobalStyle.ts
const GlobalStyle = createGlobalStyle`
  // ...

  body {
    font-size: 1.6rem;
    background: ${props => props.theme.colors.background}; // 타입 추론이 된다.
    color: ${props => props.theme.colors.text}; // 타입 추론이 된다.
  }

  // ...
`;
```

## ThemeProvider

ThemeProvider는 styled-components에서 전역으로 사용할 수 있는 Theme을 설정할 수 있는 컴포넌트이다.

```tsx
import {Reset} from 'styled-reset';
import {ThemeProvider} from 'styled-components';
import {useDarkMode} from 'usehooks-ts';

import GlobalStyle from './styles/GlobalStyle';
import defaultTheme from './styles/defaultTheme';
import darkTheme from './styles/darkTheme';

import Greeting from './components/Greeting';
import Switch from './components/Switch';
import Button from './components/Button';

export default function App() {
  const {isDarkMode, toggle} = useDarkMode();
  const theme = isDarkMode ? darkTheme : defaultTheme;
  return (
    <ThemeProvider theme={theme}>
      <div>
        <Reset />
        <GlobalStyle />
        <Button
          onClick={toggle}
          active={isDarkMode}
        >
          Toggle DarkMode
        </Button>
        <Greeting/>
        <Switch/>
      </div>
    </ThemeProvider>
  );
}
```

- ThemeProvider의 props에 Theme을 전달하면, ThemeProvider의 자식 컴포넌트에서 Theme을 사용할 수 있다.
- Theme이 변경되면, ThemeProvider의 자식 컴포넌트에서 Theme이 변경된다.

### [Jest 테스트에서 "window.matchMedia is not a function" 에러가 발생하는 경우](https://jestjs.io/docs/manual-mocks#mocking-methods-which-are-not-implemented-in-jsdom)

아래 코드를 setupTests.ts에 추가한다.

```tsx
Object.defineProperty(window, 'matchMedia', {
  writable: true,
  value: jest.fn().mockImplementation(query => ({
    matches: false,
    media: query,
    onchange: null,
    addListener: jest.fn(), // deprecated
    removeListener: jest.fn(), // deprecated
    addEventListener: jest.fn(),
    removeEventListener: jest.fn(),
    dispatchEvent: jest.fn(),
  })),
});
```
