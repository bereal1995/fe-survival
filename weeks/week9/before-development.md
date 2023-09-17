# 개발하기 전 준비

## 개발 환경 세팅

개발환경은 생존 코스에서 제공해주는 것을 사용했다.  
구체적인 세팅 내용은 생존코스에서 제공하는 곳에서 확인하자.

### 사용되는 라이브러리

1. [React Router](https://github.com/remix-run/react-router)
2. [styled-components](https://github.com/styled-components/styled-components)
3. [styled-reset](https://github.com/zacanger/styled-reset)
4. [usehooks-ts](https://github.com/juliencrn/usehooks-ts)
5. [Axios](https://github.com/axios/axios): REST API 사용을 위한 HTTP 클라이언트.
6. [tsyringe](https://github.com/microsoft/tsyringe)
7. [reflect-metadata](https://github.com/rbuckton/reflect-metadata)
8. [usestore-ts](https://github.com/seed2whale/usestore-ts)
9. [jest-dom](https://github.com/testing-library/jest-dom): React Testing Library에서 활용할 수 있는 DOM 확인용 Matcher 모음.
10. [MSW](https://github.com/mswjs/msw)

### E2E Test

CodeceptJS를 사용

### Style

themeProvider와 globalStyle을 사용하여 기본 설정

### Routes

createBrowserRouter를 사용하여 라우팅 설정

### Test Helper

styled-components의 Theme과 React Router 등을 사용할 때 테스트 코드를 작성하기 위해서는 ThemeProvider, MemoryRouter와 같은 것들로 컴포넌트를 감싸 주어여야 하는데 이를 테스트 코드마다 작성하는 것은 번거롭기 때문에 Test Helper를 사용하여 테스트 코드를 작성한다.

```tsx
export function render(
  element: React.ReactElement,
  { path = '/' }: Option = {},
) {
  return originalRender((
    <MemoryRouter initialEntries={[path]}>
      <ThemeProvider theme={defaultTheme}>
        {element}
      </ThemeProvider>
    </MemoryRouter>
  ));
}
```

### MSW 세팅

fixtures를 만들어서 MSW 모킹할때나 테스트 코드에서 사용할 수 있도록 세팅