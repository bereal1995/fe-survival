---
description: styled-components에 대해 알아보자
---

# [styled-components](https://styled-components.com/)

component처럼 선언적으로 스타일을 적용할 수 있게 해주는 라이브러리  
styled-components는 [emotion](https://emotion.sh/docs/introduction)과 비슷한 라이브러리이다.

## 설치

```bash
npm install --save styled-components

# typescript
npm install --save @types/styled-components

# swc
npm install --save-dev @swc/plugin-styled-components
```

swc를 사용하는 경우에는 `.swcrc` 파일에 플러그인을 추가해주어야 한다.

```json
{
  "jsc": {
    "experimental": {
      "plugins": [
        [
          "@swc/plugin-styled-components",
          {
            "displayName": true,
            "ssr": true
          }
        ]
      ]
    }
  }
}
```

## 사용

```tsx
import styled from 'styled-components';

const Paragraph = styled.p`
  color: #00F;

  strong {
    font-size: 2em;
    color: #F00;
  }
`;
const BigParagraph = styled(Paragraph)`
  font-size: 2em;

  strong {
    font-size: 1.5em;
  }
`;

function HelloWorld({className}: React.HTMLAttributes<HTMLElement>) {
  return (
    <BigParagraph className={className}>
      Hello, world
      <strong>!</strong>
    </BigParagraph>
  );
}

const SmallHelloWorld = styled(HelloWorld)`
  font-size: .1em;
`;

export default function Greeting() {
  return (
    <SmallHelloWorld/>
  );
}

```

- `styled` 함수를 사용하여 스타일을 적용할 컴포넌트를 만든다.
- `styled` 함수의 인자로는 태그명, 컴포넌트, 또는 다른 `styled` 함수를 사용할 수 있다.
- `styled` 함수의 반환값은 스타일이 적용된 컴포넌트이다.
- `styled` 함수의 인자로 컴포넌트를 사용하는 경우, 컴포넌트의 `className` 속성을 사용하여 스타일을 적용한다.
